# Getting Started

These are the steps to get started

# Installation

go to site and download lol

# Cargo

The RUST Build System and Package Manager. 

to create a new project

```rust
cargo new <projectname>
```

 this will generate a folder of <projectname> with the following

- src/main.rs: rust file with HelloWorld printing by default
- cargo.toml: RUST configuration file, shows the name, version author, and edition along with any dependencies defined by the user.

then:

```rust
cargo build
```

this compiles/builds your project generating:

- cargo.lock: File specifying exact dependencies
- target directory: this has the debug stuff and the executable among other things

then:

```rust
cargo run <arg1> <..> <argx>
```

this will run the executable in your current working directory

# Useful Commands