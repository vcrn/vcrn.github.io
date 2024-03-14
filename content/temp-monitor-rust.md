+++
title = "Temperature monitor for Raspberry Pi written in Rust"
date = 2021-11-03

[taxonomies]
tags = ["Rust", "Raspberry Pi"]
+++

I decided to build a resource monitoring tool in Rust for Raspberry Pi, starting with the monitoring of component temperatures since I have previously written one in Python. In this post, I’ll break down the code related to the temperature monitor (which can be found [here](https://github.com/vcrn/temp-monitor-raspberry-pi-rust)) to a more detailed level than usual since you need to be more exhaustive in how you write the code in Rust than in comparison with Python. The advantage of having this application written in Rust instead of Python is that it will need less computer resources to run. At the end of this post, the source code for both the temperature monitoring app and the full resource monitoring app can be found. 
<!-- more -->

# print!() vs. println!()

To print text, you can use `println!()` to print what is given to it, followed by a new line. If you don't want to append a new line, `print!()` is usually described as `println!()` without the new line character. But, using `print!()` won’t actually print anything until a new line character is encountered or until the standard output stream (stdout) is flushed. Thus, `print!()` needs to be followed by `io::stdout().flush()`.

# String vs. &str

In Rust, there are two types which can handles strings: the `String` and `&str`.

The `String` type acts in many respects like a vector. The data is stored in the same way as a vector, but can only contain a sequence of UTF-8 symbols. The data is allocated on the heap, while a pointer to this data along with its length and capacity is stored together on the stack. The String can grow in size. A variable which stores text entered by the user at run time is stored as a String, since its size is unknown at compile time.

The `&str` type, also known as "string slice", is a reference to a part of a String. Since it is a reference, it is immutable. When a variable is assigned the value of a text string present in the source code, as in the following example, it is known as a string literal and is also of type `&str`:

```rust
let some_str = "Some text";
```

At run time, "Some text" is present in the executable, and `some_str` is stored on the stack with a pointer to this piece of code present in the executable. `some_str` is referring to this section of read-only memory, and is thus classified as a `&str`.

# Ownership

To handle allocation of memory, Rust uses a concept called `Ownership`. The rules of ownership in Rust are the following:
* Each value in Rust has a variable that’s called its owner.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

This has different effects on the variables and data depending on how it is stored. These two snippets of code will act quite differently:

```rust
let a = 1; // Assigns 1 to a.
let b = a; // Makes a copy of a and assigns the value (1) to b.
```
```rust
let s1 = String::from("hello"); // Assigns the String value "hello" to s1.
let s2 = s1; // Moves the ownership of the value from s1 to s2,
	     // since this value cannot have two owners.
```

The reason for this difference is that the former value, `1`, is a primitive data type of a known and fixed size which is stored on the stack, while the latter is stored on the heap. To make a copy of `s1` to `s2`, this needs to be explicitly stated. What makes it more complicated is that giving `s1` as a parameter to a function would also move the ownership to inside that function. To make sure that `s1` can continue to be used after this step, without moving the ownership, it can be passed as a reference. This is done by preceding the variable with a `&`, i.e. `&s1`, and is called borrowing.

# Running terminal commands

To run a terminal command, the keyword Command needs to be used and is imported via `use std::process::Command`. To run the command `vcgencmd` with the argument `measure_temp`, the following line is used:

```rust
Command::new("vcgencmd").arg("measure_temp");
```

And to return the output, `.output()` is appended. In this project, it used once for reading GPU temperature, and the second time to read the CPU temperature.

But we still have some steps left to make the returned data  comprehendable for a human. To do this, a new concept needs to be introduced.

# enum

Like other languages, it contains the keyword `enum`, but this concept varies in comparison with other common languages, like C++, for example. `enum` is used to create a data type which can be one of several different variants. A simple example:

```rust
enum Drink {
    Coffee(String),
    Tea(String),
}
```

The enum `Drink` can either be of variant `Coffee` or `Tea`, both containing a String value. You could also include more variants, containing other types of values.

In the code for monitoring the Raspberry Pi, a built-in enum called `Result` is used, which looks like:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

There are two variants, one called `Ok` (containing a successful value of an operation, which is of generic type `T`) and the other `Err` (containing the error value, which is of generic type `E`). Them being of generic type `T` and `E` only means that the values contained in `Ok` and `Err` can, but don’t have to, be of different types. To get access to the value (of type `T` or `E`), either `.expect()` or `.unwrap()` may be used, which differ slightly: if either of them returns an `Err`, the program panics, the difference being that `.expect()` can be used to display en error message in that case, such as `.expect("Some error message")`. The code

```rust
Command::new("vcgencmd").arg("measure_temp").output();
```

returns a `Result`, which may either be an `Ok(T)` or `Err(E)`. To get access to the value of type `T` (or displaying an error message if an `Err` is returned), it is written as

```rust
let gpu_temp_output = Command::new("vcgencmd").arg("measure_temp").output().expect("Failed to execute command");
```

# Source

The source code for the app can be found [here](https://github.com/vcrn/temp-monitor-raspberry-pi-rust). The source code for the extended resource monitoring app can be found [here](https://github.com/vcrn/feo).
