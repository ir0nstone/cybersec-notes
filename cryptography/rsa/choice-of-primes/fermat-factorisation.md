---
description: Used when p and q are numericaly close
---

# Fermat Factorisation

If $$p$$ and $$q$$ are numerically close, we can use [Fermat Factorisation](https://en.wikipedia.org/wiki/Fermat's\_factorization\_method).

During Fermat Factorisation, we hope to find $$a$$ and $$b$$ such that

$$
a^2 - b^2 = N
$$

Because that then means we can factorise the left-hand expression into

$$
(a+b)(a-b)=N
$$

As thus we get the two factors of $$N$$ as $$(a+b)$$ and $$(a-b)$$.

The reason we use this when $$p$$ and $q$ are numerically close is because the closer they are to each other the closer they are to $$\sqrt{N}$$. If we say $$a=\sqrt{N}$$ rounded up to the nearest number, we can calculate $$b^2 = a^2-N$$ (as rearranged from before) until $$b$$ is a whole number, after which we've solved the equation.

An example of this attack can be found in [this writeup](https://www.notion.so/Factorize-b96056dc70f54cc7b42b32f8984cb7cf), which may make it a bit clearer.
