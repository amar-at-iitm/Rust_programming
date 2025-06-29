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

## Chapter 2: Programming a Guessing Game
[link to my code]()

This chapter introduces real-world Rust through a small CLI program: a number guessing game. It covers **project setup**, **standard input/output**, **crates**, **error handling**, **control flow**, and **variable parsing**.


### 2.1 Setting Up a New Project

#### Using `cargo new`:

```bash
$ cargo new guessing_game
$ cd guessing_game
```

This creates:

* `Cargo.toml`: metadata & dependencies
* `src/main.rs`: program entry point
* Git repo initialised by default

You run the app with:

```bash
$ cargo run
```

#### Note:

`cargo build` compiles the project without running it. Cargo automatically tracks dependencies and only recompiles when files change.


### 2.2 Processing a Guess

#### Reading User Input:

```rust
use std::io;

fn main() {
    println!("Guess the number!");
    println!("Please input your guess.");

    let mut guess = String::new();
    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

#### Key Concepts:

* **`use std::io`**: Brings I/O library into scope.
* **`let mut guess = String::new()`**: Creates a mutable empty string.
* **`read_line(&mut guess)`**: Reads input from stdin into the string.
* **`.expect(...)`**: Handles error if reading input fails.
* **String Interpolation**: `{guess}` uses Rust's formatting syntax.


### 2.3 Generating a Secret Number

#### Add External Crate (`rand`)

* Add this to `Cargo.toml`:

```toml
[dependencies]
rand = "0.8"
```

* Use it in your program:

```rust
use rand::Rng;

let secret_number = rand::thread_rng().gen_range(1..=100);
```

#### Key Concepts:

* **`rand::Rng`**: Trait that allows number generation.
* **`thread_rng()`**: Gets a random number generator local to the current thread.
* **`gen_range(start..=end)`**: Generates a number inclusive of both bounds.


### 2.4 Comparing the Guess to the Secret Number

#### Parsing and Matching:

```rust
use std::cmp::Ordering;

let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");

match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => {
        println!("You win!");
    }
}
```

#### Key Concepts:

* **`trim()`**: Removes `\n` or spaces from input.
* **`parse::<u32>()`**: Converts String to `u32`.
* **Error handling with `.expect(...)`** if parsing fails.
* **`cmp()`**: Compares numbers, returns an `Ordering` enum.
* **`match`**: Rust's powerful pattern matching.


### 2.5 Allowing Multiple Guesses with a Loop

#### Add Loop:

```rust
loop {
    // prompt, read, parse, compare
}
```

Add a `break;` when the guess is correct:

```rust
Ordering::Equal => {
    println!("You win!");
    break;
}
```

This keeps the game running until the correct number is guessed.


### 2.6 Handling Invalid Input

Instead of crashing on invalid input, gracefully skip:

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

#### Why?

* Prevents the program from panicking when the user types "abc".
* Introduces **non-panicking error handling** using `match`.



## Chapter 3: Common Programming Concepts
[Important Keywords](https://doc.rust-lang.org/book/appendix-01-keywords.html)

[Operators and Symbols](https://doc.rust-lang.org/book/appendix-02-operators.html)


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
