+++
title = "Structure of a Rust project"
date = 2023-05-23

[taxonomies]
tags = ["Rust"]
+++

It can be a bit confusing to understand how to structure a Rust project once it starts to grow 
beyond some Rust files directly located in `src/` that are only accessed from `main.rs` or 
`lib.rs`. This is a simple overview of how a Rust project can be structured. For a more elaborate 
explanation of how to structure Rust projects, read chapter 8 of the book "Programming Rust: 
Fast, Safe Systems Development". 
<!-- more -->

# Crates and Packages
As per the Rust book, chapter 7, the crate is the smallest amount of code that the Rust compiler
considers at a time. This _could_ _be_ just one `.rs`-file. There are 2 types of crates: binary 
crates, which are compiled into executable binaries; and library crates, which are compiled 
into libraries meant to be used by other libraries or executables.

A package is a bundle of one or more crates that provides a set of functionality, and 
contains in its `root` a `Cargo.toml` manifest file with the section `[package]`.

## The Binary Crate
A binary crate needs to contain a `main` function or some other function marked as an
entry point: In this text, it's assumed that only `main` functions are used as entry 
points. There are designated default paths of files that `Cargo` will analyze for 
entry points, but these can be customized in `Cargo.toml`. Running `cargo build` will
compile the crates in the package, and `cargo run` will try to run any executable 
binaries.

### Build
Running `cargo build` will look for the following files:
* `src/main.rs`
* `src/bin/*.rs`
* `src/bin/*/main.rs`
Here's an example of a project files of all these kinds:
```
├── src
│   ├── main.rs
│   └── bin
│       ├── foo.rs
│       ├── bar.rs
│       └── qux
│           └── main.rs
└── Cargo.toml
```
The files with the `.rs`-extension need to contain a `main` function, which will be the entry 
point in the respective file. Let's say that the `name` field under the `package` section in the
`Cargo.toml` is `zwy`, and that the `[[bin]]` section is missing. In this case, the following
executable binaries will be generated:

| Name    | Entry point         |
|---------|---------------------|
| zwy     | src/main.rs         |
| foo     | src/bin/foo.rs      |
| bar     | src/bin/bar.rs      |
| qux     | src/bin/qux/main.rs |

Note that the following two files are not allowed to co-exist during compilation, and will lead 
to an error:
* `src/bin/<name>.rs`
* `src/bin/<name>/main.rs`

The binaries that were compiled using entry points located in `src/bin` can be given other names
by adding `[[bin]]` sections in `Cargo.toml`. For example, the following `Cargo.toml`:
```toml
[package]
name = "zwy"
version = "0.1.0"
edition = "2021"

[[bin]]
name = "oof"
path = "src/bin/foo.rs"

[[bin]]
name = "xuq"
path = "src/bin/qux/main.rs"
```
will results in these binaries:

| Name    | Entry point         |
|---------|---------------------|
| zwy     | src/main.rs         |
| oof     | src/bin/foo.rs      |
| bar     | src/bin/bar.rs      |
| xuq     | src/bin/qux/main.rs |

It's possible to omit the `path` field, in which case the path will be assumed to be 
`src/bin/<name>.rs` or `src/bin/<name>/main.rs`. When the `path` field is omitted, compilation
will fail if no file is found at either of these locations, and is mostly useful to ensure 
that the corresponding binary indeed was compiled and using one of these entry points.

### Run
Running `cargo run` with multiple binaries available will fail, unless the `default-run` field 
under the `[package]` section in `Cargo.toml` is specified. Otherwise, the binary will have to 
be specified in the command, such as `cargo run --bin bar` to run the `bar` binary. The same 
behaviour would be achieved with `cargo run` if `Cargo. toml` contains the following:
```toml
[package]
default-run = "bar"
```

## The Library Crate and Modules
Library crates lack a `main` function. The default library crate is `src/lib.rs`, but custom paths
and names can be specified in `Cargo.toml` by adding a `[lib]` section. In the text, the library 
crate is assumed to be `src/lib.rs`.

Modules are `.rs-files` that aren't crates, and these can contain a collection of items such as 
functions, structs, enums, traits, other modules, etc. 

Both `src/lib.rs` and `src/main.rs` have special privilege in that they by default can access 
all `src/*.rs` and `src/*/mod.rs` files. Consider the following structure:
```
├── src
│   ├── main.rs
│   ├── lib.rs
│   ├── foo.rs
│   ├── bar
│   │   ├── mod.rs
│   │   └── baz.rs
│   └── bin
│       └── qux.rs
└── Cargo.toml
```
Note that `src/bar/mod.rs` can be replaced with `src/bar.rs`. These cannot co-exist, and will
behave in the same manner (what access they are granted by default, how they are accessed, etc). Note also 
that `src/bin/qux.rs` could be replaced with `src/bin/qux/main.rs` and that this directory can be further 
populated by modules and sub-directories containing sub-modules, just like `src/bar` can be. 

By default, `main.rs` and `lib.rs` will be able to access the content of `foo.rs` and `bar/mod.rs`,
but not `bar/baz.rs` directly. To access `foo.rs` and `bar/mod.rs`, add
```rust
// In `main.rs` or `lib.rs`
mod foo;  // Grants access to `foo.rs` 
mod bar;  // Grants access to `bar/main.rs`
```
Only `bar/mod.rs` will be able to access `bar/baz.rs`, and `bar/mod.rs` won't be able to access any
other content of `src/`. By adding 
```rust
// In `bar/mod.rs`
mod baz;
```
to `bar/mod.rs`, it can access `bar/baz.rs`. Neither `foo.rs` nor `bar/baz.rs` will be able to 
access any content of `src/`. To allow `main.rs` and `lib.rs` to access `bar/baz.rs`, it can be 
done via `bar/mod.rs`, by adding 
```rust
// In `bar/mod.rs`
pub mod baz;
```
to it. Now `bar/mod.rs` extends the visibility of `bar/baz.rs` to `lib.rs` and `main.rs`, and it can
be accessed from there via `bar::baz`. The scope to which `bar/baz.rs` is made visible can be 
controlled by specifying one using `pub(<scope>) mod baz;`.

To enable modules to access another another module, it can be done via `lib.rs` or `main.rs`. By 
adding
```rust
// In `main.rs` or `lib.rs`
pub mod bar;
```
`foo.rs` will now be able to access the `bar` module. This, however, isn't done via the `mod bar;`,
but via
```rust
// In `src/foo.rs`
use crate::bar;
```
since the access is granted via the library or binary crate. In this scenario, `bar` will also be 
visible to binary crates in `src/bin/`, but in order to access `bar` from there, the following needs
to be added:
```rust
// In `src/bin/qux.rs`
use zwy::bar;  // `zwy` is the `name` field of `[package]` in `Cargo.toml`
```
Changing `pub mod bar;` in `lib.rs` to `pub(crate) mod bar;` will make it visible only inside the 
library crate (for example from the `foo.rs` module), but not from any binary crates in `src/bin/`.

# Workspaces
A workspace is a set of packages that are managed together. A workspace with the following structure
```
├── foo
│   ├── src.rs
│   │   ├── main.rs
│   │   └── qux.rs
│   └── Cargo.toml
├── bar
│   ├── src.rs
│   │   ├── lib.rs
│   │   └── baz.rs
│   └── Cargo.toml
└── Cargo.toml
```
has 2 packages, `foo` and `bar`. These are called its members, and are defined in the `/Cargo.toml`
with
```toml
[workspace]
members = [
    "foo",
    "bar",
]
```
There's a lot of other fields that can be specified, but I won't go into further detail than just
mentioning the existence of workspaces.
