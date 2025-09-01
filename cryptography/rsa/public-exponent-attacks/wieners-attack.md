---
description: Using Continued Fractions to attack large e values
---

# Wiener's Attack

## Overview

Wiener's Attack utilises the convergents of the continued fraction expansion of $$\frac{k}{d}$$ to attempt to guess the decryption exponent $$d$$ when $$e$$ is large, as $$d$$ is necessarily small as a result.

## Introduction

We can say that

$$
\phi(N) = (p-1)(q-1) \\
= pq - (p + q) + 1 \\
\approx N
$$

​We can also say that since $$ed \equiv 1 \mod \phi(N)$$​, we can rearrange this to say that $$ed = k\phi(N) + 1$$.

$$
d = k\phi(N) + 1 \\
ed - k\phi(N) = 1 \\
\frac{e}{\phi(N)} - \frac{k}{d} = \frac{1}{d\phi(N)}
$$

Now since $$d\phi(N)$$ is likely to be huge, we can say $$\frac{1}{d\phi(N)}$$ is almost zero, and also use the approximation from before to say that

$$
\frac{e}{N} \approx \frac{k}{d}
$$

Note that $$\frac{e}{N}$$ is composed of **entirely public information**, meaning we have access to an approximation to $$\frac{k}{d}$$, which is **entirely private information**.

## Determining the Private Information

We can represent $$\frac{e}{N}$$ as a continued fraction. If we go through the convergents of this fraction, we may come across $$\frac{k}{d}$$ since $$\frac{e}{N} \approx \frac{k}{d}$$ ($$\frac{k}{d}$$ is a good approximation). This is more likely to work whendis smaller due to **Legendre’s Theorem in Diophantine Approximations** (TODO prove this), specifically when $$d<\frac{1}{3}N^\frac{1}{4}$$.

Once we list the convergents, we iterate through and there are a few checks we can make to determine whether or not it's the correct convergent:

* As $$ed \equiv 1 \mod \phi(N)$$ and $$\phi(N)$$ is even, $$d$$ must be odd, so we can discard convergences with even denominators.
* Since $$\phi(N)$$ must be a whole number, we can compute $$\frac{ed - 1}{k}$$ and see if it returns an integer or not - if not, we can discard the convergent.

Once we find a convergent we don't discard, we can assume it's $$\phi(N)$$ and attempt to calculate $$d$$ with that, seeing if the resultant private key yields a valid result or not. This can take a **lot** of decryption attempts to work successfully however - we can speed up the "checking" process using a quadratic equation.

### Solving for p, q

If we say $$N=pq$$, it follows that:

$$
\phi(N) = (p-1)(q-1) \\
\phi(N) = N - (p + q) + 1 \\
p + q = N - \phi(N) + 1
$$

If we now consider a quadratic equation $$(x-p)(x-q) = 0$$, with the roots $$p$$ and $$q$$ being the prime factors of $$N$$, we can expand this and substitute:

$$
(x-p)(x-q) = 0 \\
x^2 - (p+q)x + pq = 0 \\
x^2 - (N - \phi(N) + 1)x + N = 0
$$

If our value of $$\phi(N)$$ is correct, we can substitute this into the equation and solve it for two integer values $$p$$ and $$q$$. If the values are not integer, the result can be discarded.
