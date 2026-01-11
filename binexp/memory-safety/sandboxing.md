# Sandboxing

Sandboxing is a security mechanism for running untrusted code. The aim is to make a tightly-restricted, locked-down environment with minimal privileges, enforced by Operating System policies.

Inside the sandbox, access to sensitive resources - such as files, the network, devices, users or other processes - is restricted. This is a major part of the "[defence in depth](https://www.fortinet.com/uk/resources/cyberglossary/defense-in-depth)" philosophy, where we accept that vulnerabilities can occur despite our best efforts and we limit the potential impact of such vulnerabilities as much as possible.

## Linux

On Linux there are a handful of kernel-provided features. [Chromium](https://chromium.googlesource.com/chromium/src/+/0e94f26e8/docs/linux_sandboxing.md#user-namespaces-sandbox) uses [namespaces](https://man7.org/linux/man-pages/man7/namespaces.7.html) and [seccomp-bpf](https://www.kernel.org/doc/html/v4.19/userspace-api/seccomp_filter.html).

### Namespaces

To quote the man pages,

> A namespace wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource. Changes to the global resource are visible to other processes that are members of the namespace, but are invisible to other processes.

In effect, namespaces are kernel-provided and prevent anything in the namespace from accessing files, processes, networks, users or anything else outside the namespace. This stops processes inside the namespace from affecting anything _outside_ the namespace.

### seccomp-bpf

**Seccomp** filters system calls to only allow accepted ones, which has the effect of reducing attacker capabilities if they do get code execution.
