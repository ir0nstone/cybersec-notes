---
description: Kernel Page Table Isolation
---

# KPTI

[KPTI ](https://www.kernel.org/doc/html/next/x86/pti.html)is designed to protect against attacks that abuse the shared user/kernel address space. Originally called KAISER, it is a mitigation originally created to prevent [Meltdown](https://meltdownattack.com/)-style microarchitectural vulnerabilities.

KPTI separates the page tables for user space and kernel space, creating two sets.

* The first set, used by the kernel, includes a complete mapping of user space that the kernel can use for things like `copy_to_user()`. This page table has the NX bit set for userspace memory.
* The user set maps the minimum amount of kernel virtual memory possible (e.g. exception handlers and code required for the user to transition to the kernel).

You can disable KPTI from the command line via the `nopti` argument. It is also automatically disabled if the CPU is not affected by meltdown.

### Consequences and Bypasses

When in the user context, the kernel is not fully mapped. This doesn't affect most of our exploits, since they are executed in kernel mode.

However, when in kernel mode, userspace is mapped as non-executable. This means that we can't return to an `escalate()` function [we defined](../../types/kernel/kernel-rop-ret2usr.md) via `iretq`. The solution to this is to swap page tables back to user ones.

To achieve this, we can abuse a function of use that is descriptively called `swapgs_restore_regs_and_return_to_usermode`. Disassembling it (TODO!), we see that is starts with a load of `pop` instructions before a few `mov` and `push` and then a page table switch and a `swapgs` and `iretq`. We can jump to _after_ the `pop` instructions to avoid having to fill those in. This is commonly called a **KPTI Trampoline**.

TODO example

### Bypassing KPTI via a SIGSEGV Handler

Trying to return to user mode via `iretq` without switching page tables results in a SIGSEGV rather than a kernel crash, because we are in userspace.

An alternative method is therefore to use a SIGSEGV handler - the exploit gets root privileges, then tries to access userland and triggers a SIGSEGV. The kernel fault handler with switch the page tables for us when dispatching to the handler! A good example can be found [here](https://trungnguyen1909.github.io/blog/post/matesctf/KSMASH/).

TODO example
