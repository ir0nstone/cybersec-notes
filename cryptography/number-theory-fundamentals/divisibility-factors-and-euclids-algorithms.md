---
description: An outline of the fundamentals of number theory
---

# Divisibility, Factors and Euclid's Algorithms

Cryptography is built on the foundations of algebra and number theory, which we will hopefully cover well enough here.

## Divisibility

$$a$$ is said to be divisible by $$b$$ if there is another integer $$c$$ such that $$a=bc$$. In this case, $$b$$ is said to be a **factor** of $$a$$. This is denoted by $$a \mid b$$ if $$a$$ divides $$b$$, and $$a \nmid b$$ if it does not.

## Greatest Common Divisor

Given two integers $$a$$ and $$b$$, the greatest common divisor $$gcd(a,b)$$ (also known as the highest common factor) is the largest integer $$p$$ where $$p \mid a$$ and $$p \mid b$$.

### Euclidean Algorithm

Given $$a$$ and $$b$$, we can write an equation linking the two:

$$
a = b \cdot q + r
$$

​Where $$q$$ and $$r$$ are integers which fit the equation with $$r < b$$. Basically, $$b$$ divides into $$a$$ a maximum of $$q$$ times with a remainder of $$r$$. But here's where the trick lies.

Every term added together in that equation must be divisible by $$gcd(a,b)$$ because if we treat the gcd as $$g$$ we can say that $$a=k_1g,b=k_2g$$​:

$$
k_1g = k_2g \cdot q + r
$$

Meaning $$r$$ has to be some integer multiple of $$g$$​.

But now, if we think outside the box, we realise that both $$b$$ and $$r$$ are divisible by $$gcd(a,b)$$... so we can just calculate $$gcd(b, r)$$!

This is quite a leap forward which will require a bit of thinking, but let's break it down algebraically:

$$
a = b \cdot q_0 + r_1 \\
b = r_1 \cdot q_1 + r_2 \\
r_1 = r_2 \cdot q_2 + r_3 \\
r_2 = r_3 \cdot q_3 + r_4 \\
$$

And so on. But when does it stop? When do we stop taking the GCD? Well we can stop taking the GCD once $$r_n = 0$$, in which case $$r_{n-1} \mid r_{n-2}$$ and as a result we can take $$r_{n-1}$$ as the GCD!

{% hint style="info" %}
I highly recommend you think about this for a bit until it makes sense to you, and make sure to use other resources if it helps!
{% endhint %}

#### Example

Let’s say we want to find the GCD of 8075 and 16283. First, we can write it in the form $$a= b \cdot q+r$$:

$$
16283 = 8075·2 + 133
$$

And now we attempt to calculate the GCD of 8075 and 133.

$$
8075 = 133 \cdot 60 + 95 \\
    133 = 95 \cdot 1 + 38 \\
    95 = 38 \cdot 2 + 19 \\
    38 = 19 \cdot 2 + 0
$$

Therefore the GCD of 16283 and 8075 is 19.

### Extended Euclidean Algorithm

We can take the Euclidean Algorithm a step further and calculate, in addition to the GCD, $$u,v \in \mathbb{Z}$$ for $$a,b$$ which sum to the GCD, i.e.

$$
au + bv = gcd(a,b)
$$

This extension of the algorithm is invaluable for calculating **modular inverses** of numbers and is based on using the Euclidean Algorithm to calculate the GCD then writing it in terms of other numbers, repeating the process for the smallest non-GCD number until we reach an equation with only the GCD and the two starting numbers. Let's work with the example above, writing an equation for the GCD:

$$
19 = 95 - 38 \cdot 2
$$

Now 38 is the smallest non-GCD number, and we can write it in terms of the larger number in the sequence of equations written in the example, then repeat for the next smallest:

$$
19 = 95 - 38 \cdot 2 \\
    = 95 - (133 - 95 \cdot 1) \cdot 2 \\
    = 95 - (133 \cdot 2 + 95 \cdot -2) \\
    = 95 \cdot 3 + 133 \cdot -2 \\
    = (8075 + 133 \cdot -60) \cdot 3 + 133 \cdot -2 \\
    = 8075 \cdot 3 + 133 \cdot -180 + 133 \cdot -2 \\
    = 8075 \cdot 3 + 133 \cdot -182 \\
    = 8075 \cdot 3 + (16283 + 8075 \cdot -2) \cdot -182 \\
    = 8075 \cdot 3 + 16283 \cdot -182 + 8075 \cdot 364 \\
    = 8075 \cdot 367 + 16283 \cdot -182
$$

Therefore our equation $$au + bv = gcd(a,b)$$ is as follows:

$$
16283 \cdot -182 + 8075 \cdot 367 = 19
$$
