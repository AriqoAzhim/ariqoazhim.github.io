# Variables

# Mutability

A mutable variable allows for it to be changed, immutable means they cannot be changed
Example Invalid Code: (This would return an Error: cannot assign twice to immutable variable)

```rust
let x = 5;
x = 6
```

Instead:

```rust
let mut x = 5;
x = 6;
```

# Constants

constants also cannot be changed. But they also cannot be mutated (turned mutable), and they
also have to be type annotated. So use constants if you want to ensure a variable is completely unchangeable

```rust
const CONSTANT_VALUE: u32 = 69_420; // you can make numerical literals more readable by add undercores
```

# Shadowing

Allows you to create a new variable from an existing name. You’re able to change the type of a variable without having to make a new one

```rust
let mut s = 1;
println!("s val is: {}", s);
let mut s = "six";
println!("s val is: {}", s);
```

# Data Types

## Scalar

These represent a single value.

### **Integer:**

There are 6 variants of integers in RUST as shown in this table

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1b7574d-1321-4350-bbfc-a6209c7b19c0/Untitled.png)

They can also be written in other integer literal forms shown here here l I t id vebc

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b25fe45-5395-4382-9bba-21df3fb6f51f/Untitled.png)

By default, i32 (signed 32bit) is used

```rust
//Example instantiations
let x = 5;
let x: u128 = 35;

// Example Math operations with integers
let sum = 5 + 10;
let remainder = 43 % 5;
let product = 4 * 20;
```

### **Floating Point Numbers:**

2 Variants

- fp32 for single precision
- fp64 for double precision

Read here for difference:

https://www.geeksforgeeks.org/difference-between-single-precision-and-double-precision/

By default, f64 is used as in modern processers the difference in processing is minimal

```
let y: f32 = 3.0; // f32
let y: f64= 3.0; // f64
```

### **Boolean:**

Similar to other languages this type accepts `true` or `false`

```
let t = true;
let f: bool = false; // with explicit type annotation
```

Character

Represents a Unicode character, is written with single quotes

```rust
let c = 'z';
let d: char = 'D';
```

## Compound

### Tuple:

A fixed size array of related data that can be of different types

```rust
let tup = ("String in here", 69_410);
```

There are two ways to access data in a tuple:

1. De-structuring
    
    ```rust
    let(example_string, funny_num) = tup
    ```
    
2. Dot Notation
    
    ```rust
    let example_string = tp.1;
    let funny_num = tp.2
    ```
    

### **Arrays:**

Arrays are a fixed length in RUST and can only contain one type