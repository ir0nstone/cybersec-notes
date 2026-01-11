---
description: Apple's preferred approach to automatic memory management
---

# Automatic Reference Counting

## Overview

If you have read the section on [C++ Smart Pointers](../c++-smart-pointers.md), you actually pretty much understand ARC. In languages such as Swift, where ARC is used as the only memory management tool, effectively every class instance is used as a `shared_ptr`. The official documentation is [here](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/).

Officially, every time a class instance is assigned to a property, constant or variable it creates a **strong reference** to the instance. The reference is called "strong" because the instance cannot be deallocated as long as one such reference exists. Every time one of these properties, constants or variables have their references removed, through something like reaching the end of their lifetime or through reassignment, the reference counter of the class instance drops. Once it reaches zero, the instance is deallocated.

## Drawback: Reference Cycles

One of the downsides of ARC is the idea of **reference cycles** (also called **retain cycles**), where two class instances have strong references to one another. A minimal example is as follows:

```swift
class A {
    var b: B?
}

class B {
    var a: A?
}

let a = A()
let b = B()
a.b = b
b.a = a
```

Note that here the instance of `A` has two strong references (`a` and `b.a`) while the instance of `B` also has two strong references (`b` and `a.b`).

If `a` and `b` go out of scope, the strong references to the instances of `A` and `B` will disappear, and the reference count will decrement to `1` for both instances. But `a.b` and `b.a` will still point at the instances of `A` and `B` respectively, so the instances will not be automatically deallocated. Similarly, they will **never** be deallocated, as `a.b` and `b.a` are no longer accessible, even though they still have strong references! This leads to memory leaks, and is one of the main drawbacks of ARC.

## Drawback: Multi-threading

Reference counting can lead to memory-unsafe execution if a program is multi-threaded, as one thread may delete the object as it is being used by another. This issue does not occur in garbage-collected programming languages.

## Uses of ARC

Having a programming language that uses purely ARC is very rare - only Apple's languages, Objective-C and Swift, do this.

Python [does count references](https://docs.python.org/3/glossary.html#term-reference-count), but it [also has a garbage collector](https://docs.python.org/3/glossary.html#term-garbage-collection) to detect and break reference cycles. Up until Python 3.13, the [Global Interpreter Lock](https://blog.jetbrains.com/pycharm/2025/07/faster-python-unlocking-the-python-global-interpreter-lock/) (GIL) meant that multi-threading in Python was actually all on one thread, which counteracted the multi-threading race condition. With the removal of the GIL, [some extra work](https://peps.python.org/pep-0703/) was needed to fix this problem.

### Swift: Fixing Reference Cycles

Swift has two keywords that help fix reference cycles when they are found. The first is `weak`, which does not increment the reference count and instead becomes `nil` whenever the instance being referred to is deallocated. `weak` references must therefore be nullable.

The second keyword is `unowned`, which like `weak` creates no strong reference but also cannot be nullable. If the object being referred to is deallocated, the app will crash when the unowned reference is used.

In summary, Swift leaves it up to the developer to identify and fix reference cycles.

### Swift's Multi-threading Fix

Swift requires synchronized access to any object that multiple threads can access concurrently. By this we mean that any **conflicting** operations are **ordered** and executed one after the other, rather than at once. It does this through the use of [Actors](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Actors), classes which serialize access to prevent data races, and [`Sendable`](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Sendable-Types), a type trait that lets Swift know that a type is safe to send between threads (more accurately, **concurrency domains**).

Note that Swift does not **restrict** invalid access, but it classifies it as **undefined** if you do not follow its concurrency model. For example, the following compiles:

```swift
class Box {
    var value = 0
}

let box = Box()

DispatchQueue.global().async {
    box.value += 1
}

DispatchQueue.global().async {
    box.value += 1
}
```
