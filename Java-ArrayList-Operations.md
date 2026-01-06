# Java List/ArrayList Operations Guide

A comprehensive guide to working with List and ArrayList in Java, covering creation, manipulation, searching, and common operations.

## Table of Contents
1. [List Basics](#list-basics)
2. [ArrayList Creation](#arraylist-creation)
3. [Adding Elements](#adding-elements)
4. [Accessing Elements](#accessing-elements)
5. [Modifying Elements](#modifying-elements)
6. [Removing Elements](#removing-elements)
7. [Searching and Checking](#searching-and-checking)
8. [Sorting](#sorting)
9. [Iteration](#iteration)
10. [Sublist and Range Operations](#sublist-and-range-operations)
11. [List Conversion](#list-conversion)
12. [Other List Implementations](#other-list-implementations)

---

## List Basics

### What is an ArrayList?
```java
import java.util.ArrayList;
import java.util.List;

// ArrayList is a resizable array implementation
// - Maintains insertion order
// - Allows duplicates
// - Allows null elements
// - Random access: O(1) for get/set
// - Add/Remove: O(1) at end, O(n) in middle
// - Backed by dynamic array

List<String> list = new ArrayList<>();
```

---

## ArrayList Creation

### Basic Creation
```java
import java.util.ArrayList;
import java.util.List;

// Using diamond operator
List<String> list1 = new ArrayList<>();

// With initial capacity
List<String> list2 = new ArrayList<>(20);

// Concrete type reference
ArrayList<String> arrayList = new ArrayList<>();
```

### Initialize with Values
```java
// Java 9+ - List.of() for immutable lists
List<String> list1 = List.of("Apple", "Banana", "Orange");

// Arrays.asList() - fixed-size list
List<String> list2 = Arrays.asList("Apple", "Banana", "Orange");
// Cannot add or remove, but can modify elements

// From array
String[] array = {"Apple", "Banana", "Orange"};
List<String> list3 = new ArrayList<>(Arrays.asList(array));

// Traditional way
List<String> list4 = new ArrayList<>();
list4.add("Apple");
list4.add("Banana");
list4.add("Orange");
```

### Copy from Collection
```java
List<String> original = new ArrayList<>();
original.add("Apple");
original.add("Banana");

// Copy constructor
List<String> copy1 = new ArrayList<>(original);

// Using addAll
List<String> copy2 = new ArrayList<>();
copy2.addAll(original);
```

---

## Adding Elements

### Add at End
```java
List<String> list = new ArrayList<>();

// Add to end (always returns true)
list.add("Apple");
list.add("Banana");
list.add("Orange");
// list = [Apple, Banana, Orange]
```

### Add at Index
```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Orange");

// Insert at index (shifts elements)
list.add(1, "Banana");
// list = [Apple, Banana, Orange]
```

### Add Multiple Elements
```java
List<String> list = new ArrayList<>();
list.add("Apple");

// AddAll at end
List<String> moreFruits = Arrays.asList("Banana", "Orange");
list.addAll(moreFruits);
// list = [Apple, Banana, Orange]

// AddAll at index
list.addAll(1, Arrays.asList("Mango", "Grape"));
// list = [Apple, Mango, Grape, Banana, Orange]
```

---

## Accessing Elements

### Get by Index
```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
list.add("Orange");

// Get element at index
String first = list.get(0); // "Apple"
String last = list.get(list.size() - 1); // "Orange"

// Get with bounds checking
try {
    String element = list.get(10); // Throws IndexOutOfBoundsException
} catch (IndexOutOfBoundsException e) {
System.out.println("Index out of bounds");
}
```

### Get First and Last
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// First element
String first = list.get(0);

// Last element
String last = list.get(list.size() - 1);

// Java 21+ - getFirst(), getLast()
String firstNew = list.getFirst();
String lastNew = list.getLast();
```

---

## Modifying Elements

### Set by Index
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Replace element at index (returns old value)
String old = list.set(1, "Mango");
// list = [Apple, Mango, Orange]
// old = "Banana"
```

### Replace All (Java 8+)
```java
List<String> list = new ArrayList<>(Arrays.asList("apple", "banana", "orange"));

// Transform all elements
list.replaceAll(String::toUpperCase);
// list = [APPLE, BANANA, ORANGE]

// Using lambda
list.replaceAll(s -> s + "!");
// list = [APPLE!, BANANA!, ORANGE!]
```

---

## Removing Elements

### Remove by Index
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Remove by index (returns removed element)
String removed = list.remove(1);
// list = [Apple, Orange]
// removed = "Banana"
```

### Remove by Object
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Remove by object (returns true if removed)
boolean removed1 = list.remove("Banana"); // true
boolean removed2 = list.remove("Mango"); // false
// list = [Apple, Orange]

// Note: For Integer lists, use remove(Integer.valueOf(value))
List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
nums.remove(Integer.valueOf(3)); // Removes value 3
nums.remove(2); // Removes element at index 2
```

### Remove Multiple Elements
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange", "Mango"));

// RemoveAll (removes all specified elements)
List<String> toRemove = Arrays.asList("Apple", "Orange");
list.removeAll(toRemove);
// list = [Banana, Mango]
```

### Remove If (Java 8+)
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));

// Remove elements matching condition
numbers.removeIf(n -> n % 2 == 0); // Remove even numbers
// numbers = [1, 3, 5]

List<String> words = new ArrayList<>(Arrays.asList("apple", "application", "banana"));
words.removeIf(s -> s.startsWith("app"));
// words = [banana]
```

### Retain Only
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange", "Mango"));

// RetainAll (keeps only specified elements)
List<String> toKeep = Arrays.asList("Apple", "Banana");
list.retainAll(toKeep);
// list = [Apple, Banana]
```

### Clear All
```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");

list.clear(); // Removes all elements
System.out.println(list.size()); // 0
```

---

## Searching and Checking

### Contains
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

boolean hasApple = list.contains("Apple"); // true
boolean hasMango = list.contains("Mango"); // false
```

### Index Of
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Apple", "Orange"));

// First occurrence
int firstIndex = list.indexOf("Apple"); // 0

// Last occurrence
int lastIndex = list.lastIndexOf("Apple"); // 2

// Not found
int notFound = list.indexOf("Mango"); // -1
```

### Contains All
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

List<String> subset1 = Arrays.asList("Apple", "Banana");
boolean hasAll1 = list.containsAll(subset1); // true

List<String> subset2 = Arrays.asList("Apple", "Mango");
boolean hasAll2 = list.containsAll(subset2); // false
```

---

## Sorting

### Natural Order
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(5, 2, 8, 1, 9));

// Sort in natural order
Collections.sort(numbers);
// numbers = [1, 2, 5, 8, 9]

// Or using list.sort() (Java 8+)
numbers.sort(null); // null means natural order
// numbers = [1, 2, 5, 8, 9]
```

### Reverse Order
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(5, 2, 8, 1, 9));

// Sort in reverse
Collections.sort(numbers, Collections.reverseOrder());
// numbers = [9, 8, 5, 2, 1]

// Or using list.sort()
numbers.sort(Comparator.reverseOrder());
```

### Custom Comparator
```java
List<String> words = new ArrayList<>(Arrays.asList("apple", "pie", "banana"));

// Sort by length
words.sort(Comparator.comparingInt(String::length));
// words = [pie, apple, banana]

// Sort by length then alphabetically
words.sort(Comparator.comparingInt(String::length)
.thenComparing(String::compareTo));
```

### Reverse List
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

Collections.reverse(numbers);
// numbers = [5, 4, 3, 2, 1]
```

### Shuffle
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

Collections.shuffle(numbers);
// numbers = [3, 1, 5, 2, 4] (random order)
```

---

## Iteration

### For Loop
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Traditional for loop
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

### Enhanced For Loop
```java
// For-each loop
for (String fruit : list) {
    System.out.println(fruit);
}
```

### Iterator
```java
Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String fruit = iterator.next();
    System.out.println(fruit);
    // Can remove during iteration
    if (fruit.equals("Banana")) {
        iterator.remove();
    }
}
```

### ListIterator (Bidirectional)
```java
ListIterator<String> listIterator = list.listIterator();

// Forward iteration
while (listIterator.hasNext()) {
    String fruit = listIterator.next();
    System.out.println(fruit);
}

// Backward iteration
while (listIterator.hasPrevious()) {
    String fruit = listIterator.previous();
    System.out.println(fruit);
}

// Modify during iteration
listIterator = list.listIterator();
while (listIterator.hasNext()) {
    String fruit = listIterator.next();
    listIterator.set(fruit.toUpperCase());
}
```

### forEach (Java 8+)
```java
list.forEach(fruit -> System.out.println(fruit));

// Method reference
list.forEach(System.out::println);
```

### Stream (Java 8+)
```java
list.stream()
.filter(s -> s.startsWith("A"))
.map(String::toUpperCase)
.forEach(System.out::println);
```

---

## Sublist and Range Operations

### Sublist
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// Get sublist (fromIndex inclusive, toIndex exclusive)
List<Integer> sublist = numbers.subList(2, 5);
// sublist = [3, 4, 5]

// Changes to sublist reflect in original
sublist.set(0, 30);
// numbers = [1, 2, 30, 4, 5, 6, 7, 8, 9, 10]

// Clear sublist
sublist.clear();
// numbers = [1, 2, 6, 7, 8, 9, 10]
```

---

## List Conversion

### To Array
```java
List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// To Object array
Object[] objArray = list.toArray();

// To typed array
String[] strArray = list.toArray(new String[0]);

// Or specify size
String[] strArray2 = list.toArray(new String[list.size()]);
```

### To Set
```java
List<Integer> listWithDuplicates = Arrays.asList(1, 2, 2, 3, 4, 4, 5);

// Convert to set (removes duplicates)
Set<Integer> set = new HashSet<>(listWithDuplicates);
```

### Join to String
```java
List<String> words = Arrays.asList("Hello", "World", "Java");

// Join with delimiter
String joined = String.join(" ", words);
// "Hello World Java"

// Using streams
String streamJoined = words.stream()
.collect(Collectors.joining(", "));
// "Hello, World, Java"
```

---

## Other List Implementations

### LinkedList
```java
import java.util.LinkedList;

// Better for frequent insertions/deletions
List<String> linkedList = new LinkedList<>();
linkedList.add("Apple");
linkedList.addFirst("Mango"); // O(1)
linkedList.addLast("Orange"); // O(1)
```

### Vector (Legacy, Thread-Safe)
```java
import java.util.Vector;

// Legacy class, use Collections.synchronizedList() instead
List<String> vector = new Vector<>();
```

### CopyOnWriteArrayList (Thread-Safe)
```java
import java.util.concurrent.CopyOnWriteArrayList;

// Thread-safe, good for read-heavy scenarios
List<String> concurrentList = new CopyOnWriteArrayList<>();
```

---

## Common Patterns

### Filter List
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Filter even numbers
List<Integer> evens = numbers.stream()
.filter(n -> n % 2 == 0)
.collect(Collectors.toList());
// evens = [2, 4, 6, 8, 10]
```

### Transform List
```java
List<String> words = Arrays.asList("apple", "banana", "orange");

// Transform to uppercase
List<String> upper = words.stream()
.map(String::toUpperCase)
.collect(Collectors.toList());
// upper = [APPLE, BANANA, ORANGE]
```

### Find Max/Min
```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9, 3);

// Find max
int max = Collections.max(numbers); // 9

// Find min
int min = Collections.min(numbers); // 1

// Using streams
int streamMax = numbers.stream()
.max(Integer::compareTo)
.orElse(0);
```

### Frequency Count
```java
List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");

int frequency = Collections.frequency(words, "apple"); // 3
```

---

## Best Practices

### 1. Use Interface Type
```java
// ✅ Good
List<String> list = new ArrayList<>();

// ❌ Less flexible
ArrayList<String> list = new ArrayList<>();
```

### 2. Specify Initial Capacity
```java
// If you know approximate size
List<String> list = new ArrayList<>(1000);
```

### 3. Use Arrays.asList() Carefully
```java
// Fixed-size, cannot add/remove
List<String> fixedList = Arrays.asList("A", "B", "C");

// Modifiable copy
List<String> modifiableList = new ArrayList<>(Arrays.asList("A", "B", "C"));
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `list.add(element)` | Add element to end of list | `boolean` - always true | `list.add("Apple");` |
| `list.add(index, element)` | Insert element at specific index | `void` - shifts elements right | `list.add(0, "First");` |
| `list.addAll(collection)` | Add all elements from collection | `boolean` - true if list changed | `list.addAll(Arrays.asList("A", "B"));` |
| `list.addAll(index, collection)` | Insert collection at index | `boolean` - true if list changed | `list.addAll(0, otherList);` |
| `list.get(index)` | Get element at index | `E` - element at index | `String s = list.get(0);` |
| `list.getFirst()` | Get first element (Java 21+) | `E` - first element | `String first = list.getFirst();` |
| `list.getLast()` | Get last element (Java 21+) | `E` - last element | `String last = list.getLast();` |
| `list.set(index, element)` | Replace element at index | `E` - previous element at index | `String old = list.set(0, "New");` |
| `list.remove(index)` | Remove element at index | `E` - removed element | `String removed = list.remove(0);` |
| `list.remove(object)` | Remove first occurrence of object | `boolean` - true if removed, false if not found | `boolean done = list.remove("Apple");` |
| `list.removeAll(collection)` | Remove all elements in collection | `boolean` - true if list changed | `list.removeAll(Arrays.asList("A", "B"));` |
| `list.retainAll(collection)` | Keep only elements in collection | `boolean` - true if list changed | `list.retainAll(toKeep);` |
| `list.removeIf(predicate)` | Remove matching elements (Java 8+) | `boolean` - true if any removed | `list.removeIf(s -> s.startsWith("A"));` |
| `list.clear()` | Remove all elements | `void` - empties the list | `list.clear();` |
| `list.contains(object)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = list.contains("Apple");` |
| `list.containsAll(collection)` | Check if all elements exist | `boolean` - true if all present, false otherwise | `boolean hasAll = list.containsAll(subset);` |
| `list.indexOf(object)` | Find first occurrence index | `int` - index if found, -1 if not found | `int idx = list.indexOf("Apple");` |
| `list.lastIndexOf(object)` | Find last occurrence index | `int` - last index if found, -1 if not found | `int idx = list.lastIndexOf("Apple");` |
| `list.size()` | Get number of elements | `int` - count of elements | `int size = list.size();` |
| `list.isEmpty()` | Check if list is empty | `boolean` - true if no elements, false otherwise | `boolean empty = list.isEmpty();` |
| `list.sort(comparator)` | Sort list (Java 8+) | `void` - sorts in-place | `list.sort(Comparator.naturalOrder());` |
| `list.replaceAll(function)` | Transform all elements (Java 8+) | `void` - modifies in-place | `list.replaceAll(String::toUpperCase);` |
| `list.subList(from, to)` | Get view of portion (from inclusive, to exclusive) | `List<E>` - sublist view, changes reflect in original | `List<E> sub = list.subList(0, 3);` |
| `list.toArray()` | Convert to Object array | `Object[]` - array of elements | `Object[] arr = list.toArray();` |
| `list.toArray(array)` | Convert to typed array | `T[]` - typed array | `String[] arr = list.toArray(new String[0]);` |
| `list.iterator()` | Get iterator | `Iterator<E>` - iterator to traverse | `Iterator<String> it = list.iterator();` |
| `list.listIterator()` | Get bidirectional iterator | `ListIterator<E>` - can traverse both directions | `ListIterator<String> it = list.listIterator();` |
| `list.listIterator(index)` | Get iterator starting at index | `ListIterator<E>` - iterator from position | `ListIterator<E> it = list.listIterator(5);` |
| `list.forEach(action)` | Perform action for each (Java 8+) | `void` - iterates and applies action | `list.forEach(System.out::println);` |
| `list.stream()` | Create stream (Java 8+) | `Stream<E>` - stream for processing | `list.stream().filter(s -> s.length() > 3);` |
| `Collections.sort(list)` | Sort list in natural order | `void` - sorts in-place | `Collections.sort(numbers);` |
| `Collections.sort(list, comparator)` | Sort with comparator | `void` - sorts in-place | `Collections.sort(list, Collections.reverseOrder());` |
| `Collections.reverse(list)` | Reverse order of elements | `void` - reverses in-place | `Collections.reverse(list);` |
| `Collections.shuffle(list)` | Randomly shuffle elements | `void` - randomizes order | `Collections.shuffle(list);` |
| `Collections.frequency(list, object)` | Count occurrences | `int` - frequency of element | `int count = Collections.frequency(list, "A");` |
| `Collections.max(list)` | Find maximum element | `E` - maximum by natural order | `int max = Collections.max(numbers);` |
| `Collections.min(list)` | Find minimum element | `E` - minimum by natural order | `int min = Collections.min(numbers);` |
| `List.of(elements...)` | Create immutable list (Java 9+) | `List<E>` - unmodifiable list | `List<String> list = List.of("A", "B");` |
| `List.copyOf(collection)` | Create immutable copy (Java 10+) | `List<E>` - unmodifiable copy | `List<String> copy = List.copyOf(original);` |
| `Arrays.asList(elements...)` | Create fixed-size list | `List<E>` - backed by array, can modify but not resize | `List<String> list = Arrays.asList("A", "B");` |
| `Collections.unmodifiableList(list)` | Create unmodifiable wrapper | `List<E>` - read-only view | `List<String> ro = Collections.unmodifiableList(list);` |
| `Collections.synchronizedList(list)` | Create thread-safe wrapper | `List<E>` - synchronized list | `List<String> sync = Collections.synchronizedList(list);` |
