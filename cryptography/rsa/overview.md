# Overview

## Encryption

**Alice** first generates primes $$p$$ and $$q$$ and $$N$$ such that $$N=pq$$. This is half of the public key, with the other half being the **public exponent** $$e$$, which should be **coprime** to $$\phi (N)$$ (meaning $$gcd(\phi (N), e) = 1$$). These two values are then made public.

If **Bob** wants to send a message $$M$$ to Alice using RSA, he calculates:

$$
c = M^e \mod N
$$

Where $$c$$ is now the ciphertext and is sent to **Alice**.

## Decryption

Alice calculates $$\phi (N) = (p-1)(q-1)$$ and then calculates the [**modular multiplicative inverse**](../fundamentals/modular-arithmetic.md#modular-multiplicative-inverses) $$d$$ of $$e \mod \phi (N)$$. $$d$$ is the **private key** used for the decryption which is known only to **Alice**.

To decrypt, the Alice calculates $$M$$ again:

$$
M = c^d \mod N
$$

The proof for this is based on [Euler's Formula](../fundamentals/rings-fields-and-eulers-totient-function.md#eulers-formula). Because we calculated $$d$$ such that $$de \equiv 1 \mod \phi (N)$$, then we know that there is an integer $$k$$ such that $$de = k\phi (N) + 1$$. Also note that $$c^d \equiv (M^e)^d \equiv M^{de}$$:

$$
M^{de} \equiv M ^ {k\phi (N) + 1} \equiv M^{k\phi (N)} \times M \equiv (M^{\phi (N)})^k \times M \equiv 1^k \times M \equiv M \mod N
$$

## Attacks on RSA

Attacks on RSA generally fall into several categories, including weak public exponent or a poor choice of $$p,q$$ which results in a value of $$N$$ which can be factorised or attacked in another way. Remember that the trapdoor function in RSA is the difficulty of factoring $$N$$ into the primes that make it up so only Alice is able to compute $$\phi(N)$$ efficiently!
