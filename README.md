# Rust_programming
My journey through Rust programming.

For the initial learning, I only followed the [official book](https://doc.rust-lang.org/book/ch00-00-introduction.html) of Rust programming.
### What is Rust?
Rust is a systems programming language focused on performance, safety, and concurrency. Unlike other low-level languages like C/C++, Rust guarantees memory safety without a garbage collector.

**Key Features:**
- Ownership model – ensures memory safety.
- Concurrency without data races.
- Type inference.
- Zero-cost abstractions.

## Chapter 1: Getting Started

### 1.1 Installing Rust

* **Toolchain via `rustup`**: Installs the latest Rust compiler (`rustc`), `cargo`, and `rustfmt`, with seamless updates and offline documentation access.
* Supports **Linux, macOS, Windows**, and works consistently across platforms .

### 1.2 Hello, World!

* Write `fn main() { println!("Hello, world!"); }` in `main.rs`.
* Compile/run with:

  ```bash
  $ rustc main.rs
  $ ./main
  ```

  But generally use Cargo in projects.

### 1.3 Hello, Cargo!

* **Cargo** is Rust’s official package manager and build system—handles creating projects, building, testing, and managing dependencies.
* `cargo new project_name --bin` sets up:

  * `Cargo.toml` (metadata, dependencies)
  * `src/main.rs`
  * a `.gitignore` and initialises a Git repo.
* `Cargo.toml` includes fields like `name`, `version`, `edition`, and `[dependencies]`.

### 1.4 building and running with Cargo

* Replace manual `rustc` usage with:

  ```bash
  $ cargo build           # compile in debug mode
  $ cargo run             # build + run
  $ cargo build --release # optimized build
  ```
* `cargo build` supports flags like `--bin`, `--lib`, `--features`, etc.

### 1.5 rustfmt – Formatter

* **rustfmt** formats code: enforces a consistent style using conventional guidelines (e.g., snake\_case naming).
* Can be run manually or integrated with Cargo workflows.

### 1.6 Tools & Ecosystem

* **`rustc`**: the compiler.
* **`rustup`**: version/toolchain manager with offline docs: `rustup doc`.
* **`cargo`**: handles project creation, building, testing, and publishing.
* **`rustfmt`**: code formatter.
* **IDE integrations**: Rust Language Server (RLS)/Rust Analyser for editor features.
* **Crates.io**: the official library registry.
* **editions**: Set in `Cargo.toml`, e.g., `edition = "2024"`.

### 1.7 Summary (Why Rust & How to Start)

* **Why Rust?**
  Combines **performance**, **memory safety**, and **concurrency** without GC—preventing bugs at compile time.
* **Getting up and running**: Installed via `rustup`; write and run first code; move to Cargo-powered workflows.
* **Quality tooling**: Cargo, rustfmt, and IDE support promote a clean, reliable development experience.


## Chapter 2: Programming a Guessing Game
[link to my code]()


## Chapter 3: Common Programming Concepts
### Variables and Mutability
### Data Types
### Functions
### Comments
### Control Flow



## Chapter 7: Managing Growing Projects with Packages, Crates, and Modules
### Packages and Crates
### Defining Modules to Control Scope and Privacy
### Paths for Referring to an Item in the Module Tree
### Bringing Paths Into Scope with the use Keyword
### Separating Modules into Different Files

## Chapter 9: Error Handling
### Unrecoverable Errors with panic!
### Recoverable Errors with Result
### To panic! or Not to panic!
