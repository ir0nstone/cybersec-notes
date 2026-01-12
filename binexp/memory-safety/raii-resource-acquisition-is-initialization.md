# RAII (Resource Acquisition is Initialization)

The idea of RAII is that resource lifetime is tied to object lifetime. Resources are more than just memory - they can be file descriptors, sockets or a variety of things.

[RAII](https://doc.rust-lang.org/rust-by-example/scope/raii.html) ensures that whenever an object goes out of scope, its destructor is called and its owned resources are freed. This means **you never have to manually free memory** and protects you against resource leaks (like memory leaks).

Modern C++ actually supports RAII, which is part of a drive to improve C++ memory safety. It is worth noting, however, that RAII is no silver bullet - with pointers in C++ the same as always, RAII can still allow for memory corruption, as it does nothing about dangling pointers. Every little help, though!

We will soon see that [Rust](rust.md) massively buffs up RAII with a host of compile-time checks, providing proper security.
