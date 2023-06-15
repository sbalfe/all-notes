# Getting Started

`rustc` $\to$ compile `.rs` file

`println!` $\to$ print statement.

## Cargo

- Build system and package manager.
  - Build code.
  - Manage dependencies.
  - Downloading and building libraries.
- `cargo new <project>` $\to$ create new cargo project.
- `cargo.toml` 
  - `[package]` $\to$ heading to say next few lines are **configuring a package**.
  - `[dependencies]` $\to$ List dependencies for our project to run.
- **==crates==** $\to$ packages of code to import 
- `src` $\to$ where our code is written.
  - Convert standalone rust file to cargo project by moving `.rs` $\to$ `src`.
- `cargo build` $\to$ creates executable in `target/debug/<project>` 
  - Run with `./` 
- `cargo run` $\to$ compile and run in **one command**.
- `cargo check` $\to$ check code for correct compilation before creating executable.
- `cargo build --release` $\to$ build for **release**.

