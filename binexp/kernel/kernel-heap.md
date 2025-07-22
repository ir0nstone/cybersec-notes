---
description: The pain of it all
---

# Kernel Heap

Historically, the Linux kernel has had three main heap allocators: SLOB, SLAB and SLUB.

SLUB is the latest version, replacing SLAB as of [version 2.6.23](https://archive.ph/20130415043346/http://git.kernel.org/?p=linux/kernel/git/torvalds/linux-2.6.git;a=commit;h=a0acd820807680d2ccc4ef3448387fcdbf152c73). SLOB was used as the backup to SLAB and SLUB - but was removed in [version 6.4](https://lwn.net/Articles/936132/). As a result, SLUB is all we really have to care about (even pre-6.4, SLOB was practically never used). **From here on out, we will only talk about SLUB, unless explicitly stated**.

Note that, confusingly, chunks in the kernel heap are called _slabs_.

## Caches

Unlike the glibc heap, SLUB has fixed sizes for slabs, which are powers of 2 up to **8192** along with **96** and **192**. These are conveniently called `kmalloc-8`, `kmalloc-16`, `kmalloc-32` <sub>,</sub> `kmalloc-64`, `kmalloc-96`, `kmalloc-128`, `kmalloc-192`, `kmalloc-256`, `kmalloc-512`, `kmalloc-1k`, `kmalloc-2k`, `kmalloc-4k` and `kmalloc-8k`.

Each cache is assigned its own area of memory, with at least one page assigned to each caches (and guard pages between each cache, too, I believe?).

If the kernel wants to allocate space in the heap, it will call `kmalloc`  and pass it the size (and some flags). The size will be rounded up to fit in the smallest possible cache, then assigned there. Anything larger than 8192 bytes will not use `kmalloc` at all, and uses `page_alloc` instead.

This approach is a **massive** performance improvement. It can also make exploitation primitives harder, as every slab is the same size and it's harder TODO

### New Page Creation

Since caches are allocated on a page-by-page basis, we can get to a point where we have so many slabs of that size that they take up an entire page (for example, four `kmalloc-1k` slabs would fill up a page, as a page is 0x1000 bytes in size). In this case, a new page is created. This page does create just the one slab - it will create **four** slabs. Why? Because the kernel knows that this page is **only** used for `kmalloc-1k` slabs, and there can be a maximum of four on one page. It therefore creates four slabs immediately, and marks the remaining three as free.

These remaining three are saved in the freelist in a **random order**, provided that the configuration  `CONFIG_SLAB_FREELIST_RANDOM` is enabled (which it is by default).

## The Kernel Heap is Global

One major difference between user- and kernel-mode heap exploitation is that the kernel heap is shared between **all kernel processes**. Kernel modules and every other aspect of the kernel _use the same heap_.

So, let's say you find some sort of kernel heap primitive - an overflow, for example. Overflowing into identical slabs might not be helpful, but in the kernel, we can find common structs with powerful primitives that we can use to our advantage. Imagine that there is a struct that contains a function pointer, and you can trigger a call to this function. If this struct is allocated to the same cache as the slab you can overflow, it is possible to allocate this struct such that it inhabits the slab located directly behind in memory. Suddenly the overflow is incredibly powerful, and can lead immediately to something like a ret2usr.
