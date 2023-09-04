Enums are very powerful in RUST and is implemented very similar to the implementations in other functional programming languages such as Python, Javascript and Kotlin. They allow you to enumerate a list of variants. For example, in the case of IP Addresses and enumerator may be used to create a list of address variants which have the same format but with different values.  

# Example Implementation

```rust
enum IpAddrKind {
    // Add parenthese after the variant and insert the expected data type for that variant
    V4(u8, u8, u8, u8),
    V6(String),
}

// struct IpAddress {
//     kind: IpAddrKind,
//     address: String
// }

fn main() {
    // variants are namespaced under their identifier
    // i.e ipAddrKind being the namespace here then adding ::V4
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;

    // let localhost = IpAddress {
    //     kind: ipAddrKind::V4,
    //     address: String::from("127.0.0.1")
    // }

    // above instantiateion can be written instead like this
    let localhost = IpAddrKind::V4(127, 0, 0, 1);
}

// you can use an enum as a function parameter
fn route(ip_kind: IpAddrKind) {}
```

Enum methods are similarly added in an implementatin block like in the case of structs

```rust
// enum with variants of different types
enum Message {
    Quit,
    Move { x: i32, y: i32},
    Write(String),
    ChangeColor(i32, i32, i32)
}

// we can define methods for the Message enum in an impl block similar to a struct
impl Message {
    fn some_function() {
        println!("When?");
    }
}
```

# The Option Enum

Many Languages have NULL values, meaning that a variable could be NULL(not exist) or not NULL(exist). RUST does not have a NULL value. However, the functionality is instead expressed through the Option Enum which looks like this:

```rust
fn main() {
    enum Option<T> {
        Some(T), // Stores "some" value, could be anything since it's a generic
        None, // Stores no value
    }
}
```

So if there is a value that could potentially be NULL you would wrap it in the Option enum, allowing the type system to enforce that you handle the None() case, when a value does not exist and guarantee in the Some(T) case that the value is present.

The Option enum is included in the program’s scope by default

Example:

```rust
let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i8> = None; // we have to annotate the None type since Rust can't guess

let x = 5;
let y: Option<i8> = None;

// the unwrap_or basically says that if there is Some() value in y, then unwrap the option
// type and extract the integer out of it OR if there is None() in y then y = default val 0
let sum = x + y.unwrap_or(0);
```

# Match Expression

Allows you to compare a value against a set of patterns. These patterns can be literals, variables, wild cards among others. The Match Expression is exhaustive meaning that we have to deal with all possible cases that may occur during runtime which Rust calls “arms” of the match expression.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(State)
}

#[derive{Debug}]
enum State {
    California,
    Michigan,
    Florida,
    Alabama
    //.. idk anymore lol
}

fn main() {
    let x = Coin::Quarter(State::Florida);
    val_in_cents(x);
}

fn val_in_cents(coin: Coin) ->u8 {
    match coin {
        Coin::Penny => {
            1 // you can put curly brackets if you need to do more than one operation for a case
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("Minted in {:?}", state);
            25
        }
    }
}
```

combining match expression with Option enum:

```rust
fn main() {
    let five = Some(5);
    let sixe = plus_one(five);
    let none = plus_one(None);
}

fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1)
    }
}
```

In the above case there are only 2 possible values, but imagine a case where you several different types. In that scenario you can use the underscore “_” placeholder like so:

```rust
fn main() {
    let five = Some(5);
    let sixe = plus_one(five);
    let none = plus_one(None);
}

fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1), 
				_ => None // acts like an else: conditional, 
									// in this match exression it says, if Some() value then return Some(i + 1)
									// else, return None
    }
}
```

# If Let Syntax

```rust
fn main() {
    let some_val = Some(3);

    // match some_value {
    //     Some(3) => println!("three"),
    //     _ => (),
    // }

    // the following can replace the above code
    // this means if some_value == Some(3) then enter the curly brackets
    if let Some(3)  = some_value {
        println!("three");
    }
}
```