# Mersenne Primes

[Mersenne Primes](https://en.wikipedia.org/wiki/Mersenne\_prime) take the form $$p = 2^k - 1$$. Often we just loop through all possible Mersenne primes and check if either $$p$$ or $$q$$ are that.

This is very easy to do in Sage:

```python
from sage.combinat.sloane_functions import A000668

mersenne = A000668()
```

Then we can grab the $$n$$th Mersenne prime using `mersenne(n)`.
