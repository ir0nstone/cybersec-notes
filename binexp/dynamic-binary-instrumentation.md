---
description: Manipulating programs without the source
---

# Dynamic Binary Instrumentation

## Overview

**Dynamic Binary Instrumentation** (DBI) is a technique for observing, analyzing, or modifying a program while it is running, without needing its source code.

This may not sound that useful, but believe me when I say it's incredibly helpful! The classic example of this is **fuzzing**. In large binaries with ridiculously complex code bases, sometimes fuzzing is the best approach, but fuzzers like AFL++ generally like the binary to be compiled in a special fuzzer-approved way. This is because, for fuzzers to be efficient, we want to keep track of which code paths the fuzzer went down and causes a crash at. In order for this to happen, we compile the binary with a bunch of fuzzer "flags" that trigger a "message" when code execution reaches them, so fuzzers can keep track of exactly what bugs they've found - but this is only possible if we have the source code!

This is where DBI comes in. Despite not having the source code, we can **inject** extra instructions to log memory accesses, track control flow, count instructions and more. This lets us keep track of **what** actually runs, but of course at the cost of performance.

Common frameworks include the incredibly powerful [Frida](https://frida.re) and the classic [Valgrind](https://valgrind.org/info/), which is many projects' go-to tool for identifying memory leaks and corruption.
