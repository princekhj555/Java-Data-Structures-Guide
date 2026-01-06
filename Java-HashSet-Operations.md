# Java HashSet/Set Operations Guide

A comprehensive guide to working with HashSet and Set interface in Java, covering creation, manipulation, operations, and common use cases.

## Table of Contents
1. [Set Basics](#set-basics)
2. [HashSet Creation](#hashset-creation)
3. [Adding Elements](#adding-elements)
4. [Removing Elements](#removing-elements)
5. [Checking Elements](#checking-elements)
6. [Set Operations](#set-operations)
7. [Iteration](#iteration)
8. [Size and Capacity](#size-and-capacity)
9. [Other Set Implementations](#other-set-implementations)
10. [Common Patterns](#common-patterns)

---

## Set Basics

### What is a HashSet?
```java
import java.util.HashSet;
import java.util.Set;

// HashSet stores unique elements
// - No duplicates allowed
// - Allows one null element
// - No guaranteed order
// - O(1) average time complexity for add/remove/contains
// - Backed by HashMap internally

Set<String> set = new HashSet<>();
```

---

## HashSet Creation

### Basic Creation
```java
import java.util.HashSet;
import java.util.Set;

// Using diamond operator
Set<String> set1 = new HashSet<>();

// With initial capacity
Set<String> set2 = new HashSet<>(20);

// With initial capacity and load factor
Set<String> set3 = new HashSet<>(20, 0.75f);

// Concrete type reference
HashSet<String> hashSet = new HashSet<>();
```

### Initialize with Values
```java
// Java 9+ - Set.of() for immutable sets
Set<String> set1 = Set.of("Apple", "Banana", "Orange");

// From array
String[] array = {"Apple", "Banana", "Orange"};
Set<String> set2 = new HashSet<>(Arrays.asList(array));

// Using Stream
Set<String> set3 = Stream.of("Apple", "Banana", "Orange")
.collect(Collectors.toSet());

// Traditional way
Set<String> set4 = new HashSet<>();
set4.add("Apple");
set4.add("Banana");
set4.add("Orange");
```

### Copy from Collection
```java
List<String> list = Arrays.asList("Apple", "Banana", "Apple");

// Copy constructor (removes duplicates)
Set<String> set = new HashSet<>(list);
// set = ["Apple", "Banana"]
```

---

## Adding Elements

### Add Single Element
```java
Set<String> set = new HashSet<>();

// Add element (returns true if added, false if already present)
boolean added1 = set.add("Apple"); // true
boolean added2 = set.add("Banana"); // true
boolean added3 = set.add("Apple"); // false (duplicate)

System.out.println(set); // [Apple, Banana]
```

### Add Multiple Elements
```java
Set<String> set = new HashSet<>();

// AddAll from collection
List<String> fruits = Arrays.asList("Apple", "Banana", "Orange");
set.addAll(fruits);

// AddAll from another set
Set<String> morefruits = Set.of("Mango", "Grape");
set.addAll(moreFruits);
```

---

## Removing Elements

### Remove Single Element
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");

// Remove element (returns true if removed, false if not present)
boolean removed1 = set.remove("Apple"); // true
boolean removed2 = set.remove("Orange"); // false

System.out.println(set); // [Banana]
```

### Remove Multiple Elements
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange", "Mango"));

// RemoveAll (removes all specified elements)
Set<String> toRemove = Set.of("Apple", "Orange");
set.removeAll(toRemove);
// set = [Banana, Mango]
```

### Retain Only Specific Elements
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange", "Mango"));

// RetainAll (keeps only specified elements)
Set<String> toKeep = Set.of("Apple", "Banana");
set.retainAll(toKeep);
// set = [Apple, Banana]
```

### Remove If (Java 8+)
```java
Set<Integer> numbers = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5, 6));

// Remove elements matching condition
numbers.removeIf(n -> n % 2 == 0); // Remove even numbers
// numbers = [1, 3, 5]
```

### Clear All
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");

set.clear(); // Removes all elements
System.out.println(set.size()); // 0
```

---

## Checking Elements

### Contains
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");

boolean hasApple = set.contains("Apple"); // true
boolean hasOrange = set.contains("Orange"); // false
```

### Contains All
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange"));

Set<String> subset1 = Set.of("Apple", "Banana");
boolean hasAll1 = set.containsAll(subset1); // true

Set<String> subset2 = Set.of("Apple", "Mango");
boolean hasAll2 = set.containsAll(subset2); // false
```

---

## Set Operations

### Union (Combine Sets)
```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5));

// Union - all elements from both sets
Set<Integer> union = new HashSet<>(set1);
union.addAll(set2);
// union = [1, 2, 3, 4, 5]

// Using streams
Set<Integer> unionStream = Stream.concat(set1.stream(), set2.stream())
.collect(Collectors.toSet());
```

### Intersection (Common Elements)
```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6));

// Intersection - only common elements
Set<Integer> intersection = new HashSet<>(set1);
intersection.retainAll(set2);
// intersection = [3, 4]

// Using streams
Set<Integer> intersectionStream = set1.stream()
.filter(set2::contains)
.collect(Collectors.toSet());
```

### Difference (Elements in First but not Second)
```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6));

// Difference - elements in set1 but not in set2
Set<Integer> difference = new HashSet<>(set1);
difference.removeAll(set2);
// difference = [1, 2]

// Using streams
Set<Integer> differenceStream = set1.stream()
.filter(e -> !set2.contains(e))
.collect(Collectors.toSet());
```

### Symmetric Difference (Elements in Either but not Both)
```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6));

// Symmetric difference - elements in either set but not in both
Set<Integer> symmetricDiff = new HashSet<>(set1);
symmetricDiff.addAll(set2);

Set<Integer> intersection = new HashSet<>(set1);
intersection.retainAll(set2);

symmetricDiff.removeAll(intersection);
// symmetricDiff = [1, 2, 5, 6]
```

### Subset Check
```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
Set<Integer> set2 = new HashSet<>(Arrays.asList(2, 3));

// Check if set2 is subset of set1
boolean isSubset = set1.containsAll(set2); // true
```

---

## Iteration

### For-Each Loop
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Orange");

// Enhanced for loop
for (String fruit : set) {
    System.out.println(fruit);
}
```

### Iterator
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange"));

// Using iterator
Iterator<String> iterator = set.iterator();
while (iterator.hasNext()) {
    String fruit = iterator.next();
    System.out.println(fruit);
    // Can remove during iteration
    if (fruit.equals("Banana")) {
        iterator.remove();
    }
}
```

### forEach (Java 8+)
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange"));

// Lambda expression
set.forEach(fruit -> System.out.println(fruit));

// Method reference
set.forEach(System.out::println);
```

### Stream (Java 8+)
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange"));

// Stream operations
set.stream()
.filter(s -> s.startsWith("A"))
.map(String::toUpperCase)
.forEach(System.out::println);

// Collect to list
List<String> list = set.stream()
.sorted()
.collect(Collectors.toList());
```

---

## Size and Capacity

### Size
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");

int size = set.size(); // 2

// Check if empty
boolean isEmpty = set.isEmpty(); // false

set.clear();
boolean nowEmpty = set.isEmpty(); // true
```

---

## Other Set Implementations

### LinkedHashSet (Maintains Insertion Order)
```java
import java.util.LinkedHashSet;

Set<String> linkedSet = new LinkedHashSet<>();
linkedSet.add("C");
linkedSet.add("A");
linkedSet.add("B");

// Iterates in insertion order: C, A, B
for (String item : linkedSet) {
    System.out.println(item);
}
```

### TreeSet (Sorted Order)
```java
import java.util.TreeSet;

Set<String> treeSet = new TreeSet<>();
treeSet.add("C");
treeSet.add("A");
treeSet.add("B");

// Iterates in natural order: A, B, C
for (String item : treeSet) {
    System.out.println(item);
}

// With custom comparator
Set<String> reversedSet = new TreeSet<>(Comparator.reverseOrder());
```

### EnumSet (For Enum Types)
```java
import java.util.EnumSet;

enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }

// All days
Set<Day> allDays = EnumSet.allOf(Day.class);

// Weekdays
Set<Day> weekdays = EnumSet.range(Day.MONDAY, Day.FRIDAY);

// Specific days
Set<Day> weekend = EnumSet.of(Day.SATURDAY, Day.SUNDAY);
```

### CopyOnWriteArraySet (Thread-Safe)
```java
import java.util.concurrent.CopyOnWriteArraySet;

Set<String> concurrentSet = new CopyOnWriteArraySet<>();
concurrentSet.add("Apple");
// Thread-safe but slower for writes
// Good for read-heavy scenarios
```

---

## Common Patterns

### Remove Duplicates from List
```java
List<Integer> listWithDuplicates = Arrays.asList(1, 2, 2, 3, 4, 4, 5);

// Convert to set to remove duplicates
Set<Integer> uniqueSet = new HashSet<>(listWithDuplicates);

// Convert back to list if needed
List<Integer> uniqueList = new ArrayList<>(uniqueSet);
// uniqueList = [1, 2, 3, 4, 5] (order may vary)

// To preserve order, use LinkedHashSet
Set<Integer> orderedUnique = new LinkedHashSet<>(listWithDuplicates);
```

### Find Duplicates
```java
List<String> list = Arrays.asList("apple", "banana", "apple", "orange", "banana");

Set<String> seen = new HashSet<>();
Set<String> duplicates = new HashSet<>();

for (String item : list) {
    if (!seen.add(item)) {
        duplicates.add(item); // Already seen, so it's a duplicate
    }
}
// duplicates = [apple, banana]
```

### Convert Array to Set
```java
String[] array = {"Apple", "Banana", "Apple", "Orange"};

// Convert to set (removes duplicates)
Set<String> set = new HashSet<>(Arrays.asList(array));
// set = [Apple, Banana, Orange]

// Using streams
Set<String> setStream = Arrays.stream(array)
.collect(Collectors.toSet());
```

### Convert Set to Array
```java
Set<String> set = new HashSet<>(Arrays.asList("Apple", "Banana", "Orange"));

// To array
String[] array = set.toArray(new String[0]);

// Or specify size
String[] array2 = set.toArray(new String[set.size()]);
```

### Filter Set
```java
Set<Integer> numbers = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// Filter even numbers
Set<Integer> evens = numbers.stream()
.filter(n -> n % 2 == 0)
.collect(Collectors.toSet());
// evens = [2, 4, 6, 8, 10]
```

---

## Best Practices

### 1. Use Interface Type
```java
// ✅ Good - use interface
Set<String> set = new HashSet<>();

// ❌ Less flexible
HashSet<String> set = new HashSet<>();
```

### 2. Specify Initial Capacity
```java
// If you know approximate size
Set<String> set = new HashSet<>(1000);
// Reduces rehashing
```

### 3. Choose Right Implementation
```java
// Need unique elements only
Set<String> hashSet = new HashSet<>();

// Need insertion order
Set<String> linkedHashSet = new LinkedHashSet<>();

// Need sorted order
Set<String> treeSet = new TreeSet<>();

// For enums
Set<Day> enumSet = EnumSet.allOf(Day.class);
```

### 4. Avoid Modifying Elements After Adding
```java
// ❌ Bad - modifying object after adding
class Person {
    String name;
    // equals() and hashCode() based on name
}

Person p = new Person("John");
set.add(p);
p.name = "Jane"; // ❌ Don't do this!
// Now set.contains(p) might return false
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `set.add(element)` | Add element to set | `boolean` - true if added (wasn't present), false if duplicate | `boolean added = set.add("Apple");` |
| `set.addAll(collection)` | Add all elements from collection | `boolean` - true if set changed, false otherwise | `set.addAll(Arrays.asList("A", "B"));` |
| `set.remove(element)` | Remove element from set | `boolean` - true if removed (was present), false otherwise | `boolean removed = set.remove("Apple");` |
| `set.removeAll(collection)` | Remove all elements in collection | `boolean` - true if set changed, false otherwise | `set.removeAll(Arrays.asList("A", "B"));` |
| `set.retainAll(collection)` | Keep only elements in collection | `boolean` - true if set changed, false otherwise | `set.retainAll(Arrays.asList("A", "B"));` |
| `set.removeIf(predicate)` | Remove elements matching predicate (Java 8+) | `boolean` - true if any removed, false otherwise | `set.removeIf(s -> s.startsWith("A"));` |
| `set.clear()` | Remove all elements | `void` - empties the set | `set.clear();` |
| `set.contains(element)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = set.contains("Apple");` |
| `set.containsAll(collection)` | Check if all elements exist | `boolean` - true if all present, false otherwise | `boolean hasAll = set.containsAll(list);` |
| `set.size()` | Get number of elements | `int` - count of elements | `int size = set.size();` |
| `set.isEmpty()` | Check if set is empty | `boolean` - true if no elements, false otherwise | `boolean empty = set.isEmpty();` |
| `set.iterator()` | Get iterator for elements | `Iterator<E>` - iterator to traverse set | `Iterator<String> it = set.iterator();` |
| `set.forEach(action)` | Perform action for each element (Java 8+) | `void` - iterates and applies action | `set.forEach(System.out::println);` |
| `set.stream()` | Create stream from set (Java 8+) | `Stream<E>` - stream for processing | `set.stream().filter(s -> s.length() > 3);` |
| `set.parallelStream()` | Create parallel stream (Java 8+) | `Stream<E>` - parallel stream | `set.parallelStream().map(String::toUpperCase);` |
| `set.toArray()` | Convert to Object array | `Object[]` - array of elements | `Object[] arr = set.toArray();` |
| `set.toArray(array)` | Convert to typed array | `T[]` - typed array | `String[] arr = set.toArray(new String[0]);` |
| `Set.of(elements...)` | Create immutable set (Java 9+) | `Set<E>` - unmodifiable set | `Set<String> set = Set.of("A", "B", "C");` |
| `Set.copyOf(collection)` | Create immutable copy (Java 10+) | `Set<E>` - unmodifiable copy | `Set<String> copy = Set.copyOf(original);` |
| `Collections.unmodifiableSet(set)` | Create unmodifiable wrapper | `Set<E>` - read-only view | `Set<String> ro = Collections.unmodifiableSet(set);` |
| `Collections.synchronizedSet(set)` | Create thread-safe wrapper | `Set<E>` - synchronized set | `Set<String> sync = Collections.synchronizedSet(set);` |
| `Collections.singleton(element)` | Create single-element immutable set | `Set<E>` - immutable set with one element | `Set<String> single = Collections.singleton("A");` |
| `Collections.emptySet()` | Create empty immutable set | `Set<E>` - immutable empty set | `Set<String> empty = Collections.emptySet();` |