# Control Flow

## If Statements

should be very familiar but the condition must be explicitly boolean

```rust
let x = 5;

if x < 10 { // explicitly boolean expression, can't do if x, for example.
	print(" is < 10");
else if x < 22 {
	println!("x < 22")
else {
	println!("x > 22")
}

// can also do this
// if the condition is true then number is 5, otherwise it is 6
let cond = true;
let number = if cond {5} else {6}
```

## Loops

### loop {}

```rust
loop {
	// code
	println!("looopping");
	//executes code till 
	break;
}
```

### while {}

typical while loop

```rust
let mut number = 3;

while number != 0 { // breaks when cond is met
	println(" {} ", number);
	number -= 1; // decrement
}
```

### For {}

typical for loop used to loop through a collection typically

```rust
let a = [100, 200, 300];

for element in a.iter() {
	println!("value: {}", element);
}

// 1..56 denotes a range from 1 to 56
for element in 1..56 {
	println!("value: {}", element);
}
```