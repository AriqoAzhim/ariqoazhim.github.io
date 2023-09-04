Macros allow you to write code which writes other code (Meta programming)

Kind of like a function that has input that is code and output that is code that is changed in some way. 

We’ve already been using macros. e.g println!(). 

They are very similar to functions as they reduce the lines of code when trying to implement a functionality but there are differences between them

1. Functions have to declare the number of params they accept while Macros can accept variable number of params
2. Functions are called at runtime whereas Macros are expanded before your program finishes compiling
3. Macros are much more powerful than Functions, however the downside is that you’re introducing more complexity since you’re writing code which writes other code, meaning that macros will be harder to implement, read and maintain

# Declarative Macros

The most widely use form of Macros and allow you to write something similar to a match expression. Declarative Macros match again patterns and replace code with other code.

```rust
fn main() {
    let v1: Vec<u32> = vec![1, 2, 3];
    let v2: Vec<&str> = vec!["a", "b", "c", "d", "e"];
}
```

# Procedural Macros

Take code as input, operate on that code and produce code as output. Procedural Macros must be defined in their own crate with a custom crate type.

There are 3 kinds:

1. Custom Derived:
2. Attribute like:
3. Function like:

They are all defined with a similar syntax seen here:

```rust
#[some_attribute]
    pub fn some_name(input: TokenStream) -> TokenStream {
        //...
    }
```

Tokens represent the smallest individual elements of a program. They can represent keywords, identifiers, operators, separators or literals. Our function must also have an attribute which specifies the kind of procedural macro we’re creating.

## Let’s go through an example…

In the [main.rs](http://main.rs) file:

```rust
// This macro is called helloMacro which will implement
    // a trait HelloMacro which will ahve an associated function
    // with a default implem that prints out "Hello Macro"
    use hello_macro::HelloMacro;
    use hello_macro_derive::HelloMacro;

    // derive attribute specifying the macro, implementing the HelloMacro trait
    // for the pancake struct, meaning that you can call HelloMacro from 
    // the pancake struct
    #[derive(HelloMacro)]

    //custom derived macro
    struct Pancakes;

    fn main() {
        Pancakes::hello_macro();
    }
```

create a new library hello_macro with a HelloMacro trait defined

```rust
pub trait HelloMacro {
    fn hello_macro();
}
```

then while in the hello_macro lib directory create a another crate to define the custom derived macro

hello_macro_derive

in hello_macro_derive/Cargo.toml the following changes must be made

```toml
[package]
name = "hello_macro_derive"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[lib]
proc-macro = true

[dependencies]
syn = "1.0"
quote = "1.0"
```

in hello_macro_derive/src/lib.rs:

```rust
// proc_macro is a crate that comes with rust but to import the crate we need to write extern crate
extern crate proc_macro;

// Tokens represent the smallest individual elements of a program
// They can represent keywords, identifiers, operators, separators or literals
use proc_macro::TokenStream;

// allows us to take strings and turn it into rust code
use quote::quote;

// allows us to take a string of rust code and turn it into a syntax tree structure that can
// be operated on
use syn;

// functino annotated with this, indicating that it is a custom-derived macro with the name HelloMacro
#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    // Construct a representation of RUst Code as a syntax tree that we can manipulate
    // .unwrap() makes code panic if parsing fails, this is appropriate since
    // there is no Result Type being returned and since we are going to be  
    // called at compile time not runtime
    let ast = syn::parse(input).unwrap();

    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    // extract the name of the type we are working
    let name = &ast.ident;

    // use the quote macro to output some rust code
    let gen = quote! {
        // the quote macro has some templating functionality so #name will be replaced with the
        // actual name of the type
        impl HelloMacro for #name {

            // define custom imlementationf or the hello_macro associated function
            fn hello_macro() {
                println!(
                    "Hello, Macro! My name is {}",

                    // takes an expression and turn it into a string without evaluating it.
                    stringify!(#name)
                );
            }
        }
    };
    // take the output oof the quote macro, and turn it into a TokenStream by calling .into()
    gen.into()
}
```

make changes to the Cargo.toml file in the main folder to ensure that the hello_macro and hello_macro_derive crate are being imported

```toml
[package]
name = "macros"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
# local relative paths to hello_macro lib crate and hello_macro_derive proc-macro crate
hello_macro = { path = "../macros/hello_macro" }
hello_macro_derive = { path = "../macros/hello_macro/hello_macro_derive" }
```

using Cargo run in the main folder should compile all the crates and the output should look like:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ace70280-4fbb-40df-bb74-7a8b926f8679/Untitled.png)

# Attribute-like Macros

Similar to Custom Derive Macros but instead of generating code for the derive attribute you can create a custom attribute. Also custom derive macros only work on structs and enums whereas attribute-like macros can work on other  types such as functions.

Let’s say you are buidding a web framework and want to make a new route attribute that takes in a HTTP method (GET, DELETE, …) and a route. This macro will generate code which will map specific HTTP requests to a given function. In this case, mapping a GET request on the root path to the index function. You would define the attribute-like macro as such:

```rust
//Attribute-like Macros
#[route (GET, "/")]
fn index() {
    //..
}

// attribute annotation denoting the macro
#[proc_macro_attribute]
pub fn route(
    attr: TokenStream, // contains the contents of the attribute
    item: TokenStream // contains of the item the attribute is attached to 
) -> TokenStream {
    //..
}
// otherwise they work exactlly like custom derived macros
```

# Function-Like Macros

Look like function calls but they are more flexible.

1. They can take a variable number of arguments
2. Operate on Rust code

```rust
// Function-like macro

let sql = sql!(SELECT * FROM posts WHERE id=1);

// takes a sql statement as an argument, validate that it's syntax is valid
// and generate code that will allow you to execute that sql statement
// just annoted with proc_macro
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    //..
}
```