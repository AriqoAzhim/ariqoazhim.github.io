Structs allow you to group related data together.

# Example Implementation:

```rust
// struct used to define a User 
struct User {
    username: String,
    email: String,
    sign_in_count:  u64,
    active: bool,
}

fn main() {
    // can't just make one field mutable, you have to make it all mutable
    let mut user1 = User{
        email: String::from("ariqo.azhim@gmail.com"),
        username: String::from("ricky"),
        active: true,
        sign_in_count: 1
    }

    // you can access struct values using .
    let name = user1.username;
    // you can overwrite struct values if you make it mutable
    user1.username = String::from("wallace123");

    // abstract user creation to a function
    let user2 = build_user(String::from("JohnDoe@gmail.com"), String::from("johnHoee");

    //  fill missing gaps using ..<struct instantiation>
    let user3 = User {
        email: String::from("james@gmail.com"),
        username: String::from("james"),

        // this populates this struct's remaining fields with
        // whatever is in user2
        ..user2
    }
}

fn build_user(email: String, username: String) -> User {
    User {
        // because the fields and the arguments have the same syntax we just say email and username
        // field init shorthand syntax
        email
        username
        sign_in_count: 0,
        active: true
    }
}
```

# Structs without name fields

```rust
    // You can also create structs without name fields aka tuple structs
    //e.g
    // These are useful when you want to have tuples of the same type but with different names
    // as shown here Color and Point have the same internals but have different names
    struct Color(i32, i32, i32);

    struct Point(i32, i32, i32);
```

# Printing a struct

```rust
#[derive(Debug)] // This is required by rust if you want to print the struct
struct Rectangle {
    width: u32,
    height: u32
}

fn main() {
    // define rect variable with 30, 50
    let rect = Rectangle {
        width: 30,
        height: 50
    }
    // {:#?} is used when trying to print a struct variable, otherwise it is normally {}
    println!("rect: {:#?}", rect);

    println!(
        "The area of the rectangle is {} square pixels",
        area(&rect)
    );
}
```

This will print out:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9399dba0-0284-4ed0-a880-c621916196cf/Untitled.png)

# Creating Struct Methods

You need to create an implementation block which will house all of the struct’s functions and methods. Methods make the code for organised and makes it clear to the user which functions/methods are associated to what struct/data

```rust
// for the above Rectangle struct
impl Rectangle {
    // the first argument in a method is always self, which is a reference to the 
    // instance of the struct calling the method
    // you can also take ownership of the instance or make it mutable but in this case we just use
    // a reference to it (&self)
    fn area(&self) -> u32 {
        self.width * self.height
    }
// example with 2 arguments
fn can_hold(&self, other: Rectangle) -> bool {
	self.width > other.width && self.height > other.height
}
```

We can also define associated functions in our implementation block. These would be functions that are associated with the struct but do not need a struct to be a reference. Additionally, these functions can be created within another implementation block for clarity as RUST allows a struct to have multiple implementation blocks

```rust
impl Rectangle {
    // Associated function that creates a Rectangle of equal with and height
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size
        }
    }
    
}
```