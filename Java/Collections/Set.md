# Set<E>

A collection that holds **unique** elements.

## Core Methods

### Basic Operations

- `add(E element)`: Adds an element. **Returns `true` if the set did _not_ already contain the element**.
- `remove(Object o)`: Removes the element if it exists.
- `contains(Object o)`: **Returns `true` if hte set contains the element**. (Main purpose of a set)
- `size()`: Returns the number of elements.

### Checking the State

- `isEmpty()`: Returns `true` if the set is empty.
- `clear()`: Empties the entire set.

### Iterating / Looping

- **For-Each Loop**:
  ```java
  for (String s : mySet) {
      System.out.println(s); // Note: The order is not guaranteed!
  }
  ```
- **`iterator()`**

### Checking for Duplicates

You can use the `boolean` return value of `.add()` as a 1-line check.

```java
Set<Character> row = new HashSet<>();
char c = 'a';

row.add(c);

if (!row.add(c)) {
    return false; // Found a duplicate
}
```

## Implementations

### HashSet

**Default / Fastest**. Best for when just checking for/preventing duplicates.

$O(1)$ average for `add`/`remove`/`contains`.

Order is chaotic and will change.

### TreeSet

**Sorted**. Use this _only_ when you need to store unique items _and_ iterate over them in sorted order.

$O(logN)$ for `add`/`remove`/`contains`.

## LinkedHashSet

**Orderly**. Use this when you need to store unique items _and_ iterate in the _same order_ you inserted them.
