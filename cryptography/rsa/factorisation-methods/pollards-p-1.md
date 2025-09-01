---
description: Factorising N when we know a factor is smooth
---

# Pollard's p-1

If we say that $$N=pq$$, where $$p,q$$ are prime, then we can also say that for any $$a,k$$:

$$
a^{k(p-1)} \equiv 1 \mod p
$$

Due to [Fermat's Little Theorem](../../fundamentals/rings-fields-and-eulers-totient-function.md#fermats-little-theorem). This would mean $$a^{k(p-1)} - 1$$ is a factor of $$p$$ and therefore $$N$$.

## Overview

Let us say that $$L=k(p-1)$$. If $$a^L \equiv 1 \mod p$$, then $$p \mid a^L-1$$. Since $$p \mid N$$, we know that $$p \mid gcd(a^L-1, N)$$.

This may seem all well and good, but how does this help us? We don't know $$p$$, nor do we have a way of calculating $$k$$.

What we actually do here is _guess_ the value of $$L$$ by (effectively) brute force. Essentially we put $$a$$ to the power of integers with a huge number of prime factors, so it's all the more likely that the factors of $$p-1$$ are there. Once we put it to the powers, we can calculate the $$gcd(a^L-1, N)$$ and if it is not equal to $$0$$ then we have factorised $$N$$ successfully.

The most common technique is to calculate $$a^{k!} \mod N$$ and then calculate the GCD from there. We can make this into a more efficient approach by repeatedly putting it to the power of one greater than the previous iteration, e.g. calculate $$a,a^2,(a^2)^3,((a^2)^3)^4,...$$ and calculate the GCD each time - this way, we will have a wealth of factors. A common choice of $$a$$ is $$2$$.

## Example

Let's try and factorise $$713$$ using Pollard's $$p-1$$:

$$
2 ^ 1 \equiv 2 \mod 713, \,\, gcd(1, 713) == 1 \\
2 ^ 2 \equiv 4 \mod 713, \,\, gcd(3, 713) == 1 \\
4 ^ 3 \equiv 64 \mod 713, \,\, gcd(63, 713) == 1 \\
64 ^ 4 \equiv 326 \mod 713, \,\, gcd(325, 713) == 1 \\
326 ^ 5 \equiv 311 \mod 713, \,\, gcd(310, 713) == 31
$$

We get to $$2^{5!} \mod 713$$, and find that $$gcd(310, 713)=31$$​. We have successfully factorised $$N$$!

## Applications

This algorithm is useful when we know $$p$$ is **smooth**, meaning it has **lots of small prime factors**. If this is the case, the iterative approach is more likely to include $$p-1$$ sooner rather than later. This leads to the concept of **safe primes**, primes in the form $$p=2q+1$$ where $$q$$ is also a prime (called a [Sophie Germain prime](https://en.wikipedia.org/wiki/Safe\_and\_Sophie\_Germain\_primes)). In this case, $$p-1$$ is not smooth.

## Sage Code

We can implement this algorithm really easily in Sage:

```python
def pollards(n, iterations=50):
    R = IntegerModRing(n)
    cur = R(2)
    
    for i in range(2, iterations):
        print(f'{cur} ^ {i} = {cur**i}')
        
        cur **= i
        tst = gcd(cur-1, n)
        
        print(f'gcd({cur-1}, {n}) = {tst}')
        
        if tst != 1:
            return tst
    else:
        return None
```

You can remove the `print()` functions as they are completely unneccessary, but the logging may help visualise what it does.
