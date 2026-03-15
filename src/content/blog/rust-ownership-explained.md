---
title: "Rust Ownership: What Finally Made It Click"
description: "After struggling with the borrow checker, here's the mental model that finally made Rust's ownership system make sense to me."
date: 2026-03-10
tags: ["rust"]
---

Coming from a background of cloud infrastructure and scripting, Rust's ownership model felt alien at first. After weeks of fighting the borrow checker, I finally found a mental model that made it click.

## The Core Idea

Every value in Rust has a single **owner**. When the owner goes out of scope, the value is dropped. If you pass that value into something else, like a function parmeter, that function is now the owner unless you use references.

```rust
fn main() {
    let s = String::from("hello"); // s owns the string
    takes_ownership(s);            // ownership moves into the function
    // s is no longer valid here
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
} // some_string is dropped here
```

## Why This Matters for Cloud Engineers

If you spend your days thinking about resource lifecycles, such as compute instances, database connections, storage accounts... ownership actually maps really well to that mental model. Resources have owners. When the owner is done, the resource is cleaned up. Not needed anymore? *terraform destroy*

The difference is that Rust enforces this *at compile time*, not at runtime. No GC pauses, no dangling pointers, no "oops, that connection pool was closed."

## Borrowing vs Moving

The thing that tripped me up longest was the difference between *moving* a value and *borrowing* it.

```rust
fn main() {
    let s = String::from("hello");
    let len = calculate_length(&s); // borrow with &
    println!("Length of '{}' is {}.", s, len); // s still valid!
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

The `&` means "I want to use this, but I don't want to own it." The original owner keeps the value.

## What's Next

I'm working through the Rust Book chapter by chapter alongside building small CLI tools. Next up: lifetimes. Wish me luck.
