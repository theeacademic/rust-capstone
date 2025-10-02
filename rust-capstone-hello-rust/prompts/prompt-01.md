# Prompt Journal - Entry 01

## Original Prompt
Create a new repository structure for a Rust capstone project with the following layout:

```
rust-capstone-hello-rust/
├── .gitignore
├── README.md
├── TOOLKIT.md   # full toolkit markdown content
├── hello_rust/  # Cargo project (use `cargo new hello_rust`)
│   ├── Cargo.toml
│   └── src/main.rs
└── prompts/
    ├── prompt-01.md
    ├── prompt-02.md
    └── prompt-03.md
```

Requirements:

* Fill `README.md` with quick start instructions (install Rust, run `cargo run`).
* Fill `TOOLKIT.md` with the full markdown content from the toolkit document.
* In `hello_rust/src/main.rs`, include the Rust Hello World program:

  ```rust
  fn main() {
      println!("Hello, World from Rust!");
  }
  ```
* Add `.gitignore` with entries for `/target`, `Cargo.lock`, `.idea`, `.vscode`.
* Place the prompt journal files (`prompt-01.md`, `prompt-02.md`, `prompt-03.md`) in the `prompts/` folder, each containing the respective prompt, AI summary, and reflection.

Initialize as a Git repo with an initial commit.

## AI Summary
The AI successfully created a complete Rust capstone project structure including:

1. **Directory Structure**: Created the main project directory with all required subdirectories
2. **Configuration Files**: 
   - `.gitignore` with Rust-specific entries (target/, Cargo.lock, IDE files)
   - `Cargo.toml` with proper package configuration
3. **Documentation**:
   - `README.md` with comprehensive quick start instructions including Rust installation
   - `TOOLKIT.md` with extensive Rust learning resources and examples
4. **Source Code**: `main.rs` with the requested Hello World program
5. **Prompt Journal**: Created the first prompt journal entry documenting the process

The AI encountered that Cargo wasn't installed on the system but proceeded to create the project structure manually, which is perfectly valid for a learning project.

## Reflection
This initial setup demonstrates the importance of having a well-structured project foundation. The comprehensive `TOOLKIT.md` will serve as an excellent reference throughout the Rust learning journey. The modular structure with separate prompt journals will help track progress and learning insights over time.

Key takeaways:
- Project structure matters for maintainability
- Documentation is crucial for learning projects
- Even without Cargo installed, we can create valid Rust project structures
- The prompt journal approach will be valuable for tracking learning progress
