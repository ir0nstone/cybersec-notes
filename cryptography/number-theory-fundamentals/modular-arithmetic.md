---
description: An introduction to the fundamentals
---

# Modular Arithmetic

## The Basics

Modular Arithmetic is an incredibly important aspect of practically all asymmetric cryptography. A common way to refer to it is as **clock arithmetic** - imagine it's eleven o'clock right now. Three hours later it'll be two o'clock, right? Well we do the same thing in modular arithmetic:

$$
11 + 3 = 14 \equiv 2 \mod 12
$$

​Essentially we take the sum of $$11$$ and $$3$$ and then divide by $$12$$ and keep the remainder. Here, the $$12$$ is called the **modulus**. The triple equals $$\equiv$$ denotes a _congruence_, and we say $$14$$ is congruent to $$2$$ $$\mod 12$$.

We can think of this another way and say that two numbers $$a$$ and $$b$$ are congruent modulo $$m$$ if their difference $$a-b$$ is divisible by $$m$$. For example, $$26$$ and $$2$$ are congruent modulo $$12$$ because  $$26-2=24$$ which is divisible by $$12$$.

## Modular (Multiplicative) Inverses

In normal maths, every number has an **inverse** - another number that, when multiplied with them, equals $$1$$. In maths terms, we would say that for every number $$a$$ there is another $$b$$ where $$ab=1$$. Generally, we can say that $$b=\frac{1}{a}$$ as that fits the equation - the inverse of $$7$$, for example, is $$\frac{1}{7}$$.

In modular arithmetic, there are no fractions. Instead of finding $$b$$ such that $$ab=1$$, we instead find $$b$$ such that $$ab \equiv 1 \mod m$$. This means that for different values of $$m$$ there are different inverses for $$a$$. For example, the inverse of $$3 \mod 5$$ is $$2$$, because $$3 \cdot 2 = 6 \equiv 1 \mod 5$$. This inverse is called the _modular multiplicative inverse_. We can therefore say that $$3^{-1} \equiv 2 \mod 5$$.

Yet not every number $$a$$ has an inverse modulo $$m$$ - for example, $$2$$ has no inverse modulo $$6$$. Why? This is because $$gcd(a,m)=1$$. Let's prove this in the general case and see why that is.

### Proof&#x20;

Assume there is $$b$$ such that $$a \cdot b \equiv 1 \mod m$$. This would mean that there also exists $$c$$ such that $$ab = cm + 1$$ and therefore $$ab - cm = 1$$. It follows therefore that both sides must be divisible by $$gcd(a,m)$$, meaning $$gcd(a,m)=1$$.

As a result, if there is an inverse of $$a$$ modulo $$m$$, such an inverse can only exist if $$gcd(a,m)=1$$ $$\square$$

### An Intuitive Approach

If you're struggling to visualise this proof, think back to the previous example of $$2$$ having no inverse modulo $$6$$. Why is this? Well, regardless of how much you multiply $$2$$ with, the sequence will go $$0, 2, 4, 0, 2, 4, ...$$ and loop around _because_ it is a factor of 6.

Actually, the fact that a number it is not **coprime** with the modulus shows that it can never have an inverse. Again, why? Think about it this way: if they have a common factor, the remainder when one is divided by the other will also have that factor - and therefore not be $$1$$. If we choose $$4$$ rather than $$2$$, we can see this - if there is a common factor (as there is of $$2$$ in this case) there is no value $$x$$ for which $$4x$$ will return $$1$$ modulo $$6$$. Any number you can multiply $$4$$ by will always give a remainder divisible by $$2$$ when divided by 6.

I suggest you spend some time reflecting on this fact, as it is a very important result.

### Uses

The uses of modular multiplicative inverses are similar to those of regular inverses - as division. Normally, if we divide two numbers, we can treat the number being divided by as _multiplying by its inverse._

$$
8 \div 2 = 8 \cdot 2^{-1} = 8 \cdot \frac{1}{2} = 4
$$

​We can use the modular multiplicative inverse the same way - say we want to divide $$3$$ by $$2$$, modulo $$7$$:

$$
3 \div 2 \equiv 3 \cdot 2^{-1} \equiv 3 \cdot 4 \equiv 12 \equiv 5 \mod 7 \\
$$

{% hint style="info" %}
Note that $$4$$ is the inverse of $$2$$ as $$2 \cdot 4 \equiv 1 \mod 7$$.
{% endhint %}

And just like that we realise that dividing $$3$$ by $$2$$ modulo $$7$$ gives us $$5$$.
