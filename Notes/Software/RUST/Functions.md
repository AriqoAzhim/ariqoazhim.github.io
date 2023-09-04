In RUST you can think about a piece of code as a statement or an expression. 

- Statements perform and action but don’t return a value
- Expressions DO return a value

```rust
fn main() {
let sum = example_function(11, 12);
println!("Sum = {}", sum)
}

fn example_function(x: i32, y: i32) => i32{
	println!("x val =  {}", x);
	println!("y val =  {}", y);
	x + y // in RUST the last expression is explicitly return so although you can do 
				// return x + y, there is no need
}
```