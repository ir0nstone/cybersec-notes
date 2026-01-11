# Virtual Addresses and Virtual Memory

If we disable ASLR and run two programs side-by-side, we might notice that the libc is loaded into the same address. Contrary to what you might think, these programs are **not** sharing the same instance of libc!

In fact, even with ASLR off, we can run two different programs and we might still notice that they are loaded into broadly the same part of memory.

The reason for this is is that the addresses we see in a debugger are **virtual addresses**.

## Overview

When a program and its libraries are started up, they are loaded into a [Virtual Address Space](https://en.wikipedia.org/wiki/Virtual_address_space) (VAS). Addresses in the VAS are then **mapped** to **real, physical locations** in RAM!

This means that if you have two separate programs both loaded at `0x5655523fa000`, this address actually corresponds to two **different locations in RAM**. The OS handles the translation by using the processor's [Memory Management Unit](https://en.wikipedia.org/wiki/Memory_management_unit) (MMU), and the actual memory location differs **for each process**.

In fact, as the OS is what handles the mapping from virtual addresses to physical addresses, the executable itself only sees virtual addresses! So when pointers are printed out and display virtual addresses, that is not the program handling a layer of abstraction away from you, it genuinely treats pointers in that way. The abstraction is another layer deep.

## The Kernel

The kernel only has **one** continguous virtual address space, so all processes running in kernel mode can see one another (and all user mode programs as well!). In fact, the reason programs are loaded in lower addresses is that in Linux the higher addresses are reserved for the kernel. This is why, [later on](../kernel/), you will see drivers loaded at addresses starting with `0xffff...`

## Benefits

Virtual addressing has three main benefits.

#### Contiguous Memory Allocation

While physical RAM may not have a sufficient chunk of contiguous memory for a program, virtual addressing allows us to _pretend_ as if it does, loading large programs into what seems to be one large chunk. The corresponding physical addresses, of course, could really be spread out all over memory. Virtual addresses mean we do not have to worry about such problems.

#### Strict Process Isolation

Processes cannot interfere with the address space of another process, creating a stronger security boundary.

#### Allocate More Memory than we have RAM

When the physical memory (RAM) is filled up, the OS will move inactive pages in memory to the [swap space](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-swapspace), which is located on the **hard drive**. The idea is that, in times of large strain, the hard drive will act as a sort of RAM overflow. Swapped memory is much, much slower than RAM, which is why inactive memory pages are the ones moved. This allows the Operating System to handle low memory gracefully and without crashing. Virtual addressing allows us to not think about this happening, as the address translation done by the OS via the MMU will automatically map the corresponding virtual addresses to hard drive addresses.
