# Java TreeSet Operations Guide

A comprehensive guide to working with TreeSet in Java, covering sorted set operations, navigation, and common use cases.

## Table of Contents
1. [TreeSet Basics](#treeset-basics)
2. [TreeSet Creation](#treeset-creation)
3. [Adding Elements](#adding-elements)
4. [Removing Elements](#removing-elements)
5. [Navigation Methods](#navigation-methods)
6. [Range Operations](#range-operations)
7. [Checking Elements](#checking-elements)
8. [Iteration](#iteration)
9. [Comparator Operations](#comparator-operations)
10. [Common Patterns](#common-patterns)

---

## TreeSet Basics

### What is a TreeSet?
```java
import java.util.TreeSet;
import java.util.Set;

// TreeSet is a sorted set implementation
// - Elements stored in sorted order (natural or custom)
// - No duplicates allowed
// - Does NOT allow null elements
// - O(log n) time complexity for add/remove/contains
// - Backed by Red-Black tree (self-balancing BST)
// - Implements NavigableSet interface

Set<Integer> set = new TreeSet<>();
```

---

## TreeSet Creation

### Basic Creation
```java
import java.util.TreeSet;

// Natural order (ascending)
TreeSet<Integer> set1 = new TreeSet<>();

// With custom comparator (descending)
TreeSet<Integer> set2 = new TreeSet<>(Comparator.reverseOrder());

// From collection
List<Integer> list = Arrays.asList(5, 2, 8, 1, 9);
TreeSet<Integer> set3 = new TreeSet<>(list);
// set3 = [1, 2, 5, 8, 9] (automatically sorted)
```

### Initialize with Values
```java
// From existing collection
TreeSet<String> set1 = new TreeSet<>(Arrays.asList("C", "A", "B"));
// set1 = [A, B, C] (sorted)

// Using Stream
TreeSet<Integer> set2 = Stream.of(5, 2, 8, 1, 9)
.collect(Collectors.toCollection(TreeSet::new));
// set2 = [1, 2, 5, 8, 9]
```

### Custom Comparator
```java
// Sort strings by length, then alphabetically
TreeSet<String> set = new TreeSet<>(
Comparator.comparingInt(String::length)
.thenComparing(String::compareTo)
);
set.add("apple");
set.add("pie");
set.add("banana");
// set = [pie, apple, banana] (sorted by length)

// Reverse order
TreeSet<Integer> reversed = new TreeSet<>(Collections.reverseOrder());
reversed.addAll(Arrays.asList(1, 5, 3, 9, 2));
// reversed = [9, 5, 3, 2, 1]
```

---

## Adding Elements

### Add Single Element
```java
TreeSet<Integer> set = new TreeSet<>();

// Add elements (automatically sorted)
boolean added1 = set.add(5); // true
boolean added2 = set.add(2); // true
boolean added3 = set.add(8); // true
boolean added4 = set.add(2); // false (duplicate)

System.out.println(set); // [2, 5, 8] (sorted)
```

### Add Multiple Elements
```java
TreeSet<Integer> set = new TreeSet<>();

// AddAll from collection
set.addAll(Arrays.asList(5, 2, 8, 1, 9));
// set = [1, 2, 5, 8, 9] (automatically sorted)
```

---

## Removing Elements

### Remove Elements
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

// Remove specific element
boolean removed = set.remove(5); // true
// set = [1, 2, 8, 9]

// Remove first (lowest)
Integer first = set.pollFirst(); // 1
// set = [2, 8, 9]

// Remove last (highest)
Integer last = set.pollLast(); // 9
// set = [2, 8]
```

### Clear All
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

set.clear(); // Removes all elements
System.out.println(set.size()); // 0
```

---

## Navigation Methods

### First and Last
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

// Get first (lowest) element
Integer first = set.first(); // 1

// Get last (highest) element
Integer last = set.last(); // 9

// NoSuchElementException if empty
TreeSet<Integer> empty = new TreeSet<>();
// Integer error = empty.first(); // Throws exception
```

### Lower and Higher
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

// Lower: greatest element < given element
Integer lower = set.lower(5); // 2 (less than 5)
Integer lower2 = set.lower(1); // null (no element less than 1)

// Higher: smallest element > given element
Integer higher = set.higher(5); // 8 (greater than 5)
Integer higher2 = set.higher(9); // null (no element greater than 9)
```

### Floor and Ceiling
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

// Floor: greatest element <= given element
Integer floor1 = set.floor(5); // 5 (equal)
Integer floor2 = set.floor(6); // 5 (less than 6)
Integer floor3 = set.floor(0); // null (no element <= 0)

// Ceiling: smallest element >= given element
Integer ceil1 = set.ceiling(5); // 5 (equal)
Integer ceil2 = set.ceiling(6); // 8 (greater than 6)
Integer ceil3 = set.ceiling(10); // null (no element >= 10)
```

---

## Range Operations

### SubSet
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// SubSet (fromElement inclusive, toElement exclusive)
Set<Integer> subset = set.subSet(3, 7);
// subset = [3, 4, 5, 6]

// With inclusive/exclusive flags
Set<Integer> subset2 = set.subSet(3, true, 7, true);
// subset2 = [3, 4, 5, 6, 7]

// Changes to subset reflect in original
subset.remove(4);
// set = [1, 2, 3, 5, 6, 7, 8, 9, 10]
```

### HeadSet
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// HeadSet: elements < toElement (exclusive by default)
Set<Integer> headSet = set.headSet(5);
// headSet = [1, 2, 3, 4]

// With inclusive flag
Set<Integer> headSet2 = set.headSet(5, true);
// headSet2 = [1, 2, 3, 4, 5]
```

### TailSet
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// TailSet: elements >= fromElement (inclusive by default)
Set<Integer> tailSet = set.tailSet(7);
// tailSet = [7, 8, 9, 10]

// With exclusive flag
Set<Integer> tailSet2 = set.tailSet(7, false);
// tailSet2 = [8, 9, 10]
```

### Descending Set
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

// Get descending view
NavigableSet<Integer> descending = set.descendingSet();
// descending = [9, 8, 5, 2, 1]

// Changes reflect in original
descending.add(7);
// set = [1, 2, 5, 7, 8, 9]
```

---

## Checking Elements

### Contains
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

boolean has5 = set.contains(5); // true
boolean has7 = set.contains(7); // false
```

### Size and Empty
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 5, 8, 9));

int size = set.size(); // 5
boolean isEmpty = set.isEmpty(); // false

set.clear();
boolean nowEmpty = set.isEmpty(); // true
```

---

## Iteration

### Ascending Order (Default)
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(5, 2, 8, 1, 9));

// For-each (ascending order)
for (Integer num : set) {
    System.out.println(num); // 1, 2, 5, 8, 9
}

// Using iterator
Iterator<Integer> iterator = set.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

### Descending Order
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(5, 2, 8, 1, 9));

// Descending iterator
Iterator<Integer> descIterator = set.descendingIterator();
while (descIterator.hasNext()) {
    System.out.println(descIterator.next()); // 9, 8, 5, 2, 1
}

// Or use descendingSet
for (Integer num : set.descendingSet()) {
    System.out.println(num); // 9, 8, 5, 2, 1
}
```

### Using Streams
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(5, 2, 8, 1, 9));

// Stream operations
set.stream()
.filter(n -> n > 3)
.forEach(System.out::println); // 5, 8, 9
```

---

## Comparator Operations

### Get Comparator
```java
// TreeSet with natural ordering
TreeSet<Integer> naturalSet = new TreeSet<>();
Comparator<? super Integer> comp1 = naturalSet.comparator();
// comp1 = null (natural ordering)

// TreeSet with custom comparator
TreeSet<Integer> customSet = new TreeSet<>(Collections.reverseOrder());
Comparator<? super Integer> comp2 = customSet.comparator();
// comp2 = ReverseComparator
```

---

## Common Patterns

### Remove Duplicates and Sort
```java
List<Integer> listWithDuplicates = Arrays.asList(5, 2, 8, 2, 1, 9, 5);

// TreeSet automatically removes duplicates and sorts
TreeSet<Integer> sorted = new TreeSet<>(listWithDuplicates);
// sorted = [1, 2, 5, 8, 9]

// Convert back to list if needed
List<Integer> sortedList = new ArrayList<>(sorted);
```

### Find kth Smallest/Largest
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(5, 2, 8, 1, 9, 3, 7));

// Find 3rd smallest
int k = 3;
Integer kthSmallest = set.stream()
.skip(k - 1)
.findFirst()
.orElse(null);
// kthSmallest = 5

// Find 3rd largest
Integer kthLargest = set.descendingSet()
.stream()
.skip(k - 1)
.findFirst()
.orElse(null);
// kthLargest = 5
```

### Range Queries
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// Find all elements in range [3, 7]
Set<Integer> range = set.subSet(3, true, 7, true);
// range = [3, 4, 5, 6, 7]

// Count elements in range
long count = set.subSet(3, true, 7, true).size(); // 5
```

### Closest Value
```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 5, 10, 15, 20));

int target = 12;

// Find closest value
Integer floor = set.floor(target); // 10
Integer ceiling = set.ceiling(target); // 15

Integer closest;
if (floor == null) {
    closest = ceiling;
} else if (ceiling == null) {
closest = floor;
} else {
closest = (target - floor < ceiling - target) ? floor : ceiling;
}
// closest = 10
```

---

## Best Practices

### 1. Elements Must Be Comparable
```java
// ✅ Good - Integer implements Comparable
TreeSet<Integer> numbers = new TreeSet<>();

// ❌ Error - Person doesn't implement Comparable
// TreeSet<Person> people = new TreeSet<>(); // ClassCastException

// ✅ Good - Provide comparator
TreeSet<Person> people = new TreeSet<>(Comparator.comparing(p -> p.name));
```

### 2. No Null Elements
```java
TreeSet<Integer> set = new TreeSet<>();
// set.add(null); // ❌ Throws NullPointerException

// HashSet allows null
Set<Integer> hashSet = new HashSet<>();
hashSet.add(null); // ✅ OK
```

### 3. Consistent equals() and compareTo()
```java
// If implementing Comparable, ensure consistency
class Person implements Comparable<Person> {
    String name;
    @Override
    public int compareTo(Person other) {
        return this.name.compareTo(other.name);
    }
@Override
public boolean equals(Object obj) {
    // Must be consistent with compareTo
    if (!(obj instanceof Person)) return false;
    return this.name.equals(((Person) obj).name);
}
@Override
public int hashCode() {
    return name.hashCode();
}
}
```

### 4. Choose Right Set Implementation
```java
// Need sorted order: TreeSet
TreeSet<Integer> sorted = new TreeSet<>();

// Need fast operations, no order: HashSet
HashSet<Integer> fast = new HashSet<>();

// Need insertion order: LinkedHashSet
LinkedHashSet<Integer> ordered = new LinkedHashSet<>();
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `set.add(element)` | Add element in sorted position | `boolean` - true if added, false if duplicate | `boolean added = set.add(5);` |
| `set.addAll(collection)` | Add all elements maintaining sorted order | `boolean` - true if set changed | `set.addAll(Arrays.asList(1, 2, 3));` |
| `set.remove(element)` | Remove specific element | `boolean` - true if removed, false if not present | `boolean removed = set.remove(5);` |
| `set.pollFirst()` | Remove and return first (lowest) element | `E` - first element or null if empty | `Integer first = set.pollFirst();` |
| `set.pollLast()` | Remove and return last (highest) element | `E` - last element or null if empty | `Integer last = set.pollLast();` |
| `set.clear()` | Remove all elements | `void` - empties the set | `set.clear();` |
| `set.contains(element)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = set.contains(5);` |
| `set.size()` | Get number of elements | `int` - count of elements | `int size = set.size();` |
| `set.isEmpty()` | Check if set is empty | `boolean` - true if no elements, false otherwise | `boolean empty = set.isEmpty();` |
| `set.first()` | Get first (lowest) element | `E` - first element, throws exception if empty | `Integer first = set.first();` |
| `set.last()` | Get last (highest) element | `E` - last element, throws exception if empty | `Integer last = set.last();` |
| `set.lower(element)` | Get greatest element strictly less than given | `E` - lower element or null if none | `Integer lower = set.lower(5);` |
| `set.higher(element)` | Get smallest element strictly greater than given | `E` - higher element or null if none | `Integer higher = set.higher(5);` |
| `set.floor(element)` | Get greatest element less than or equal to given | `E` - floor element or null if none | `Integer floor = set.floor(5);` |
| `set.ceiling(element)` | Get smallest element greater than or equal to given | `E` - ceiling element or null if none | `Integer ceil = set.ceiling(5);` |
| `set.subSet(from, to)` | Get view of range (from inclusive, to exclusive) | `NavigableSet<E>` - subset view, changes reflect | `Set<Integer> sub = set.subSet(3, 7);` |
| `set.subSet(from, fromInc, to, toInc)` | Get range with inclusive/exclusive flags | `NavigableSet<E>` - subset with custom bounds | `Set<E> sub = set.subSet(3, true, 7, true);` |
| `set.headSet(toElement)` | Get elements less than toElement (exclusive) | `NavigableSet<E>` - head subset | `Set<Integer> head = set.headSet(5);` |
| `set.headSet(toElement, inclusive)` | Get head with inclusive flag | `NavigableSet<E>` - head subset | `Set<E> head = set.headSet(5, true);` |
| `set.tailSet(fromElement)` | Get elements >= fromElement (inclusive) | `NavigableSet<E>` - tail subset | `Set<Integer> tail = set.tailSet(5);` |
| `set.tailSet(fromElement, inclusive)` | Get tail with inclusive flag | `NavigableSet<E>` - tail subset | `Set<E> tail = set.tailSet(5, false);` |
| `set.descendingSet()` | Get reverse order view | `NavigableSet<E>` - descending view, changes reflect | `NavigableSet<E> desc = set.descendingSet();` |
| `set.descendingIterator()` | Get reverse order iterator | `Iterator<E>` - descending iterator | `Iterator<E> it = set.descendingIterator();` |
| `set.iterator()` | Get ascending order iterator | `Iterator<E>` - ascending iterator | `Iterator<Integer> it = set.iterator();` |
| `set.comparator()` | Get the comparator used | `Comparator<? super E>` - comparator or null if natural | `Comparator<E> comp = set.comparator();` |
| `set.forEach(action)` | Perform action for each element (Java 8+) | `void` - iterates in sorted order | `set.forEach(System.out::println);` |
| `set.stream()` | Create stream (Java 8+) | `Stream<E>` - stream in sorted order | `set.stream().filter(n -> n > 5);` |