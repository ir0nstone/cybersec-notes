# Operations of the Other Bins

When a non-fast chunk is freed, it gets put into the Unsorted Bin. When new chunks are **requested**, glibc looks at all of the bins

* If the requested size is fastbin size, [check the corresponding fastbin](https://elixir.bootlin.com/glibc/glibc-2.3.6/source/malloc/malloc.c#L3849)
  * If there is a chunk in it, return it
* If the requested chunk is of smallbin size, [check the corresponding smallbin](https://elixir.bootlin.com/glibc/glibc-2.3.6/source/malloc/malloc.c#L3868)
  * If there is a chunk in it, return it
* If the requested chunk is large (of largebin size), [we first consolidate the largebins](https://elixir.bootlin.com/glibc/glibc-2.3.6/source/malloc/malloc.c#L3897) with [`malloc_consolidate()`](../malloc_consolidate.md). We will get into the mechanisms of this at a later point, but essentially I lied earlier - fastbins **do** consolidate, but not on freeing!
* Finally, we iterate through the chunks in the unsorted bin
  * If it is empty, we service the request through making the heap larger by [splitting the top chunk](../the-top-chunk-and-remainder.md), pushing the now-smaller top chunk to start at a higher memory address
* If the requested size is equal to the size of the chunk in the bin, return the chunk
* If it's smaller, split the chunk in the bin in two and return a portion of the correct size
* If it's larger, we [consolidate the fastbins](https://elixir.bootlin.com/glibc/glibc-2.3.6/source/malloc/malloc.c#L3897), and after this there may be free chunks big enough to service the request
  * If not, we again service the request by [splitting the top chunk](../the-top-chunk-and-remainder.md) and pushing the now-smaller top chunk to start at a higher memory address

One thing that is very easy to forget is what happens on _allocation_ and what happens on _freeing_, as it can be a bit counter-intuitive. For example, the fastbin consolidation is triggered from an allocation!
