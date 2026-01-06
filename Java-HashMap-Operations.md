# Java HashMap/Map Operations Guide

A comprehensive guide to working with HashMap and Map interface in Java, covering creation, manipulation, iteration, and common operations.

## Table of Contents
1. [Map Basics](#map-basics)
2. [HashMap Creation](#hashmap-creation)
3. [Adding and Updating](#adding-and-updating)
4. [Retrieving Values](#retrieving-values)
5. [Removing Elements](#removing-elements)
6. [Checking Existence](#checking-existence)
7. [Size and Capacity](#size-and-capacity)
8. [Iteration](#iteration)
9. [Map Views](#map-views)
10. [Advanced Operations](#advanced-operations)
11. [Other Map Implementations](#other-map-implementations)

---

## Map Basics

### What is a HashMap?
```java
import java.util.HashMap;
import java.util.Map;

// HashMap stores key-value pairs
// - Keys must be unique
// - Values can be duplicated
// - Allows one null key and multiple null values
// - No guaranteed order
// - O(1) average time complexity for get/put

Map<String, Integer> map = new HashMap<>();
```

---

## HashMap Creation

### Basic Creation
```java
import java.util.HashMap;
import java.util.Map;

// Using diamond operator
Map<String, Integer> map1 = new HashMap<>();

// With initial capacity
Map<String, Integer> map2 = new HashMap<>(20);

// With initial capacity and load factor
Map<String, Integer> map3 = new HashMap<>(20, 0.75f);

// Concrete type reference
HashMap<String, Integer> hashMap = new HashMap<>();
```

### Initialize with Values
```java
// Java 9+ - Map.of() for immutable maps (up to 10 entries)
Map<String, Integer> map1 = Map.of(
"Apple", 1,
"Banana", 2,
"Orange", 3
);

// Java 9+ - Map.ofEntries() for more entries
Map<String, Integer> map2 = Map.ofEntries(
Map.entry("Apple", 1),
Map.entry("Banana", 2),
Map.entry("Orange", 3)
);

// Traditional way - mutable map
Map<String, Integer> map3 = new HashMap<>();
map3.put("Apple", 1);
map3.put("Banana", 2);
map3.put("Orange", 3);

// Double brace initialization (not recommended)
Map<String, Integer> map4 = new HashMap<>() {{
        put("Apple", 1);
        put("Banana", 2);
    }};
```

### Copy from Another Map
```java
Map<String, Integer> original = new HashMap<>();
original.put("A", 1);
original.put("B", 2);

// Copy constructor
Map<String, Integer> copy1 = new HashMap<>(original);

// Using putAll()
Map<String, Integer> copy2 = new HashMap<>();
copy2.putAll(original);
```

---

## Adding and Updating

### Put
```java
Map<String, Integer> map = new HashMap<>();

// Add new entry
map.put("Apple", 1); // Returns null

// Update existing entry
Integer oldValue = map.put("Apple", 2); // Returns 1
```

### PutIfAbsent (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);

// Only puts if key doesn't exist
map.putIfAbsent("Apple", 2); // No change, returns 1
map.putIfAbsent("Banana", 2); // Adds, returns null
```

### PutAll
```java
Map<String, Integer> map1 = new HashMap<>();
map1.put("Apple", 1);

Map<String, Integer> map2 = new HashMap<>();
map2.put("Banana", 2);
map2.put("Orange", 3);

// Add all entries from map2 to map1
map1.putAll(map2);
// map1 now has: {Apple=1, Banana=2, Orange=3}
```

---

## Retrieving Values

### Get
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

// Get value by key
Integer value = map.get("Apple"); // 1

// Get non-existent key
Integer notFound = map.get("Orange"); // null
```

### GetOrDefault (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);

// Get with default value if key doesn't exist
Integer value1 = map.getOrDefault("Apple", 0); // 1
Integer value2 = map.getOrDefault("Orange", 0); // 0
```

---

## Removing Elements

### Remove by Key
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

// Remove and return value
Integer removed = map.remove("Apple"); // Returns 1
// map now has: {Banana=2}

// Remove non-existent key
Integer notFound = map.remove("Orange"); // Returns null
```

### Remove by Key-Value Pair (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

// Remove only if key-value pair matches
boolean removed1 = map.remove("Apple", 1); // true, removes
boolean removed2 = map.remove("Banana", 5); // false, doesn't remove
```

### Clear All
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

map.clear(); // Removes all entries
System.out.println(map.size()); // 0
```

---

## Checking Existence

### Contains Key
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

boolean hasApple = map.containsKey("Apple"); // true
boolean hasOrange = map.containsKey("Orange"); // false
```

### Contains Value
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

boolean hasValue1 = map.containsValue(1); // true
boolean hasValue5 = map.containsValue(5); // false
```

---

## Size and Capacity

### Size
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

int size = map.size(); // 2

// Check if empty
boolean isEmpty = map.isEmpty(); // false

map.clear();
boolean nowEmpty = map.isEmpty(); // true
```

---

## Iteration

### Iterate Over Keys
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);
map.put("Orange", 3);

// For-each on keySet
for (String key : map.keySet()) {
    System.out.println(key + " = " + map.get(key));
}

// Using iterator
Iterator<String> iterator = map.keySet().iterator();
while (iterator.hasNext()) {
    String key = iterator.next();
    System.out.println(key);
}
```

### Iterate Over Values
```java
// For-each on values
for (Integer value : map.values()) {
    System.out.println(value);
}
```

### Iterate Over Entries
```java
// For-each on entrySet (most efficient)
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println(key + " = " + value);
}
```

### Using forEach (Java 8+)
```java
// Lambda expression
map.forEach((key, value) -> {
    System.out.println(key + " = " + value);
});

// Method reference
map.forEach((k, v) -> System.out.println(k + ": " + v));
```

### Using Streams (Java 8+)
```java
// Stream over entries
map.entrySet().stream()
.forEach(entry -> System.out.println(entry.getKey() + " = " + entry.getValue()));

// Filter and process
map.entrySet().stream()
.filter(entry -> entry.getValue() > 1)
.forEach(entry -> System.out.println(entry.getKey()));
```

---

## Map Views

### KeySet View
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);

Set<String> keys = map.keySet();
// keys = ["Apple", "Banana"]

// Changes to view affect the map
keys.remove("Apple");
// map now has: {Banana=2}
```

### Values Collection
```java
Collection<Integer> values = map.values();
// values = [1, 2]

// Can iterate, but not random access
for (Integer value : values) {
    System.out.println(value);
}
```

### EntrySet
```java
Set<Map.Entry<String, Integer>> entries = map.entrySet();

for (Map.Entry<String, Integer> entry : entries) {
    entry.setValue(entry.getValue() * 2); // Modifies map
}
```

---

## Advanced Operations

### Compute (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);

// Compute new value based on key and old value
map.compute("Apple", (key, oldValue) ->
oldValue == null ? 1 : oldValue + 1
); // Apple = 2

// Compute for new key
map.compute("Banana", (key, oldValue) ->
oldValue == null ? 1 : oldValue + 1
); // Banana = 1
```

### ComputeIfAbsent (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();

// Only compute if key is absent
map.computeIfAbsent("Apple", key -> key.length()); // Apple = 5
map.computeIfAbsent("Apple", key -> 100); // No change, still 5

// Useful for initializing nested structures
Map<String, List<Integer>> nestedMap = new HashMap<>();
nestedMap.computeIfAbsent("Numbers", k -> new ArrayList<>()).add(1);
nestedMap.computeIfAbsent("Numbers", k -> new ArrayList<>()).add(2);
// Numbers = [1, 2]
```

### ComputeIfPresent (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 5);

// Only compute if key exists
map.computeIfPresent("Apple", (key, value) -> value * 2); // Apple = 10
map.computeIfPresent("Banana", (key, value) -> value * 2); // No change
```

### Merge (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);

// Merge value if key exists, otherwise put value
map.merge("Apple", 1, (oldValue, newValue) -> oldValue + newValue);
// Apple = 2

map.merge("Banana", 1, (oldValue, newValue) -> oldValue + newValue);
// Banana = 1 (key didn't exist, just put the value)

// Common use case: counting
String[] words = {"apple", "banana", "apple", "orange", "banana", "apple"};
Map<String, Integer> wordCount = new HashMap<>();
for (String word : words) {
    wordCount.merge(word, 1, Integer::sum);
}
// {apple=3, banana=2, orange=1}
```

### Replace (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);

// Replace value for key
Integer oldValue = map.replace("Apple", 5); // Returns 1
Integer notReplaced = map.replace("Banana", 5); // Returns null

// Replace only if old value matches
boolean replaced1 = map.replace("Apple", 5, 10); // true
boolean replaced2 = map.replace("Apple", 1, 20); // false (old value is 10)
```

### ReplaceAll (Java 8+)
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);
map.put("Orange", 3);

// Replace all values
map.replaceAll((key, value) -> value * 2);
// {Apple=2, Banana=4, Orange=6}
```

---

## Other Map Implementations

### LinkedHashMap (Maintains Insertion Order)
```java
import java.util.LinkedHashMap;

Map<String, Integer> linkedMap = new LinkedHashMap<>();
linkedMap.put("C", 3);
linkedMap.put("A", 1);
linkedMap.put("B", 2);

// Iterates in insertion order: C, A, B
for (String key : linkedMap.keySet()) {
    System.out.println(key);
}
```

### TreeMap (Sorted by Keys)
```java
import java.util.TreeMap;

Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("C", 3);
treeMap.put("A", 1);
treeMap.put("B", 2);

// Iterates in natural order: A, B, C
for (String key : treeMap.keySet()) {
    System.out.println(key);
}

// With custom comparator
Map<String, Integer> reversedMap = new TreeMap<>(Comparator.reverseOrder());
```

### ConcurrentHashMap (Thread-Safe)
```java
import java.util.concurrent.ConcurrentHashMap;

Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("Apple", 1);

// Safe for concurrent access
// No need for external synchronization
```

### Hashtable (Legacy, Thread-Safe)
```java
import java.util.Hashtable;

// Legacy class, use ConcurrentHashMap instead
Hashtable<String, Integer> hashtable = new Hashtable<>();
hashtable.put("Apple", 1);
// Does NOT allow null keys or values
```

---

## Common Patterns

### Frequency Count
```java
String[] words = {"apple", "banana", "apple", "orange", "banana", "apple"};
Map<String, Integer> frequency = new HashMap<>();

for (String word : words) {
    frequency.put(word, frequency.getOrDefault(word, 0) + 1);
}
// {apple=3, banana=2, orange=1}

// Or using merge
for (String word : words) {
    frequency.merge(word, 1, Integer::sum);
}
```

### Group By
```java
List<String> words = Arrays.asList("apple", "banana", "avocado", "blueberry", "apricot");
Map<Character, List<String>> grouped = new HashMap<>();

for (String word : words) {
    char firstLetter = word.charAt(0);
    grouped.computeIfAbsent(firstLetter, k -> new ArrayList<>()).add(word);
}
// {a=[apple, avocado, apricot], b=[banana, blueberry]}
```

### Invert Map
```java
Map<String, Integer> original = new HashMap<>();
original.put("Apple", 1);
original.put("Banana", 2);

Map<Integer, String> inverted = new HashMap<>();
for (Map.Entry<String, Integer> entry : original.entrySet()) {
    inverted.put(entry.getValue(), entry.getKey());
}
// {1=Apple, 2=Banana}
```

### Sort Map by Value
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 3);
map.put("Banana", 1);
map.put("Orange", 2);

// Sort by value
List<Map.Entry<String, Integer>> sorted = new ArrayList<>(map.entrySet());
sorted.sort(Map.Entry.comparingByValue());

// Or create sorted LinkedHashMap
Map<String, Integer> sortedMap = map.entrySet().stream()
.sorted(Map.Entry.comparingByValue())
.collect(Collectors.toMap(
Map.Entry::getKey,
Map.Entry::getValue,
(e1, e2) -> e1,
LinkedHashMap::new
));
// {Banana=1, Orange=2, Apple=3}
```

---

## Best Practices

### 1. Use Interface Type
```java
// ✅ Good - use interface
Map<String, Integer> map = new HashMap<>();

// ❌ Less flexible
HashMap<String, Integer> map = new HashMap<>();
```

### 2. Specify Initial Capacity
```java
// If you know approximate size
Map<String, Integer> map = new HashMap<>(1000);
// Reduces rehashing
```

### 3. Check for Null
```java
String key = getUserInput();
Integer value = map.get(key);

// ✅ Safe
if (value != null) {
    System.out.println(value);
}

// Or use getOrDefault
Integer safeValue = map.getOrDefault(key, 0);
```

### 4. Use EntrySet for Iteration
```java
// ✅ Efficient - one lookup
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
}

// ❌ Inefficient - two lookups per iteration
for (String key : map.keySet()) {
    Integer value = map.get(key);
}
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `map.put(key, value)` | Add or update key-value pair | `V` - previous value if key existed, null otherwise | `map.put("A", 1);` |
| `map.putIfAbsent(key, value)` | Add only if key doesn't exist (Java 8+) | `V` - existing value if present, null if added | `map.putIfAbsent("A", 1);` |
| `map.putAll(otherMap)` | Add all entries from another map | `void` - modifies map in-place | `map.putAll(otherMap);` |
| `map.get(key)` | Get value by key | `V` - value if key exists, null otherwise | `Integer val = map.get("A");` |
| `map.getOrDefault(key, defaultValue)` | Get value or default if key absent (Java 8+) | `V` - value if present, defaultValue otherwise | `int val = map.getOrDefault("A", 0);` |
| `map.remove(key)` | Remove entry by key | `V` - removed value if existed, null otherwise | `Integer removed = map.remove("A");` |
| `map.remove(key, value)` | Remove only if key-value matches (Java 8+) | `boolean` - true if removed, false otherwise | `boolean done = map.remove("A", 1);` |
| `map.clear()` | Remove all entries | `void` - empties the map | `map.clear();` |
| `map.containsKey(key)` | Check if key exists | `boolean` - true if key present, false otherwise | `boolean has = map.containsKey("A");` |
| `map.containsValue(value)` | Check if value exists | `boolean` - true if value found, false otherwise | `boolean has = map.containsValue(1);` |
| `map.size()` | Get number of entries | `int` - count of key-value pairs | `int size = map.size();` |
| `map.isEmpty()` | Check if map is empty | `boolean` - true if no entries, false otherwise | `boolean empty = map.isEmpty();` |
| `map.keySet()` | Get set view of keys | `Set<K>` - set of all keys, changes reflect in map | `Set<String> keys = map.keySet();` |
| `map.values()` | Get collection view of values | `Collection<V>` - collection of all values | `Collection<Integer> vals = map.values();` |
| `map.entrySet()` | Get set view of entries | `Set<Map.Entry<K,V>>` - set of key-value pairs | `Set<Entry<K,V>> entries = map.entrySet();` |
| `map.forEach(action)` | Perform action for each entry (Java 8+) | `void` - iterates and applies action | `map.forEach((k,v) -> System.out.println(k));` |
| `map.compute(key, function)` | Compute new value for key (Java 8+) | `V` - computed value, null if function returns null | `map.compute("A", (k,v) -> v == null ? 1 : v+1);` |
| `map.computeIfAbsent(key, function)` | Compute if key absent (Java 8+) | `V` - existing or computed value | `map.computeIfAbsent("A", k -> new ArrayList<>());` |
| `map.computeIfPresent(key, function)` | Compute if key present (Java 8+) | `V` - computed value or null if removed | `map.computeIfPresent("A", (k,v) -> v*2);` |
| `map.merge(key, value, function)` | Merge value with existing (Java 8+) | `V` - merged value | `map.merge("A", 1, Integer::sum);` |
| `map.replace(key, value)` | Replace value for existing key (Java 8+) | `V` - previous value if replaced, null otherwise | `Integer old = map.replace("A", 5);` |
| `map.replace(key, oldValue, newValue)` | Replace only if old value matches (Java 8+) | `boolean` - true if replaced, false otherwise | `boolean done = map.replace("A", 1, 5);` |
| `map.replaceAll(function)` | Replace all values using function (Java 8+) | `void` - modifies all values in-place | `map.replaceAll((k,v) -> v*2);` |
| `entry.getKey()` | Get key from entry | `K` - the key | `String key = entry.getKey();` |
| `entry.getValue()` | Get value from entry | `V` - the value | `Integer val = entry.getValue();` |
| `entry.setValue(value)` | Set value for entry | `V` - previous value | `Integer old = entry.setValue(10);` |
| `Map.of(k1,v1,k2,v2,...)` | Create immutable map (Java 9+, max 10 pairs) | `Map<K,V>` - unmodifiable map | `Map<String,Integer> m = Map.of("A",1,"B",2);` |
| `Map.ofEntries(entries...)` | Create immutable map from entries (Java 9+) | `Map<K,V>` - unmodifiable map | `Map<K,V> m = Map.ofEntries(Map.entry("A",1));` |
| `Map.entry(key, value)` | Create map entry (Java 9+) | `Map.Entry<K,V>` - immutable entry | `Entry<String,Integer> e = Map.entry("A", 1);` |
| `Collections.synchronizedMap(map)` | Create thread-safe wrapper | `Map<K,V>` - synchronized map view | `Map<K,V> sync = Collections.synchronizedMap(new HashMap<>());` |
| `Collections.unmodifiableMap(map)` | Create unmodifiable wrapper | `Map<K,V>` - read-only map view | `Map<K,V> ro = Collections.unmodifiableMap(map);` |
