---
description: Creating more heap space
---

# The Top Chunk and Remainder

Also known as the **wilderness**, the **top chunk** is the final chunk in the heap. The size of the top chunk is equal to the size of the free heap space.

\[TODO image here]

If a new chunk is allocated and there are no free chunks suitable, the top chunk shrinks and is pushed back to make space for the new heap. The use of the top is triggered [here](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L3983), and the actual logic can be found [here](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L4069):

```c
victim = av->top;
size = chunksize (victim);

if ((unsigned long) (size) >= (unsigned long) (nb + MINSIZE))
  {
    remainder_size = size - nb;
    remainder = chunk_at_offset (victim, nb);
    av->top = remainder;
    set_head (victim, nb | PREV_INUSE |
              (av != &main_arena ? NON_MAIN_ARENA : 0));
    set_head (remainder, remainder_size | PREV_INUSE);

    check_malloced_chunk (av, victim, nb);
    void *p = chunk2mem (victim);
    alloc_perturb (p, bytes);
    return p;
  }
```

If the `size` of the requested chunk is less than or equal to the size of the top chunk, it is broken into two chunks - the return chunk (located where the top chunk was previously) and the **remainder** chunk, which is the new top chunk with a reduced size.

If the size is greater than the top chunk can handle, glibc attempts to consolidate fastbins. If there are no fastbins (or there's still not enough space), we [default to `sysmalloc`](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L4120), which calls [`mmap`](https://elixir.bootlin.com/glibc/glibc-2.26/source/malloc/malloc.c#L2312) (on systems that have it).
