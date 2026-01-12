---
description: The poster child for memory safety
---

# Rust

Rust's memory safety relies on two main principles: **ownership** and **borrowing**.

Every value in Rust has an **owner**, meaning that at any given point in the code there is **one** thing that can have read/write control over the value. Ownership can be **borrowed** temporarily, but this is tracked by Rust's **compiler** and the violation of any ownership rules **does not compile**.

This is a different approach to other programming languages. As we have seen so far, C has no such concept of memory safety. Higher-level _managed_ languages, such as Python, Java or C#, handle memory-safety in the **runtime**. This compromises speed, as well as the requirement of a runtime.

[This article](https://www.infoworld.com/article/2336661/rust-memory-safety-explained.html) has a good explanation, as does [this Stack Overflow](https://stackoverflow.com/questions/36136201/how-does-rust-guarantee-memory-safety-and-prevent-segmentation-faults) post. [The official documentation for this topic](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html) is also fantastic.

### Ownership

Consider the following code:

```rust
fn main() {
    let original = "Hello, World!".to_string();
    let other = original;
    println!("{}", original);
}
```

While this is valid in many other languages, Rust will throw an error here because the value is **moved**.

```
error[E0382]: borrow of moved value: `original`
 --> src/main.rs:4:20
  |
2 |     let original = "Hello, World!".to_string();
  |         -------- move occurs because `original` has type `String`, which does not implement the `Copy` trait
3 |     let other = original;
  |                 -------- value moved here
4 |     println!("{}", original);
  |                    ^^^^^^^^ value borrowed here after move
```

In the second line, as soon as we define `other`, it becomes **invalid** to use `original` again. This protects against **double-frees**, **use-after-frees** and any other sorts of vulnerabilities that can arise due to dangling pointers.

{% hint style="info" %}
Note that the error talks about how the type `String` does not implement [the `Copy` trait](https://doc.rust-lang.org/std/marker/trait.Copy.html). If a type does implement this, then an assignment like the second line here would **duplicate** the variable, copying all of the bits over. This wouldn't cause an issue, as there are two distinct variables owning their respective values. What Rust does not want is two variables refering to **the same data in memory**, which would occur for `String` types as they are just pointers to the actual string in memory.
{% endhint %}

### Borrowing

Instead of copying data or transferring ownership, Rust allows you to **borrow** data. This borrowing can be done **immutably** or **mutably**. There are restrictions on borrowing which prevent **data races**, **iterator invalidation** and **bugs caused by concurrency**.

#### Immutable Borrowing

Immutable borrowing allows for read-only borrows. There can be an **any number of these borrows** at any one time:

```rust
fn main() {
    let original = "Hello, World!".to_string();
    let a = &original;
    let b = &original;

    println!("{}", a);
    println!("{}", b);
}
```

This compiles correctly, and acts as you would expect.

#### Mutable Borrowing

Mutable borrowing is a read/write borrow.

```rust
fn main() {
    let mut original = "Hello, World!".to_string();
    let a = &mut original;

    println!("{}", a);
    *a = "Test".to_string();
    println!("{}", a);
}
```

However, to prevent bugs such as data races, we **cannot have two mutable borrows**:

```rust
fn main() {
    let mut original = "Hello, World!".to_string();
    let a = &mut original;
    let b = &mut original;

    println!("{}", a);
    *a = "Test".to_string();
    println!("{}", a);
    println!("{}", b);
}
```

```
error[E0499]: cannot borrow `original` as mutable more than once at a time
 --> src/main.rs:4:13
  |
3 |     let a = &mut original;
  |             ------------- first mutable borrow occurs here
4 |     let b = &mut original;
  |             ^^^^^^^^^^^^^ second mutable borrow occurs here
5 |
6 |     println!("{}", a);
  |                    - first borrow later used here
```

Similarly, we actually can't take an **immutable** reference either!

```rust
fn main() {
    let mut original = "Hello, World!".to_string();
    let a = &mut original;
    let b = &original;

    println!("{}", a);
    *a = "Test".to_string();
    println!("{}", a);
    println!("{}", b);
}
```

```
error[E0502]: cannot borrow `original` as immutable because it is also borrowed as mutable
 --> src/main.rs:4:13
  |
3 |     let a = &mut original;
  |             ------------- mutable borrow occurs here
4 |     let b = &original;
  |             ^^^^^^^^^ immutable borrow occurs here
5 |
6 |     println!("{}", a);
  |                    - mutable borrow later used here
```

So we can have either

* **one** mutable borrow
* **any number** of immutable borrows

But it is not allowed for us to mix and match - **the compiler enforces this**.

Notably, even if the borrow is **immutable** we are unable to mpodify a mutable variable. ake the following example:

```rust
fn main() {
    let mut original = "Hello, World!".to_string();
    let a = &original;

    println!("{}", a);
    original = "wot".to_string();
    println!("{}", a);
}
```

We get the error

```
error[E0506]: cannot assign to `original` because it is borrowed
 --> src/main.rs:6:5
  |
3 |     let a = &original;
  |             --------- `original` is borrowed here
...
6 |     original = "wot".to_string();
  |     ^^^^^^^^ `original` is assigned to here but it was already borrowed
7 |     println!("{}", a);
  |                    - borrow later used here
```

Coming from a language like C, we can be forgiven for thinking that we can get a pointer to a variable and, once the variable is modified, read the new value with this pointer. But Rust doesn't allow that!

### Lifetimes

A **lifetime** is what the compiler's borrow checker uses to ensure borrows are valid. A lifetime begins when a variable is created, and ends when it is destroyed. [The Rust book](https://doc.rust-lang.org/rust-by-example/scope/lifetime.html) has a fantastic description of lifetimes, as well as how you can explicitly control them. The idea is that a variable is tied to the existence of other variables, and dies once they die.

Think about this example:

```rust
let r;
{
    let x = 5;
    r = &x;
}

println!("r: {}", r);
```

In something like C, `x` would go out of scope and die once the `}` ends the scope, while `r` lives on as a dangling reference to where `x` would have been. In Rust, the compiler catches this and refuses to compile.

```
error[E0597]: `x` does not live long enough
 --> src/main.rs:5:13
  |
4 |         let x = 5;
  |             - binding `x` declared here
5 |         r = &x;
  |             ^^ borrowed value does not live long enough
6 |     }
  |     - `x` dropped here while still borrowed
7 |
8 |     println!("r: {}", r);
  |                       - borrow later used here
```

Meanwhile, if we move the `println!` into the scope defined by the `{}`, the program compiles fine as `x` lives long enough for the borrowed value to be used.

### No Null Pointers

Rust does not have null pointers - cases must be handled explicitly. This prevents null pointer dereferences and maybe-null bugs.

### Out of Bounds

Rust performs out-of-bounds checking at compilation time, but if this is impossible (e.g. the index is provided by the user), it automatically includes runtime OOB checks. Take the following code for example:

```rust
use std::io;

fn main() {
    let arr = [10, 20, 30];

    println!("Enter an index:");

    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();

    let index: usize = input.trim().parse().unwrap();
    let value = arr[index];

    println!("Value: {}", value);
}
```

If we run the program and input `5`, which is OOB, the program panics:

```
thread 'main' (7216640) panicked at src/main.rs:13:17:
index out of bounds: the len is 3 but the index is 5
```
