---
description: The simple iterative approach
---

# Baby Step, Giant Step

Baby Step, Giant Step is a very simple approach to solving the DLP often described as a **meet-in-the-middle algorithm**.

## Approach

Let us say that $$a^x \equiv b \mod p$$. First we choose an arbitrary number $$k$$, which defines how many iterations we try. Next we evaluate

$$
a, a^2, a^3, ..., a^{k-1}
$$

​Following that, we evaluate

$$
ba^{-k}, ba^{-2k}, ba^{-3k},...,ba^{-rk} \text{ 
 where } rk>p
$$

​If the DLP has a soultion, there will be at least one common value in **both lists**. If we let these common values be $$a^n$$ and $$ba^{-mk}$$, we can then solve it easily from there:

$$
a^n \equiv ba^{-mk} \mod p \\
a ^ {mk + n} \equiv b \mod p
$$

Solving the DLP! A few problems arise, however.

You will first notice that our choice of $$k$$ is arbitrary. If $$p$$ is large enough, this approach becomes completely infeasible, so using solely this algorithm is only effective when $$p$$ is small. After all, this is a very naive brute-force approach.

## Implementing BSGS

In Sage, this is quite simple:

```python
def baby_step_giant_step(a, b, p, k=1000):
    F = GF(p)
    a, b = F(a), F(b)

    powers_of_a = []

    # add all powers of a to the list
    for i in range(k):
        a_pow = a^i

        if a_pow == b:
            # solved, we are dunzo
            return i

        if a_pow in powers_of_a:
            # we've cycled around and so gotten every single element in the subgroup of a
            # none of them are b, so the DLP can't be solved
            return None

        powers_of_a.append(a_pow)

    # backwards powers now
    r = 0
    while r*k < p:
        backward = b*a^(-r*k)

        if backward in powers_of_a:
            return powers_of_a.index(backward) + r*k

        r += 1

    return None
```

And we can test it easily too

```python
# test it out
p = 104_729     # prime
F = GF(p)

a = F(4)
# print(a^2932)
b = F(20227)

x = baby_step_giant_step(a=a, b=b, p=p, k=100)

assert a^x == b

print(x)
```

