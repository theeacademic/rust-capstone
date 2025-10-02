# Rust Capstone - Hello Rust

A simple Rust capstone project demonstrating basic Rust programming concepts.

## Quick Start

### Prerequisites

1. **Install Rust**: Visit [rustup.rs](https://rustup.rs/) and follow the installation instructions for your operating system.
   
   On Linux/macOS, you can install Rust by running:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source ~/.cargo/env
   ```

   On Windows, download and run the installer from the website.

2. **Verify Installation**: Check that Rust is properly installed:
   ```bash
   rustc --version
   cargo --version
   ```

### Running the Project

1. **Navigate to the project directory**:
   ```bash
   cd hello_rust
   ```

2. **Run the Hello World program**:
   ```bash
   cargo run
   ```

   Expected output:
   ```
   Hello, World from Rust!
   ```

3. **Build the project** (optional):
   ```bash
   cargo build
   ```

4. **Run tests** (if any):
   ```bash
   cargo test
   ```

## Project Structure

```
rust-capstone-hello-rust/
├── .gitignore
├── README.md
├── TOOLKIT.md
├── hello_rust/          # Cargo project
│   ├── Cargo.toml       # Project configuration
│   └── src/
│       └── main.rs      # Main source code
└── prompts/             # Prompt journal files
    ├── prompt-01.md
    ├── prompt-02.md
    └── prompt-03.md
```

## Next Steps

- Explore the `TOOLKIT.md` file for comprehensive Rust learning resources
- Review the prompt journal files in the `prompts/` directory
- Experiment with modifying the code in `hello_rust/src/main.rs`
- Learn more about Rust at [doc.rust-lang.org](https://doc.rust-lang.org/)
