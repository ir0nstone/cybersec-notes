---
description: C++'s foray into memory safety
---

# C++ Smart Pointers

Smart pointers have existed almost as long as C++, but have recently gained attention as worries about memory safety becomes more commonplace. The idea of a smart pointer is that they are a wrapper around a raw pointer, which will automatically be freed once it stops being used (like a standard pointer in Rust).

`unique_ptr` allows for exactly one owner of the underlying pointer, and while ownership can be transferred to other `unique_ptr` objects, the pointer cannot be copied or shared. Once a `unique_ptr` goes out of scope, the underlying pointer (if it still exists) is freed to clean up. If ownership was transferred to another `unique_ptr`, then there is no underlying pointer, and nothing happens. The original raw pointer is not freed until the new owning `unique_ptr` is out of scope.

`shared_ptr` is different and allows for multiple owners. The raw pointer is not deleted until **all** `shared_ptr` owners are out of scope (or otherwise given up ownership). To do this, a `shared_ptr` also has a **shared** [**reference count**](garbage-collection/automatic-reference-counting.md) which is decremented every time a `shared_ptr` goes out of scope, is reset or is overwritten. Once it drops to zero, the raw pointer is freed.
