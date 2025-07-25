---
description: The first heap exploits
---

# The Malloc Maleficarum

In 2001, two of the most famous heap exploitation papers were printed in the famous [Phrack](http://phrack.org) magazine - [Vudo malloc tricks](http://phrack.org/issues/57/8.html) and [Once upon a free()](http://phrack.org/issues/57/9.html). These are some of the very first heap exploitation techniques published, covering some of the ones you have read about previously.

In late 2004, glibc was hardened, and this rendered these exploits obsolete. The next famous heap exploitation paper was [The Malloc Maleficarum](https://seclists.org/bugtraq/2005/Oct/118) in 2005, which documents a collection of techniques sorted into _Houses_:

* The House of Prime
* The House of Mind
* The House of Force
* The House of Lore
* The House of Spirit
* The House of Chaos

Each of these had its own unique spin. In keeping with this tradition, modern heap exploits are often nicknamed as their own _House_, such as the [House of Rust](https://c4ebt.github.io/2021/01/22/House-of-Rust.html).

The original houses are the cornerstone of modern heap exploitation, and while they're no longer possible, they were until more recently that you'd think. They are also important to understand to build up your knowledge.
