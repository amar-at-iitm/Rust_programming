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


This chapter introduces foundational elements of Rust: **variables, data types, functions, control flow**, and more.

### 3.1 Variables and Mutability

#### Immutable by Default:

```rust
let x = 5;
x = 6; // Compile-time error!
```

#### Use `mut` for Mutability:

```rust
let mut x = 5;
x = 6; //  Works
```

#### Shadowing:

Allows redefining a variable with the same name:

```rust
let x = 5;
let x = x + 1;
```

* Unlike `mut`, shadowing lets you **change type** too:

```rust
let spaces = "   ";
let spaces = spaces.len();
```


### 3.2 Data Types

Rust is **statically typed**: types must be known at compile time. It uses **type inference** but allows annotations.

#### Scalar Types:

* **Integer**: `i32`, `u32`, `i64`, `isize`, etc.
* **Floating point**: `f64` (default), `f32`
* **Boolean**: `true`, `false`
* **Character**: `'a'`, `'😻'` (Unicode supported)

#### Integer Literals:

```rust
let x = 98_222;   // underscore as visual separator
let y = 0xff;     // hexadecimal
let z = 0b1111_0000; // binary
```

#### Integer Overflow:

* In **debug** mode: panics.
* In **release** mode: wraps around (2’s complement).


### 3.3 Compound Types

#### Tuples:

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;
println!("{}", y); // 6.4
```

* Access by index: `tup.0`, `tup.1`

#### Arrays:

```rust
let a = [1, 2, 3, 4, 5];
let first = a[0];
```

* Arrays have **fixed size**, unlike vectors.
* Initialisation shortcut:

```rust
let b = [3; 5]; // [3, 3, 3, 3, 3]
```


### 3.4 Functions

#### Basic Function:

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

#### Notes:

* Parameter types **must be specified**.
* Naming uses **snake\_case** by convention.

#### Return Values:

* No `return` keyword needed for final expressions:

```rust
fn five() -> i32 {
    5 // No semicolon = return value
}
```

* Use `return` for early return:

```rust
return x + 1;
```


### 3.5 Comments

Rust supports **single-line comments**:

```rust
// This is a comment.
```

* [More on comments](https://doc.rust-lang.org/reference/comments.html)


### 3.6 Control Flow

#### `if` Expressions:

```rust
let number = 6;
if number % 4 == 0 {
    println!("Divisible by 4");
} else if number % 3 == 0 {
    println!("Divisible by 3");
} else {
    println!("Not divisible by 3 or 4");
}
```

* `if` is an expression and can be used like:

```rust
let condition = true;
let number = if condition { 5 } else { 6 };
```

#### Loops

##### 1. `loop`

```rust
loop {
    println!("again!");
    break;
}
```

* Infinite unless explicitly `break`.

##### 2. `while`

```rust
let mut number = 3;
while number != 0 {
    println!("{number}!");
    number -= 1;
}
```

##### 3. `for`

```rust
let a = [10, 20, 30];
for element in a.iter() {
    println!("The value is: {element}");
}
```

##### 4. `for` with range:

```rust
for number in (1..4).rev() {
    println!("{number}!");
}
```

* Efficient and safe—preferred over `while`.





## Chapter 7: Managing Growing Projects with Packages, Crates, and Modules

This chapter explores **Rust’s module system**, and how to organise code across files and directories using **modules**, **packages**, and **crates**.

### 7.1 Packages and Crates

#### What’s a Crate?

* **Crate**: A binary or library unit of compilation.
* Every Rust project is at least one **crate**:

  * Binary Crate → has a `main` function.
  * Library Crate → defines functions/types to be used elsewhere.

#### What’s a Package?

* A **Package** is a bundle that contains **zero or more crates** (one **binary** or **library** crate is required).
* A package includes a `Cargo.toml` file that defines:

  * Dependencies
  * Project metadata
  * Crate types

#### Example Package Layout:

```
my_project/
├── Cargo.toml      # defines package
└── src/
    ├── main.rs     # binary crate root (if present)
    └── lib.rs      # library crate root (optional)
```

> ! A package can contain both `src/main.rs` and `src/lib.rs`, but **only one library crate**.


### 7.2 Defining Modules to Control Scope and Privacy

#### What’s a Module?

* **Modules** allow logical grouping of code and control of **visibility** (public vs private).

#### Declaring a Module:

```rust
mod sound {
    fn guitar() {
        // private by default
    }
}
```

#### Making Public:

```rust
pub mod sound {
    pub fn guitar() {
        println!("Playing guitar");
    }
}
```

* **`mod` keyword** defines a module.
* **`pub` keyword** exposes the module or item to other scopes.

#### Module Hierarchy:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

To access `add_to_waitlist()`:

```rust
front_of_house::hosting::add_to_waitlist();
```


### 7.3 Paths for Referring to an Item in the Module Tree

#### Absolute vs Relative Paths

* **Absolute path**: Starts from the crate root

  ```rust
  crate::front_of_house::hosting::add_to_waitlist();
  ```

* **Relative path**: Starts from the current module

  ```rust
  self::hosting::add_to_waitlist();
  ```

#### Using `use` to Bring Paths into Scope

```rust
use crate::front_of_house::hosting;

fn serve() {
    hosting::add_to_waitlist();
}
```

You can also shorten:

```rust
use crate::front_of_house::hosting::add_to_waitlist;
```


### 7.4 Bringing Paths into Scope with `use`

#### Use Aliases:

```rust
use std::fmt::Result;
use std::io::Result as IoResult;
```

> Useful when different crates have functions or types with the same name.

#### Nested Paths:

```rust
use std::{cmp::Ordering, io};
```

#### Glob operator:

```rust
use std::collections::*;
```

* Imports **everything** in the module (use with care).


### 7.5 Separating Modules into Different Files

#### From Inline to File:

```rust
mod front_of_house;
```

If `mod front_of_house` is declared, Rust looks for:

* `src/front_of_house.rs`, OR
* `src/front_of_house/mod.rs`

Within `front_of_house.rs`, submodules can be added:

```rust
pub mod hosting;
```

Rust expects this structure:

```
src/
├── main.rs
└── front_of_house/
    ├── mod.rs
    └── hosting.rs
```

This supports scalable file-based project organisation.



## Practical Example (Structure and Access)

### File Tree:

```
src/
├── main.rs
└── front_of_house/
    ├── mod.rs
    └── hosting.rs
```

### main.rs:

```rust
mod front_of_house;

fn main() {
    front_of_house::hosting::add_to_waitlist();
}
```

### front\_of\_house/mod.rs:

```rust
pub mod hosting;
```

### front\_of\_house/hosting.rs:

```rust
pub fn add_to_waitlist() {
    println!("Waitlist updated");
}
```


## Chapter 9: Error Handling
### Unrecoverable Errors with panic!
### Recoverable Errors with Result
### To panic! or Not to panic!
