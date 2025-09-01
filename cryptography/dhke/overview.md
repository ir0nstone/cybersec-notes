# Overview

Unlike RSA, where you can send messages of your choice, the DHKE is used to generate a secret number shared between Alice and Bob. This shared secret is then used as the key for a symmetric cryptosystem like AES.

## The Key Exchange

A large prime $$p$$ and a generator $$g \in \mathbb{F}_p, g \neq 0$$ are made public.

Alice and Bob choose their secret integers $$a$$ and $$b$$ respectively. They then compute the following:

$$
A = g^a \mod p \\
B = g^b \mod p
$$

These numbers $$A$$ and $$B$$ are exchanged between Alice and Bob over a public channel. Once the other person's number is received, they then put it to the power of their secret integer, i.e. Alice computes $$B^a$$ and Bob computes $$A^b$$, both modulo $$p$$. Note that:

$$
B^a = (g^b)^a = g^{ab} = (g^a)^b = A^b
$$

This means that once they do this, they are in possession of the same number, which they can then use as a shared secret.

## The Discrete Logarithm Problem

The safety of the Diffie-Hellman Key Exchange is grounded on he difficulty of solving the discrete logarithm problem - the difficulty of computing $$x$$ given

$$
g^x \equiv a \mod p
$$

You can see this in Overview presented above - the values $$g^a \mod p$$ and $$g^b \mod p$$ are sent over a public channel, but because we cannot solve the DLP efficiently an attacker is unable to retrieve $$a$$ or $$b$$. We say that the DLP is DHKE's **trapdoor function**.

As you may expect from here, many attacks on Diffie-Hellman rely on situations in which you **can** efficiently compute the discrete logarithm.
