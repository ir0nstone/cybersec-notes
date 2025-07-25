---
description: Exploiting the wilderness
---

# The House of Force

<table data-header-hidden><thead><tr><th width="283"></th><th></th></tr></thead><tbody><tr><td>Glibc Version</td><td>*-<a href="https://elixir.bootlin.com/glibc/glibc-2.29/source/malloc/malloc.c#L4114">2.29</a></td></tr><tr><td>Primitive Required</td><td><ul><li>Heap overflow into the top chunk</li><li>Chunk allocation of arbitrary size</li></ul></td></tr><tr><td>Primitive Gained</td><td><ul><li>Arbitrary Write</li></ul></td></tr></tbody></table>

In the House of Force, we overflow the `size` field of the [top chunk](../the-top-chunk-and-remainder.md) with a huge value. We next allocate a **huge** chunk size. Due to the size overwrite, we bypass the [top size check](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L4088):

```c
if ((unsigned long) (size) >= (unsigned long) (nb + MINSIZE))
```

Because the check is passed, the we trick glibc into allocating the large request to the heap rather than use `mmap()`. This gives us a lot of control over the **remainder chunk**:

```c
remainder_size = size - nb;
remainder = chunk_at_offset (victim, nb);
av->top = remainder;
```

Note that if we can control the allocation size (the `nb` variable here), we pass that size to [`chunk_at_offset()`](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L1300):

```c
/* Treat space at ptr + offset as a chunk */
#define chunk_at_offset(p, s)  ((mchunkptr) (((char *) (p)) + (s)))
```

This macro takes the address and adds `nb` onto it - but because we have control over `nb`, we can **control where the remainder chunk is placed**, and therefore **where the next top chunk is located**. This means that the **next** allocation can be located at an address of our choice!

{% hint style="info" %}
Note that we can even write to addresses **ahead** of the heap in memory by triggering an **integer overflow**!
{% endhint %}

&#x20;TODO mathematics

TODO source

TODO patch

## The Patch

In glibc 2.29, there is a [new security check](https://elixir.bootlin.com/glibc/glibc-2.29/source/malloc/malloc.c#L4114) to protect against the House of Force:

```c
if (__glibc_unlikely (size > av->system_mem))
    malloc_printerr ("malloc(): corrupted top size");
```

Very simple - check if the size is ridiculously large, and throw an error if so.
