---
description: The basics of Rings, Fields and Euler's Phi Function
---

# Rings, Fields and Euler's Totient Function

## Rings

In modular arithmetic we work modulo some modulus $$m$$, and we can think of the numbers we are able to use this way as a group of numbers:

$$
\mathbb{Z}/m\mathbb{Z} = \{0,1,2,...,m-1\}
$$

​This ring $$\mathbb{Z}/m\mathbb{Z}$$ is the _ring of integers modulo m_. We are able to add and multiply elements of this ring, then divide by $$m$$ and take the remainder to obtain an element in $$\mathbb{Z}/m\mathbb{Z}$$. There are [general rules which define this as a ring](https://www.britannica.com/science/ring-mathematics), including the need for associative addition and multiplication.

### Units of a Ring

As we discussed, $$a \in \mathbb{Z}/m\mathbb{Z}$$ has a modular multiplicative inverse if $$gcd(a,m)=1$$. The set of all numbers with modular inverses are denoted as $$(\mathbb{Z}/m\mathbb{Z})^*$$ - this is called the _group of units modulo_ $$m$$. Number which have inverses are called **units**. For example:

$$
(\mathbb{Z}/12\mathbb{Z})^* = \{1,5,7,11\}
$$

You can think of the group of units modulo $$12$$ as essentially a set containing all numbers less than $$12$$ which are coprime with it.

## Fields

If every number in $$\mathbb{Z}/m\mathbb{Z}$$ has a modular inverse, it is promoted from a **ring** to a **field** (a field is a ring in which division is possible). Note that the only values of $$m$$ where this are possible are values which are prime, which is why these fields are usually denoted $$\mathbb{F}_p$$. A **finite field** (also called a **Galois field**) is a field with a finite number of elements, such as those used in modular arithmetic. As a result, the term finite field will come up quite often to describe $$\mathbb{F}_p$$.

### Relevant Sets of Numbers

$$\mathbb{Z}$$ denotes the **ring of integers** - note it is not a field, as there are no fractions to provide inverses.&#x20;

## Euler's Totient Function

Euler's totient function returns the number of elements in the group of units modulo $$m$$. Mathematically, this means:

$$
\phi(m) = \#(\mathbb{Z}/m\mathbb{Z})^* = \#\{0 \leq a < m : gcd(a,m) = 1\}
$$

​This function has a huge array of applications and further rules which allow us to do some pretty awesome things, and is a key piece of the RSA cryptosystem.

### Totient Rules

If $$gcd(p,q)=1$$ then $$\phi(pq)=\phi(p)\phi(q)$$.

### Computing the Euler Totient

If we say that the prime factorisation of $$n$$ is $$n=p_1^{e_1} \cdot p_2^{e_2} \cdot ... \cdot p_k^{e_k}$$ then

$$
\phi(n) = n \prod_{p \mid n} (1-\frac{1}{p})
$$

Note that if $$p$$ is prime, $$\phi (p) = p-1$$.

#### Proof

TODO

### Euler's Formula

Euler's Formula states that if $$gcd(a,p)=1$$ then​

$$
a^{\phi(n)} \equiv 1 \mod n
$$

This is perhaps the most important formula for the RSA cryptosystem, which we will get to soon.​ Note that this actually tells us a little more in the case modulo a prime $$p$$, a case named after Fermat.

### Fermat's Little Theorem

Since $$\phi(p)=p-1$$​, Fermat's Little Theorem states that:

$$
a^{p-1} \equiv 1 \mod p
$$

This was stated by Fermat over 100 years before Euler's Formula. You may note that this also gives us a quick way to compute the **modular multiplicative inverse** of $$a$$ in $$\mathbb{F}_p$$:

$$
a^{p-1} \equiv 1 \mod p \\
a^{p-1} \cdot a^{-1} \equiv 1 \cdot a^{-1} \mod p \\
a^{p-2} \equiv a^{-1} \mod p
$$

