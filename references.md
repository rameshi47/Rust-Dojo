# References in Rust

## What is a Reference?

A reference allows us to access a value without taking ownership.

Example:

```rust
let temperature = 30;
let reference = &temperature;
```

Conceptually:

```text
temperature ───► 30
                  ▲
                  │
              reference
```

* `temperature` owns the value.
* `reference` borrows the value.
* Ownership remains with `temperature`.

---

## Why Do References Exist?

Without references, ownership would move into functions.

References allow:

* Reading values without ownership transfer
* Sharing data between functions
* Avoiding unnecessary copies
* Safe memory access

---

## Creating a Reference

Syntax:

```rust
&variable
```

Example:

```rust
let speed = 50;
let ref_speed = &speed;
```

Meaning:

> Create a reference to `speed`.

---

## Reading Through a Reference

```rust
let speed = 50;
let ref_speed = &speed;

println!("{}", ref_speed);
```

Rust automatically reads the value through the reference.

---

## Ownership Does Not Move

```rust
let speed = 50;
let ref_speed = &speed;

println!("{}", speed);
```

This works.

Reason:

```text
speed still owns 50
reference only borrows it
```

---

## References and Functions

Example:

```rust
fn show(value: &i32) {
    println!("{}", value);
}

fn main() {
    let temperature = 30;

    show(&temperature);

    println!("{}", temperature);
}
```

Flow:

```text
main owns temperature
      ↓
show borrows temperature
      ↓
show finishes
      ↓
main still owns temperature
```

---

# Mutable References

## What is a Mutable Reference?

A mutable reference allows access and modification.

Syntax:

```rust
&mut variable
```

Example:

```rust
let mut sensor = 20;

let reference = &mut sensor;
```

Conceptually:

```text
sensor ───► 20
              ▲
              │
         reference
```

The borrower can modify the original value.

---

## Why Must The Variable Be Mutable?

This is valid:

```rust
let mut sensor = 20;

let reference = &mut sensor;
```

This is invalid:

```rust
let sensor = 20;

let reference = &mut sensor;
```

Reason:

```text
sensor is not mutable
cannot create mutable reference
```

---

## Modifying Through a Mutable Reference

```rust
let mut sensor = 20;

let reference = &mut sensor;

*reference = 50;
```

Result:

```text
sensor = 50
```

The original value changed.

---

# Dereference (*)

## What is Dereference?

Dereference means:

> Access the original value through a reference.

Syntax:

```rust
*reference
```

---

## Example

```rust
let mut sensor = 20;

let reference = &mut sensor;
```

Memory:

```text
sensor ───► 20
              ▲
              │
         reference
```

Access value:

```rust
*reference
```

This reaches:

```text
20
```

---

## Modifying Through Dereference

```rust
*reference = 50;
```

Result:

```text
sensor ───► 50
```

Important:

```text
* does not modify the reference

* accesses the original value
```

---

# Relationship Between &, &mut and *

## &

```rust
&value
```

Meaning:

```text
Create reference
Read only
No ownership transfer
```

---

## &mut

```rust
&mut value
```

Meaning:

```text
Create mutable reference
Read and modify
No ownership transfer
```

---

## *

```rust
*reference
```

Meaning:

```text
Access original value
through the reference
```

---

# Real Embedded Example

Imagine:

```rust
let mut temperature = 30;
```

Main firmware owns temperature.

Function needs to update it:

```rust
adjust_temperature(&mut temperature);
```

Flow:

```text
main owns value
      ↓
function borrows value
      ↓
function updates value
      ↓
ownership remains in main
```

This is extremely common in:

* STM32 firmware
* GPIO control
* Sensor processing
* Communication drivers
* Industrial automation

---

# Apprentice Summary

## Reference (&)

```text
Borrow value
Read only
Ownership remains
```

## Mutable Reference (&mut)

```text
Borrow value
Read and modify
Ownership remains
```

## Dereference (*)

```text
Access original value
through the reference
```

## Important Memory

```text
&       → Create reference

&mut    → Create mutable reference

*       → Access original value
```

References exist so functions can use data without taking ownership or creating unnecessary copies.
