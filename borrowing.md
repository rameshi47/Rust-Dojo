# Borrowing in Rust

## What is Borrowing?

Borrowing means:

> Using a value without taking ownership.

Borrowing is done through references.

Example:

```rust
let temperature = 30;

let reference = &temperature;
```

Ownership remains with:

```text
temperature
```

The reference only borrows.

---

# Why Borrowing Exists

Without borrowing:

```rust
fn show(value: String) {
}
```

Ownership moves into the function.

After the call:

```text
Original owner loses ownership
```

With borrowing:

```rust
fn show(value: &String) {
}
```

Ownership stays with the caller.

---

# Borrowing Rule 1

## You Can Have Multiple Read-Only Borrows

Example:

```rust
let temperature = 30;

let a = &temperature;
let b = &temperature;
let c = &temperature;
```

Valid.

Conceptually:

```text
temperature ───► 30
     ▲
     │
 ┌───┼───┐
 │   │   │
 a   b   c
```

All are reading.

Nobody is modifying.

---

# Borrowing Rule 2

## Only One Mutable Borrow At A Time

Example:

```rust
let mut temperature = 30;

let a = &mut temperature;
```

Valid.

But:

```rust
let mut temperature = 30;

let a = &mut temperature;
let b = &mut temperature;
```

Invalid.

Reason:

```text
Two mutable references
could modify same data
at same time
```

Rust prevents this.

---

# Why Rust Prevents This

Imagine:

```text
a wants value = 50

b wants value = 100
```

Which should win?

This can create bugs.

Rust avoids the problem entirely.

---

# Read Borrow + Mutable Borrow

Invalid:

```rust
let mut sensor = 20;

let a = &sensor;
let b = &mut sensor;
```

Reason:

```text
a reading

b modifying

same time
```

Rust rejects this.

---

# Correct Pattern

Read first:

```rust
let mut sensor = 20;

let a = &sensor;

println!("{}", a);
```

Then:

```rust
let b = &mut sensor;
```

Now valid.

---

# Borrowing In Functions

## Read Borrow

```rust
fn display(value: &i32) {
    println!("{}", value);
}
```

Usage:

```rust
let temperature = 30;

display(&temperature);
```

Ownership remains in main.

---

## Mutable Borrow

```rust
fn increase(value: &mut i32) {
    *value += 1;
}
```

Usage:

```rust
let mut temperature = 30;

increase(&mut temperature);
```

Result:

```text
temperature = 31
```

Ownership still remains in main.

---

# Embedded Rust Example

Suppose:

```rust
let mut sensor_value = 100;
```

Firmware owns the data.

Read function:

```rust
display_sensor(&sensor_value);
```

Borrow only.

Modify function:

```rust
filter_sensor(&mut sensor_value);
```

Borrow and modify.

No ownership transfer.

Very common in:

* STM32 firmware
* Drivers
* UART
* SPI
* I2C
* Industrial control

---

# Borrowing Mental Model

Owner:

```text
temperature ───► 30
```

Read Borrow:

```text
temperature ───► 30
                   ▲
                   │
               reference
```

Mutable Borrow:

```text
temperature ───► 30
                   ▲
                   │
             mutable_ref
```

Borrower can modify.

Owner still exists.

---

# Common Beginner Mistake

Thinking:

```text
Borrowing creates a copy
```

Wrong.

Borrowing does not copy.

Borrowing creates access.

---

# Difference Between Ownership And Borrowing

Ownership:

```rust
let a = String::from("Rust");
let b = a;
```

Ownership moved.

---

Borrowing:

```rust
let a = String::from("Rust");

let b = &a;
```

Ownership stays with:

```text
a
```

---

# Apprentice Summary

Borrowing means:

```text
Use value
Without taking ownership
```

Rules:

```text
Many read borrows allowed

OR

One mutable borrow allowed
```

Never:

```text
Multiple mutable borrows

OR

Read borrow + mutable borrow
at same time
```

---

# Memory Trick

```text
Ownership
    ↓
Responsible for value

Reference (&)
    ↓
Read borrow

Mutable Reference (&mut)
    ↓
Modify borrow

Borrowing
    ↓
Use without ownership
```

Master borrowing before moving to Structs.

Borrowing is one of the most important concepts in Rust.
