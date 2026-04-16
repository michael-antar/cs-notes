#TODO 
A recently developed programming language that prioritizes *memory safety*, *speed*, and **fearless concurrency**. It achieves this by having a very **strict compiler**.

Its primary goal is to achieve the raw performance and *low-level memory control* of C and C++ but without the notorious memory vulnerabilities (like buffer overflows or dangling pointers). It also achieves this *without using a Garbage Collection system*.

## Base Mechanics
Rust relies on a few fundamental concepts to replace traditional memory management.

### Ownership & Borrowing
Instead of having a GC or manually allocating/freeing memory (like `malloc`/`free`), Rust enforces a set of *rules* using a component called the **Borrow Checker**.
1. Each value in Rust has a variable called its *owner*
2. There can only be *one owner* at a time
3. When the owner goes out of scope, the value is immediately dropped from memory.
### No Nulls, No Exceptions

### Traits

