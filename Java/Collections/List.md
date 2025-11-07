# List<E>

An **ordered** collection of elements. Indexed based, allows for duplicates.

## Core Methods

### Basic Operations

- `add(E element)`: Adds an element to the **end** of the list
- `get(int index)`: Gets the element at a specific index.
- `size()`: Returns the number of elements in the list.

### Modifying the List

- `add(int index, E element)`: Inserts an element at a specific index, shifting others down.
- `set(int index, E element)`: **Replaces** the element at a specific index.
- `remove(int index)`: Removes the element at a specific index, shifting others up.
- `remove(Object o)`: Removes the _first occurence_ of a matching element.
- `clear()`: Empties the entire list.

### Checking the State

- `contains(Object o)`: Returns `true` if the list contains the element.
- `indexOf(Object o)`: Returns the index of the _first_ occurrence of the element (or -1).
- `lastIndexOf(Object o)`: Returns the index of the _last_ occurrence of the element (or -1).
- `isEmpty()`: Returns `true` if the list is empty.

### Iterating / Looping

- **For-Each Loop**
  ```java
  for (String s : myList) {
      System.out.println(s);
  }
  ```
- **Index Loop**:
  ```java
  for (int i = 0; i < myList.size(); i++) {
      System.out.println(i + ": " + myList.get(i));
  }
  ```
- **`iterator()`**

## Implementation

### ArrayList

**Default / Fastest for Access**. It's a "dynamic array"

- $O(1)$ for `get(index)` and `set(index)`.
- $O(n)$ for `add(index)` and `remove(index)` (except at the end)

### LinkedList

**Fast Inserts/Deletes**. Use only when frequently adding or removing to the _middle_ of a large list.

- $O(n)$ for `get(index)` and `set(index)`.
- $O(1)$ for `add(index)` and `remove(index)` (if you're _already_ at the element).

The standard way to implement a `Queue` in Java:

```java
Queue<Integer> q = new LinkedList<>();
```
