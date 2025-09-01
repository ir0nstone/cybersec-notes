# Continued Fractions

## Overview

**Continued Fractions** are a way of representing real numbers as a sequence of positive integers. Let's say we have a real number $$\alpha_1$$. We can form a sequence of positive integers from $$\alpha_1$$ in this way:

$$
\alpha_1 = a_1 + \frac{1}{\alpha_2}, \text{where } a_1=\left \lfloor{x}\right \rfloor
$$

​For example, let's say $$\alpha_1=1.5$$​. We can say that

$$
\alpha_1 = 1 + \frac{1}{2}, \text{with } \alpha_2 = 2
$$

​The trick here is that if $$\alpha_2\not\in \mathbb{Z}$$, we can continue this exact process with $$\alpha_2$$ and keep the **continued fraction** going.

$$
\alpha_1 = a_1 + \frac{1}{a_2 + \frac{1}{a_3 + \frac{1}{\ddots}}}
$$

## Example

Let's take another example, that of $$\frac{17}{11}$$:



$$
\frac{17}{11} = 1 + \frac{6}{11} = 1 + \frac{1}{\frac{11}{6}} = 1 + \frac{1}{1 + \frac{5}{6}}  = 1 + \frac{1}{1 + \frac{1}{\frac{6}{5}}} = 1 + \frac{1}{1 + \frac{1}{1 + \frac{1}{5}}}
$$

​The list of continued fractions is represented as a list of the coefficients $$a_i$$, in this case

$$
\frac{17}{11} = \left[ 1; 1, 1, 5 \right]
$$

## Convergents

The $$k$$th convergent of a continued fraction is the approximation of the fraction we gain by truncating the continued fraction and using only the first $$k$$ terms of the sequence, for example the 2nd convergence of $$\frac{17}{11}$$ is $$1 + \frac{1}{1} = \frac{2}{1} = 2$$​ while the 3rd would be $$1 + \frac{1}{1 + \frac{1}{1}} = 1 + \frac{1}{2} = \frac{3}{2}$$.

One of the obvious applications of these convergents is as **rational approximations to irrational numbers**.

### Properties of Convergences

* As a sequence, they have a limit
  * This limit is $$\alpha_1$$, the real number you are attempting to approximate
* They are alternately greater than and less than $$\alpha_1$$

## Sage

In Sage, we can define a continue fraction very easily:

```python
f = continued_fraction(17/11)
```

We can then print off the list of coefficients:

```python
print(f)
>>> [1; 1, 1, 5]
```

And the convergents are even calculated for us:

```python
print(f.convergents())
>>> [1, 2, 3/2, 17/11]
```
