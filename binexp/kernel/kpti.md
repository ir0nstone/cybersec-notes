---
description: Kernel Page Table Isolation
---

# KPTI

[KPTI ](https://www.kernel.org/doc/html/next/x86/pti.html)is designed to protect against attacks that abuse the shared user/kernel address space. Originally called KAISER, it is a mitigation originally created to prevent [Meltdown](https://meltdownattack.com/)-style microarchitectural vulnerabilities.

KPTI separates the page tables for user space and kernel space, creating two sets.

* The first set, used by the kernel, includes a complete mapping of user space that the kernel can use for things like `copy_to_user()`. This page table has the NX bit set.&#x20;
* The user set maps the minimum amount of kernel virtual memory possible (e.g. exception handlers and code required for the user to transition to the kernel).

TODO explain why it's important + KPTI trampolines.

