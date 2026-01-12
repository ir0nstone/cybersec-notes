---
description: >-
  Swift uses purely ARC. How does it fix the problems that arise without a
  tracing garbage collector?
---

# Swift

## Fixing Reference Cycles

Swift has two keywords that help fix reference cycles when they are found. The first is `weak`, which does not increment the reference count and instead becomes `nil` whenever the instance being referred to is deallocated. `weak` references must therefore be nullable.

The second keyword is `unowned`, which like `weak` creates no strong reference but also cannot be nullable. If the object being referred to is deallocated, the app will crash when the unowned reference is used.

In summary, Swift leaves it up to the developer to identify and fix reference cycles.

## Fixing Races caused by Multi-Threading

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
