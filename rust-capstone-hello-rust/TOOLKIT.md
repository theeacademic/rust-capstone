# Rust Development Toolkit

## Table of Contents
1. [Getting Started](#getting-started)
2. [Core Concepts](#core-concepts)
3. [Memory Management](#memory-management)
4. [Error Handling](#error-handling)
5. [Collections and Iterators](#collections-and-iterators)
6. [Concurrency](#concurrency)
7. [Testing](#testing)
8. [Cargo and Project Management](#cargo-and-project-management)
9. [Common Patterns](#common-patterns)
10. [Resources](#resources)

## Getting Started

### Installation
```bash
# Install Rust using rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env

# Verify installation
rustc --version
cargo --version
```

### Your First Rust Program
```rust
fn main() {
    println!("Hello, World!");
}
```

### Cargo Commands
```bash
cargo new my_project        # Create new project
cargo build                 # Build project
cargo run                   # Build and run
cargo test                  # Run tests
cargo check                 # Check compilation without building
cargo clean                 # Remove build artifacts
cargo doc                   # Generate documentation
cargo fmt                   # Format code
cargo clippy                # Run linter
```

## Core Concepts

### Variables and Mutability
```rust
// Immutable by default
let x = 5;
let name = "Alice";

// Mutable variables
let mut counter = 0;
counter += 1;

// Constants
const MAX_POINTS: u32 = 100_000;

// Shadowing
let x = 5;
let x = x + 1;  // x is now 6
```

### Data Types
```rust
// Scalar types
let integer: i32 = 42;
let float: f64 = 3.14;
let boolean: bool = true;
let character: char = 'Z';

// Compound types
let tuple: (i32, f64, char) = (42, 3.14, 'Z');
let array: [i32; 5] = [1, 2, 3, 4, 5];
let slice: &[i32] = &array[1..3];

// String types
let string_literal: &str = "Hello";
let owned_string: String = String::from("Hello");
```

### Functions
```rust
// Basic function
fn add(x: i32, y: i32) -> i32 {
    x + y  // No semicolon = return value
}

// Function with explicit return
fn multiply(x: i32, y: i32) -> i32 {
    return x * y;
}

// Function without return value
fn print_hello() {
    println!("Hello!");
}
```

### Control Flow
```rust
// If expressions
let number = 6;
if number % 4 == 0 {
    println!("number is divisible by 4");
} else if number % 3 == 0 {
    println!("number is divisible by 3");
} else {
    println!("number is not divisible by 4 or 3");
}

// If as expression
let condition = true;
let number = if condition { 5 } else { 6 };

// Loops
// Infinite loop
loop {
    println!("again!");
    break;
}

// While loop
let mut number = 3;
while number != 0 {
    println!("{}!", number);
    number -= 1;
}

// For loop
let a = [10, 20, 30, 40, 50];
for element in a.iter() {
    println!("the value is: {}", element);
}

// For with range
for number in (1..4).rev() {
    println!("{}!", number);
}
```

## Memory Management

### Ownership Rules
1. Each value in Rust has a variable that's called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

### Borrowing
```rust
// Immutable borrow
fn calculate_length(s: &String) -> usize {
    s.len()
}

// Mutable borrow
fn change(s: &mut String) {
    s.push_str(", world");
}

// Example usage
let mut s1 = String::from("hello");
let len = calculate_length(&s1);
change(&mut s1);
```

### Lifetimes
```rust
// Lifetime annotation
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

// Struct with lifetime
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

## Error Handling

### Result Type
```rust
use std::fs::File;
use std::io::ErrorKind;

fn open_file(filename: &str) -> Result<File, std::io::Error> {
    let f = File::open(filename);
    
    match f {
        Ok(file) => Ok(file),
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create(filename) {
                Ok(fc) => Ok(fc),
                Err(e) => Err(e),
            },
            other_error => Err(other_error),
        },
    }
}

// Using ? operator
fn read_username_from_file() -> Result<String, std::io::Error> {
    let mut s = String::new();
    File::open("hello.txt")?.read_to_string(&mut s)?;
    Ok(s)
}
```

### Option Type
```rust
fn find_word(s: &str, word: &str) -> Option<usize> {
    s.find(word)
}

// Pattern matching with Option
match find_word("hello world", "world") {
    Some(index) => println!("Found at index {}", index),
    None => println!("Not found"),
}

// Using unwrap_or
let result = find_word("hello", "world").unwrap_or(0);
```

## Collections and Iterators

### Vectors
```rust
// Creating vectors
let v: Vec<i32> = Vec::new();
let v = vec![1, 2, 3, 4, 5];

// Accessing elements
let third: &i32 = &v[2];
let third: Option<&i32> = v.get(2);

// Iterating
for i in &v {
    println!("{}", i);
}

// Mutable iteration
for i in &mut v {
    *i += 50;
}
```

### Hash Maps
```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// Accessing values
let team_name = String::from("Blue");
let score = scores.get(&team_name);

// Iterating
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

### Iterators
```rust
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter();

// Consuming adaptors
let total: i32 = v1_iter.sum();

// Iterator adaptors
let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

// Chaining
let v3: Vec<_> = v1.iter()
    .filter(|x| **x % 2 == 0)
    .map(|x| x * 2)
    .collect();
```

## Concurrency

### Threads
```rust
use std::thread;
use std::time::Duration;

let handle = thread::spawn(|| {
    for i in 1..10 {
        println!("hi number {} from the spawned thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
});

for i in 1..5 {
    println!("hi number {} from the main thread!", i);
    thread::sleep(Duration::from_millis(1));
}

handle.join().unwrap();
```

### Message Passing
```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();
});

let received = rx.recv().unwrap();
println!("Got: {}", received);
```

### Shared State Concurrency
```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    });
    handles.push(handle);
}

for handle in handles {
    handle.join().unwrap();
}

println!("Result: {}", *counter.lock().unwrap());
```

## Testing

### Unit Tests
```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }

    #[test]
    fn it_adds_two() {
        assert_eq!(add_two(2), 4);
    }

    #[test]
    #[should_panic]
    fn it_panics() {
        panic!("Make this test fail");
    }
}

fn add_two(x: i32) -> i32 {
    x + 2
}
```

### Integration Tests
```rust
// tests/integration_test.rs
use my_crate::add_two;

#[test]
fn it_adds_two_integration() {
    assert_eq!(4, add_two(2));
}
```

## Cargo and Project Management

### Cargo.toml
```toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2021"
authors = ["Your Name <your.email@example.com>"]
description = "A sample Rust project"

[dependencies]
serde = "1.0"
tokio = { version = "1.0", features = ["full"] }

[dev-dependencies]
tempfile = "3.0"

[profile.release]
opt-level = 3
lto = true
```

### Workspaces
```toml
# Cargo.toml (workspace root)
[workspace]
members = [
    "crates/adder",
    "crates/calculator",
]

# crates/adder/Cargo.toml
[package]
name = "adder"
version = "0.1.0"
edition = "2021"

[dependencies]
calculator = { path = "../calculator" }
```

## Common Patterns

### Builder Pattern
```rust
pub struct Person {
    name: String,
    age: Option<u32>,
    email: Option<String>,
}

impl Person {
    pub fn new(name: String) -> Person {
        Person {
            name,
            age: None,
            email: None,
        }
    }

    pub fn age(mut self, age: u32) -> Person {
        self.age = Some(age);
        self
    }

    pub fn email(mut self, email: String) -> Person {
        self.email = Some(email);
        self
    }
}

// Usage
let person = Person::new("Alice".to_string())
    .age(30)
    .email("alice@example.com".to_string());
```

### RAII (Resource Acquisition Is Initialization)
```rust
struct DatabaseConnection {
    // Connection details
}

impl DatabaseConnection {
    fn new() -> Self {
        // Initialize connection
        DatabaseConnection { /* ... */ }
    }
}

impl Drop for DatabaseConnection {
    fn drop(&mut self) {
        // Clean up connection
        println!("Closing database connection");
    }
}
```

## Resources

### Official Documentation
- [The Rust Programming Language Book](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [Cargo Book](https://doc.rust-lang.org/cargo/)

### Learning Resources
- [Rustlings](https://github.com/rust-lang/rustlings) - Interactive Rust exercises
- [Exercism Rust Track](https://exercism.org/tracks/rust) - Practice problems
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/)

### Tools and IDEs
- **VS Code**: Install the `rust-analyzer` extension
- **IntelliJ IDEA**: Install the Rust plugin
- **Vim/Neovim**: Use `rust-analyzer` LSP
- **Emacs**: Use `rust-mode` and `lsp-mode`

### Useful Crates
- `serde` - Serialization/deserialization
- `tokio` - Async runtime
- `reqwest` - HTTP client
- `clap` - Command line argument parsing
- `anyhow` - Error handling
- `thiserror` - Error types
- `log` - Logging
- `env_logger` - Logging implementation

### Community
- [Rust Users Forum](https://users.rust-lang.org/)
- [Reddit r/rust](https://reddit.com/r/rust)
- [Rust Discord](https://discord.gg/rust-lang)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/rust)

### Best Practices
1. **Use `cargo fmt`** to format your code consistently
2. **Run `cargo clippy`** to catch common mistakes
3. **Write tests** for your code
4. **Use meaningful variable names**
5. **Handle errors properly** with `Result` and `Option`
6. **Document public APIs** with doc comments
7. **Use `cargo doc`** to generate documentation
8. **Profile your code** before optimizing

### Common Commands Cheat Sheet
```bash
# Project management
cargo new project_name
cargo init
cargo build
cargo run
cargo test
cargo check

# Code quality
cargo fmt
cargo clippy
cargo doc --open

# Dependencies
cargo add crate_name
cargo update
cargo tree

# Publishing
cargo login
cargo publish
```

Remember: Rust's compiler is your friend! It catches many errors at compile time that other languages would only discover at runtime. Embrace the borrow checker and let it guide you toward safe, efficient code.
