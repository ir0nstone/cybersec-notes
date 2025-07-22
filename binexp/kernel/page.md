---
description: The pain of it all
---

# Kernel Heap

Historically, the Linux kernel has had three main heap allocators: SLOB, SLAB and SLUB.

SLUB is the latest version, replacing SLAB as of [version 2.6.23](https://archive.ph/20130415043346/http://git.kernel.org/?p=linux/kernel/git/torvalds/linux-2.6.git;a=commit;h=a0acd820807680d2ccc4ef3448387fcdbf152c73). SLOB was used as the backup to SLAB and SLUB, but was removed in [version 6.4](https://lwn.net/Articles/936132/). As a result, SLUB is all we really have to care about (even pre-6.4, SLOB was practically never used). **From here on out, we will only talk about SLUB, unless explicitly stated**.

Note that, confusingly, "chunks" in the kernel heap are called _objects_ and they are stored in _slabs_.

## Slabs and Caches

Unlike the glibc heap, SLUB has fixed sizes for objects, which are powers of 2 up to **8192** along with **96** and **192**. These are conveniently called `kmalloc-8`, `kmalloc-16`, `kmalloc-32` <sub>,</sub> `kmalloc-64`, `kmalloc-96`, `kmalloc-128`, `kmalloc-192`, `kmalloc-256`, `kmalloc-512`, `kmalloc-1k`, `kmalloc-2k`, `kmalloc-4k` and `kmalloc-8k`. We call these individual classifications **caches**, and they are comprised of **slabs**.

Each **slab** is assigned its own area of memory and comprised of 1 or more continuous pages. If the kernel wants to allocate space in the heap, it will call `kmalloc`  and pass it the size (and some flags). The size will be rounded up to fit in the smallest possible cache, then assigned there. Anything larger than 8192 bytes will not use `kmalloc` at all, and uses `page_alloc` instead.

This approach is a **massive** performance improvement. It can also make exploitation primitives harder, as every object is the same size and it's harder to overlap. Similarly, because the sizes are determined by the cache rather than metadata, we cannot fake size.

### Slab Creation

We can get to a point where we have so many objects in a cache that they fill all of the slabs. In this case, a new slab is created. This slab does not create the singular object - it will create **multiple** objects. Why? Because the kernel knows that this slab is **only** used for `kmalloc-1k` objects, it creates all possible objects immediately and marks the remaining as free.

These remaining three are saved in the freelist in a **random order**, provided that the configuration  `CONFIG_SLAB_FREELIST_RANDOM` is enabled (which it is by default).

The default size of slabs depends on the cache it is being used for. You can read `/proc/slabinfo` to see the current configuration for the system:

```
$ sudo cat /proc/slabinfo
# name            <active_objs> <num_objs> <objsize> <objperslab> <pagesperslab> [...]
[...]
kmalloc-8k            80            80        8192          4            8
kmalloc-4k           208           208        4096          8            8
kmalloc-2k           768           768        2048         16            8
kmalloc-1k          1296          1296        1024         16            4
kmalloc-512         2190          2224         512         16            2
kmalloc-256         1917          1936         256         16            1
kmalloc-128         1024          1024         128         32            1
kmalloc-64          7532          7936          64         64            1
kmalloc-32          6442          6528          32        128            1
kmalloc-16         10123         10240          16        256            1
kmalloc-8           5120          5120           8        512            1
kmalloc-192         3885          3885         192         21            1
kmalloc-96          3506          4158          96         42            1
```

Here `objsize` is the size of each element in the cache, and `objsperslab` is the number of objects created at once when a new slab is initialized. Then `pagesperslab` is the product of `objsize/0x1000` (pages per object) and `objperslab`, and tells you how many pages each slab has.

TODO `CONFIG_SLAB_FREELIST_HARDENED`.

## The Kernel Heap is Global

One major difference between user- and kernel-mode heap exploitation is that the kernel heap is shared between **all kernel processes**. Kernel modules and every other aspect of the kernel _use the same heap_.

So, let's say you find some sort of kernel heap primitive - an overflow, for example. Overflowing into identical objects might not be helpful, but in the kernel, we can find common structs with powerful primitives that we can use to our advantage. Imagine that there is a struct that contains a function pointer, and you can trigger a call to this function. If this struct is allocated to the same cache as the object you can overflow, it is possible to allocate this struct such that it inhabits the object located directly behind in memory. Suddenly the overflow is incredibly powerful, and can lead immediately to something like a ret2usr.
