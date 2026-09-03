# Ownership in Rust

## What is Ownership?

Ownership is Rust's memory management system.

Every value in Rust has exactly one owner.

```rust
let temperature = 30;
```

In this example:

* `temperature` is the owner
* `30` is the value

Conceptually:

```text
temperature ───► 30
```

---

## Why Ownership Exists

Rust uses ownership to:

* Prevent memory leaks
* Prevent invalid memory access
* Eliminate the need for a garbage collector
* Improve memory safety

Rust checks ownership rules during compilation.

---

## Ownership Rule 1

### Every value has one owner

```rust
let speed = 50;
```

Owner:

```text
speed ───► 50
```

---

## Ownership Rule 2

### There can only be one owner at a time

Example:

```rust
let a = String::from("Rust");
let b = a;
```

Ownership moves from `a` to `b`.

```text
Before

a ───► "Rust"

After

b ───► "Rust"
```

`a` is no longer valid.

Trying to use `a` causes a compile error.

---

## Ownership Move

When ownership is transferred to another variable, it is called a move.

```rust
let a = String::from("Rust");
let b = a;
```

Ownership moved.

```text
a ❌ invalid

b ───► "Rust"
```

---

## Copy Types

Some types are copied instead of moved.

Example:

```rust
let a = 10;
let b = a;
```

Result:

```text
a = 10
b = 10
```

Both variables remain valid.

Common Copy types:

* i8
* i16
* i32
* i64
* u8
* u16
* u32
* u64
* bool
* char
* f32
* f64

---

## Ownership and Functions

### Ownership Transfer

```rust
fn show(name: String) {
    println!("{}", name);
}

fn main() {
    let name = String::from("Rameshi");

    show(name);
}
```

Ownership moves into the function.

After the call:

```text
main no longer owns name
show owns name
```

---

## Returning Ownership

Ownership can be returned.

```rust
fn give_back(name: String) -> String {
    name
}
```

Ownership goes into the function and comes back.

---

## Ownership vs Borrowing

Ownership:

```rust
let name = String::from("Rust");
```

Borrowing:

```rust
let reference = &name;
```

Ownership remains with `name`.

```text
name ───► "Rust"
              ▲
              │
         reference
```

Borrowing allows access without taking ownership.

---

## Real Embedded Example

Imagine a sensor value:

```rust
let sensor = 100;
```

Main firmware owns the value.

Functions can borrow it:

```rust
read_sensor(&sensor);
```

The function can use the value without becoming the owner.

This avoids unnecessary copies.

---

## Ownership Mental Model

Think:

```text
Owner
  ↓
Responsible for value
```

Example:

```text
temperature ───► 30
```

If ownership moves:

```text
temperature ❌

new_owner ───► 30
```

Only one owner exists.

---

## What We Learned

### Ownership

The variable responsible for a value.

### Move

Transfer ownership to another variable or function.

### Copy

Duplicate small values instead of moving ownership.

### Borrow

Allow another function to use a value without taking ownership.

---

## Apprentice Notes

Important memory:

```text
Every value has one owner.

Ownership can move.

Copy types are duplicated.

Borrowing allows access without ownership.
```

Ownership is the foundation of:

* References (&)
* Mutable References (&mut)
* Borrowing
* Lifetimes

Master ownership first.
Everything else becomes easier.
