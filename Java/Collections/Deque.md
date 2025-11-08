# Deque<E>

**Double Ended Queue**. Lets you add or remove elements from **both the front and the back**.

Can act as both a _stack_ and a _queue_.

## Core Methods

Deque methods have two names for everything since one set is "stack-like" and the other is "queue-like"

| Action                | "Stack" Method | "Queue" Method           | What It Does                                |
| :-------------------- | :------------- | :----------------------- | :------------------------------------------ |
| **Add to Front**      | `push(e)`      | `addFirst(e)`            | Adds an element to the head.                |
| **Remove from Front** | `pop()`        | `pollFirst()`            | Removes and returns the head element.       |
| **Look at Front**     | `peek()`       | `peekFirst()`            | Looks at the head element without removing. |
|                       |                |                          |                                             |
| **Add to Back**       | (N/A)          | `addLast(e)` or `add(e)` | Adds an element to the tail.                |
| **Remove from Back**  | (N/A)          | `pollLast()`             | Removes and returns the tail element.       |
| **Look at Back**      | (N/A)          | `peekLast()`             | Looks at the tail element without removing. |

## Implementations

| Class               | When to Use                                                                                           | How it works                                    | Performance                                       |
| :------------------ | :---------------------------------------------------------------------------------------------------- | :---------------------------------------------- | :------------------------------------------------ |
| **`ArrayDeque<E>`** | **Default / Fastest**. Use 99% of the time.                                                           | **Circular Array** with `head`/`tail` pointers. | **$O(1)$ Amortized**. Fast operations, low memory |
| **`LinkedList<E>`** | **Special Cases**. _Only_ if you have a massive list that are _frequently_ deleted from the _middle_. | **Doubly-Linked List**                          | **$O(1)$ Consistent**. High memory                |

## Note

- **`Stack` (Legacy Class)**: `extends Vector` (synchronized), which uses thread safety locks. Very slow.
- **`ArrayDeque` (Modern Class)**: Not synchronized, _dramatically_ faster for all stack operations in a single-threaded setting.

Always use `Deque<E> stack = new ArrayDeque<>();` instead of `Stack<E> stack = new Stack<>();`
