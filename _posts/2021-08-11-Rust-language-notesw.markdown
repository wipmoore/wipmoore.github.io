---
layout: post
title: "Rust Language Notes"
date: 2021-08-10 15:00:00 +0100
categories:
---

- [Memory Safety](#memory-safety)
- [Ownership](#ownership)

## Memory safety

- You decide the lifetime of each value __Rust__ will free the memory and other resources belonging to the value at a point that is under your control.

- A program can never use a pointer to an object after it has been dropped so long as you do not use any unsafe code.

## Ownership

- A value can only have a single owner at any point in time.
- The lifetime of a value is defined by its owner.
- When the owner of a value is __dropped__ so is the value.
- When control leaves the scope in which a variable is declared it is dropped.

- Variable own their values
- Structs own their fields
- Tuples, Arrays and Vectors own their elements

### Moving

- Assign a value from one variable to another move the __ownership__ of the value.
- Passing a value to a function or return one from a function __moves__ the ownership.

- __N__ This is moving ownership not necessarily moving the values position in memory.
- __N__ Move applies to values not the heap storage a value may own.

```Rust
let x = vec![10, 20, 30];
if e {
	f(x);
} else {
	g(x);
}
```

- Is valid as only one branch can be called.

```Rust
let x = vec![10, 20, 30];
while f() {
	g(x);
}
```

- Is invalid as after the first pass of the loop x is uninitialised.


```Rust
let three = v[2];
```

- Fails because it would mean moving the value out of the vector and leaving an uninitialize gap.

### Copy Types

- Simple types like integers etc just copy the value rather than moving the ownership.
  - Machine integers
  - Floating point numbers
  - Characters
  - Boolean
  - Tuples and Fixed size arrays of __Copy__ types.

- N. It is only types that a simple bit-to-bit copy will suffice that can be copy types.

- By default __structs__ are not copy types

- If all fields of a struct are copy types it can be made a copy type by implemeing the _Copy_ and _Clone_ Traits.

```Rust
#[derive(Copy,Clone)]
...
```

### Shared Ownership

- For values that you want to live until everyone is done with it there are __Reference Counted__ types.

```Rust
let s : Rc<String> = Rc::new("shiratati".to_string());
let t : s.clone();
let u : s.clone();
```

- Rc is allocated on the heap.

- __N__ a value can not be both _shared_ and _mutable_

- There is also an _Arc_ reference counted type that is thread safe.
