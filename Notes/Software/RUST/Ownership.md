This is RUST’s shining feature that allows for it’s memory safety properties and is it’s solution to dealing with memory management

# Memory Management Solution Comparison

|  | Used In | Advantages | Disadvantages |
| --- | --- | --- | --- |
| Garbage Collection | JAVA, C# | - Error Free*: You can’t introduce memory bugs if it’s handled by a garbage collector. However, your garbage collector can still have bugs just much less than if done manually

- Faster Write Time: Because you don’t have to deal with memory yourself | - No Control over memory

- Slower and unpredictable runtime performance: Since the garbage collector decides memory management. You can’t optimise it yourself

- Larger Program Size: Garbage collector is a piece of code you have to include with the program |
| Manual Memory Management | C | - Control Over memory
- Faster Runtime
- Smaller Program size | - Error Prone: since memory management is done manually

- Slower write time |
| RUST Ownership Model | RUST | - Control over memory: 
- Error Free*: Due to many memory checks during compile time
- Faster runtime
- Smaller Program Size
 | - Slower development time. Learning curve aka Fighting with the borrow checker. RUST has a strict set of rules to follow so you have to follow them and this can take time to learn |

# Brief Overview (Stack and Heap)

Memory is laid out in a stack and heap during runtime. RUST makes certain decisions depending on whether or not memory it stored on a stack or on a heap.

The Stack is fixed size and cannot grow or shrink during runtime. The stack stores “Stack Frames” which are created for every function that executes. The stack frame stores the local variables of the function being executed. The size of a stack frame is calculated at compile time, meaning that the variables inside of the stack must have a known fixed size. Variables also only live as long as the stack frame is within the stack

The heap on the other hand is less organised and can grow or shrink at runtime. The data stored on it can be dynamic in size and in large amounts and we could determine the lifetime of the data

## Memory Management Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e9ccd50-d6f7-49d9-883f-bfaa971bd3c5/Untitled.png)

```rust
fn main() {
// a "Stack Frame" is added to the stack, initialising the variables x and y
// x is a string literal which is stored in the binary with the reference &str in the stack frame
// y is an unsigned integer so that can be stored directly on the stack frame
    fn a() {
        let x: = "hello";
        let y = 22;
				// b "Stack Frame" is added to the stack
        b();
    }

    fn b() {
				// x is a String which is dynamic in size so it cannot be stored in the stack and 
				// is therefore allocated memory in the heap, which then passes a pointer back to the stack
				// frame for referencing the String.(The pointer is stored on the stack frame)
        let x = String::from("world")
    }
}
```

Pushing to the stack is faster than allocating to the heap, since time has to be spent looking for space on the heap to store the new data. Also accessing data on the stack is faster since you’d have to follow a pointer to access heap data.

# Ownership Rules

**These are VERY important**

1. Each value in RUST has a variable that’s called its owner
2. There can only be one owner at  a time
3. When the owner goes out of scope, the value will be dropped

For example:

```rust
// s is not valid here as it has not been initialised
{
	let s = "hello"; // s is initialised here as a string literal &str
	// s can now be used 
}
// s is now bye bye and therefore invalid
```

If wanting to have a dynamic string

```rust
// s is not valid here as it has not been initialised
{
	let s = String::from("hello"); // s is initialised here as a dynamic string (String)
																 // Now mutable
																 // no need for String new() and delete(), done automatically
	// s can now be used 
}
// s is now bye bye and therefore invalid
```

## Example Ownership run through

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    println!("{}", s);
}

fn takes_ownership(s: String) {
    println!("{}", s);
}
```

# Allocation

When doing the following:

```rust
let x = 1;
let y = x;
```

the value in x is copied over to the y value. This is because the value was an integer. Integers and other simple types have the copy trait which allows for this to happen by default. 

However, if you were to do the following

```rust
let s1 = String::from("hello");
let s2 = s1; // RUST does a MOVE here by default. Meaning that the value is "moved" from s2 to s1
             //  making s1 invalid

