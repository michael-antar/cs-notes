A **virtual machine** that *runs Java programs* by translating platform-independent Java bytecode into machine-specific code.
## Memory
### The Java Stack
The storage area for *method execution*. It is allocated as one continuous block of memory _per thread_. It acts as a _LIFO_ structure, controlled by the **Stack Pointer (SP)** register in the CPU.
#### Stack Frames
Each method call is represented by a **stack frame**.
- **Size**: Determined at _compile time_. The compiler analyzes the code, counts the variables/parameters, and calculates the exact frame size in advance.
- **Optimization**: Small methods with fewer variables create smaller frames, allowing deeper recursion before a crash.
##### Frame Structure
A frame is _self-contained_ (it cannot access the variables of the frame below it). It contains 3 distinct sections:

1. **Local Variable Array (LVA)**: An array (index 0, 1, 2, ...) that holds parameters and variables. It uses **slots** (typically 4 bytes)
   - `int`, `float`, `boolean`, `char`, `reference`: Take _1 slot_
     - _Note_: `boolean`, `byte`, `char`, and `short` are all expanded to essentially be `int` while in the stack
   - `long`, `double`: Take _2 slots_
2. **Operand Stack**: A "mini-stack" used as a workspace for calculations
3. **Frame Data**: stores the "context" required to support the method:
   - _Return Address_: Where to go back when the method finishes
   - _Dynamic Linking_: Reference to the **constant pool** (to resolve calls to _other_ methods)
   - _Exception Table_: References to `catch` blocks if an error occurs
#### Errors (Limits)
- `StackOverflowError` **(Runtime)**: The _stack memory_ is full. Too many frames (recursion depth)
- `ClassFormatError` **(Compile Time)**: Limit to local variables
  - Limit: 65,535 ($2^16 - 1$)
  - Why: The bytecode instruction to access a variable uses a 2-byte index
### The Heap
The *runtime data area* used for **dynamic memory location**. All class instances (Objects) and arrays are allocated here. The Heap allows for random allocation and deallocation.
#### Access and Performance
**Access Speed**: $O(1)$
- The reference on the Stack holds the direct physical memory address (e.g., `0x3F...`). The CPU jumps directly to that address

**Latency**: *Slower than the Stack*
- While technically $O(1)$, it is physically slower because Heap memory is scattered, causing more CPU Cache Misses compared to the contiguous Stack

**Concurrency**: *Shared by all threads*
- Because it is shared, it is not thread-safe by default and requires synchronization (locks) if multiple threads modify the same object
#### Memory Structure (Generational)
To optimize cleanup, the Heap is divided into "generations" based on the **Weak Generational Hypothesis** (most objects die young)

**Young Generation**:
  - Where all new objects are born (`new User()`)
  - _Behavior_: GC runs frequently here ("Minor GC"). It is very fast because most objects (like HTTP request wrappers) are dead and can be wiped instantly.

**Old Generation**:
  - Objects that have survived many GC cycles. Long lived data like caches, database connections, or singleton beans
  - _Behavior_: GC runs rarely here ("Major GC"). When it does, it is slow and often triggers a "Stop the World" pause
  
**Metaspace**
  - Stores **class metadata** (static variables, method definitions). Technically sits _outside_ the main heap in native memory (since Java 8)
#### Memory Management (Garbage Collection)
**The Mechanism**:
1. **GC Roots**: The GC starts at the Stack (local variables) and static variables
2. **Mark**: It follows references (pointers) to "Mark" every live object in the Heap
3. **Sweep**: Any object _not_ marked is deleted

**Compaction**: Since objects are created and deleted at random sizes, the Heap gets full of gaps (fragmentation)
- **The Process**: The GC pauses the app, moves all live objects together to close the gaps, and _updates the references_ (pointers) in the Stack to point to the new locations
- _Note_; This is why references in Java do not become "stale" like in C++. The JVM fixes them for you during compaction
#### The String Pool
Strings are objects, so they live in the Heap. However, to save memory, Java maintains a **string constant pool** inside the Heap.
#### Errors
- `OutOfMemoryError: Java heap space`: Even after running GC, there is no room for new objects
  - _Cause_: Memory Leaks (static lists that never get cleared) or simply too much data (IoT spikes)
- `OutOfMemoryError: Metaspace`; Too many loaded classes (common in large frameworks like Spring)