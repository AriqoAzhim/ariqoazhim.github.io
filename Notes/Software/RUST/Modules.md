As our project grows we need a way to organise and encapsulate our code.

To address this, RUST has  a module system which starts with a package.

running

```rust
cargo new
```

creates a new package and a Package stores crates which can either be a 

- binary crate: code you can execute
- library crate: code that can be used by other programs

Crates then in turn contain modules that allow you to organise a chunk of code and control the privacy rules. For example, You could have a crate that contained a module that dealt with authentication. You are able to set it to private so that it is not exposed to the rest of your codebase and expose public functions that would be used such login() which would need to be accessed through a path to the function.

RUST also has something called workspaces which are meant for very large projects and allow you to store interrelated packages inside the workspace.

Packages store crates and these crates can be defined in the cargo.toml file. By default there are no crates defined in the .toml file. However, this doesn’t mean that there are no crates in the package. RUST follows the convention that if there is a src/main.rs file then there will be a binary crate with same name as the package, where src/main.rs is the crate route. The crate route is the source file the RUST compiler starts at during buildtime. It also makes up the root module of your crate.

There is  also a similar convention for library crates, when there is a src/lib.rs file then RUST will create a library crate with the same name as your package and src/lib.rs would be that crate’s route.

# Package rules

1. There has to be at least one crate in a package
2. There can only be none or one library crate in a package
3. There can be any number of binary crates in a package
4. If you were to create more than one binary crate they are to be added to a folder in src named bin. e.g. a new binary file would have the route src/bin/new_bin.rs

# Generating a package with a library crate

run:

```rust
cargo new --lib <package-name>
```

A test module will be added automatically into the src/lib.rs. Ctrl + A and delete it to start adding to your library

# Example Restaurant library

This package will contain functionality to help with running a restaurant. It will have a Front Of House and Back of House which will each have functionality tied to them. This tree shows the module tree for the front_of_house module.

There is a crate module that is created by default serving as the root of the tree then we have a front_of_house module which houses two other modules, hosting and serving. Then these modules have their own function within them.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad72a3f3-9b6b-4cda-8577-333db2280486/Untitled.png)

```rust
mod front_of_house {
    // modules can contain other modules within them as well as
    // structs, enums, constants, traits etc
    mod hosting {
        fn add_to_waitlist() {
        }
        fn seat_at_table() {
        }
    }
}

mod serving {
    fn take_order() {
    }

    fn serve_order() {
    }

    fn take_payment() {
    }
}
```

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist(){
            //..
        }
    }
}

pub fn eat_at_restaurant() {
    //absolute path
    crate::front_of_house::hosting::add_to_waitlist();
    //relative path
    front_of_house::hosting::add_to_waitlist();
}
```

By default things within a module are private relative to the parent module. However, child modules are able to see what is in their parent modules. As seen above the pub was added to the hosting module and add_to_waitlist() to make the viewable by eat_at_restaurant().

super:: is used to reference the parent of a module to access it’s function/methods as shown here

```rust
fn serve_order() {}

mod back_of_house{
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order();
    }

    fn cook_order() {}
}
```

# Module privacy rules

By default the fields of a struct are private, hence to change them after the fact, you have to use the pub modifier when defining the field item.

```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String, // pub modifier used so that it can be changed
        seasonal_fruit: String, // private field by default so you can't define a struct 
                                // outside of the back_of_house module i.e in the eat_at_restaurant fn
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches")
            }
        }
    }
}

pub fn eat_at_restaurant() {
    let mut meal = back_of_house::Breakfast::summer("Rye");

    meal.toast = String::from("Wheat");
}
```

for an enum however, all fields are public if you make the enum public

```rust
mod back_of_house {
    pub enum Appetiser {
        Soup,
        Salad
    }
}

pub fn eat_at_restaurant() {
    let order1 = back_of_house::Appetiser::Soup;
    let order2 = back_of_house::Appetiser::Salad;
}
```

# The `use` Keyword