println!("S1 = {}", s1); // this will not work since s1 was moved to s2
```

By default a string will be moved. The value is moved from s1 to s2 making s1 invalid to operate on as it points to nothing.

Instead you can call the .clone() method on the s1 as such:

```rust
let s1 = String::from("hello");
let s2 = s1.clone; // RUST does a CLONE instead of MOVE here. Value is copied and saved from s1
                   // but this is more expensive. 

println!("S1 = {}", s1); // this will work
```

# Understanding References

## Borrowing

Instead of taking ownership of a variable and then giving back that ownership when creating a function like here:

```rust
fn main() {
    let s = String::from("hello");
    s = takes_and_gives_back_ownership(s);
    println!("{}", s);
}

fn takes_and_gives_back_ownership(s: String) -> String {
    println!("{}", s);
    s
}
```

We can use a reference instead so that you don't have to do that. This is called borrowing

(passing in function parameters as references).

```rust
fn main() {
    let s = String::from("hello");
    s = using_ref(&s);
    println!("{}", s);
}

fn using_ref(s: &String) {
    println!("{}", s)
}
```

References are immutable by default but you can make them mutable like so:

```rust
fn main() {
    let s = String::from("hello");
    s = using_ref(&mut s);
    println!("{}", s);
}

fn using_ref(s: &mut String) {
    println!("{}", s)
}
```

However, you can only have one mutable reference at a time. You cannot have the case where:

```rust
fn main() {
    let s = String::from("hello");
    s = using_ref(&mut s);
		s2 = using_ref_again(&mut s); // invalid
    println!("{}", s);
}

fn using_ref(s: &mut String) {
    println!("{}", s)
}
```

Additionally you cannot have an mutable reference if an immutable reference already exists within the scope but you can have multiple immutable references.

If the immutable references leave the scope however, you can make a mutable reference. The scope for the variable begins at when it is instantiated and ends the last time a reference/owned instance is used. After that you can make a mutable reference to it. as such:

```rust
fn main() {
	let mut s = String::from("hello");//beginning of scope for s

	let r1 = &s; 
	let r2 = &s;

	println("{} {}", r1, r2);// end of scope of r // end of scope for s used by r1 and r2
	
	let r3 = &mut s; // s can be used again since nothing else is
	println("()", r3);

}
```

What happens when you have a reference that points to invalid data?

```rust
fn main() {
    let ref_to_nothing = dangle();

}

fn dangle() -> &String {
    let s = String::from("Hello"); // rust will throw an error here
                                   // since this is memory unsafe. the s variable will be dropped
                                   // since s was instantiated within this function
                                   // Therefore when returning the &String it will point to 
                                   // nothing and RUST will throw and error
    &s
}
```

## Rule of References

1. At any given time, you can have either one mutable reference or any number of immutable references
2. References must always be valid

# Slices

Slices let you reference a contiguous sequence of elements within a collection instead of referencing the entire collection and do not take ownership of the underlying data.

```rust
fn main() {
    let s2 = "hello world"; // string literals are string slices
    let first_word = first_word(&s2);
}

// function to return the first word of a sentence
// returns a string slice
fn first_word(s: &str) -> &str {
    // convert the string into an array of bytes
    let bytes = s.as_bytes();

    // iterate through the bytes and call the enumerate
    // to get the item and the index of that item
    // then for each byte we look for an empty space
    // which would signify the end of a word, and if that's true 
    // we return a string slice from the beginning of the sentence
    // to the current index

    //i = index, &item = reference to byte in byte array
    for(i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[..i];
        }
    }

    &s[..];
}
```

You can also use slices for other data types. e.g Integers

```rust
let int_list = [1, 2, 3, 4, 5];
let first_3 = &int_list[..2]; // gets a slice of teh first 3 ints in the list
```