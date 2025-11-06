# Map<K, V>

Maps unique **keys** to **values**.

## Core Methods

### Basic Operations

- `put(K key, V value)`: Add a key-value pair.
- `get(Object key)`: Gets the value for a key. Returns `null` if not found.
- `getOrDefault(Object key, V defaultValue)`: Safer. Returns `defaultValue` if not found.

### Checking the State

- `containsKey(Object key)`: Returns `true` if the key is in the map.
- `containsValue(Object value)`: Returns `true` if the value is in the map.
- `size()`: Returns the number of key-value pairs.
- `isEmpty()`: Returns `true` if the map is empty.

### Modern "Helper" Methods

- `computeIfAbsent(K key, Function mappingFunction)`: Gets the value, or if it's missing, runs the function, puts the result, and returns it.
  - Great for setting up a "default dictionary".
- `merge(K key, V value, BiFunction remappingFunction)`: Puts the value if absent, or runs the function to combine the `oldVal` and `newVal` if present.
  - Great for frequency counting.
- `putIfAbsent(K key, V value)`: A simple version of `computeIfAbsent` that only works with a pre-made value.
  - _WARNING_: Can be very expensive when you have a low miss rate since it has to make the value regardless of whether or not its used, and discards it immediately if not needed.

### Iterating / Looping

- `keySet()`: Returns a `Set` of all keys.
- `values()`: Returns a `Collection` of all values.
- `entrySet()`: Returns a `Set<Map.Entry<K, V>>`.
  - Most efficient way to loop if you need the key and the value.

## Common Implementations

### HashMap

**Default / Fastest**. Use this 90% of the time when you just need a key-value store and don't care about order.

$O(1)$ Speed for `put`/`get`.

**Not Ordered** (Cannot be used as a key). Order is chaotic and will change.

### TreeMap

**Sorted**. Use this _only_ when you need the keys to be in sorted (alphabetical/numerical) order.

$O(logN)$ Speed for `put`/`get`

### LinkedHashMap

**Orderly**. Use this when you need to iterate in the _same order_ you inserted the items.

$O(1)$ Speed for `put`/`get`

Great for caches.
