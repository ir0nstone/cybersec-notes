# Small e

If $$e$$ is sufficiently small, the exponent is ineffective at encrypting $$m$$.

Let's say $$m^e<N$$; in this case, we can simply take the $$e$$th root of $$c$$. For example, if $$e=3$$, then we can calculate $$m = \sqrt[3]c$$.

If $$m^e > N$$ then this is a _bit_ more secure, but we can progressively add more multiples of $$N$$ until the cube root gives us a valid answer:

$$
m = \sqrt[3]{c + kn}
$$

### Python

In Python we can use the `gmpy3` `iroot` function:

```python
from gmpy2 import iroot

m = iroot(ct, e)
```
