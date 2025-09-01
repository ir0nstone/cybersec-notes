# P=Q

If $$p = q$$ then $$N = pq = p^2$$ and you can use function such as `isqrt` in Python to retrieve $$p$$.

Note that in the situation $$N=p^2$$, $$\phi(N) \neq (p-1)^2$$ due to the full definition of Euler's totient function:

$$
\phi(n) = n \prod_{p|n} (1-\frac{1}{p})
$$

The key here is that $$p \mid n$$ are **distinct** prime factors, so we would only use $$p$$ once in the equation:

$$
\phi(n) = n(1-\frac{1}{p}) = n - p
$$
