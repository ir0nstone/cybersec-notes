---
description: Querying code to find vulnerabilities
---

# CodeQL

## Overview

CodeQL is a static analysis engine that can automate finding security vulnerabilities and quality issues in code. As a tool, it provides us with the capacity to to identify vulnerabilities through pattern-matching.

CodeQL is more powerful than something simple like regex, as it abstracts away the quirks of the language itself and generates a database of functions, variables, control flow, etc. The idea is that once we identify a vulnerability we can craft a query that would pick up that vulnerability and perhaps make us aware of similar code in other parts of a large, unwieldy codebase. The crafted queries (which can include many out-of-the-box ones) are automatically run on a pull request and often implemented as part of a CI/CD pipeline.&#x20;

GitHub owns CodeQL, and if you are interested in learning more you can check out their introduction [here](https://codeql.github.com/docs/codeql-overview/about-codeql/).
