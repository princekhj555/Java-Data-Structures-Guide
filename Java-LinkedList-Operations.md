# Java LinkedList Operations Guide

A comprehensive guide to working with LinkedList in Java, covering doubly-linked list operations, deque operations, and common use cases.

## Table of Contents
1. [LinkedList Basics](#linkedlist-basics)
2. [LinkedList Creation](#linkedlist-creation)
3. [Adding Elements](#adding-elements)
4. [Accessing Elements](#accessing-elements)
5. [Removing Elements](#removing-elements)
6. [Deque Operations](#deque-operations)
7. [Queue Operations](#queue-operations)
8. [Searching](#searching)
9. [Iteration](#iteration)
10. [Common Patterns](#common-patterns)

---

## LinkedList Basics

### What is a LinkedList?
```java
import java.util.LinkedList;

// LinkedList is a doubly-linked list implementation
// - Implements List, Deque, and Queue interfaces
// - Efficient insertion/deletion at both ends: O(1)
// - Inefficient random access: O(n)
// - Each element has reference to next and previous
// - More memory overhead than ArrayList
// - Good for frequent insertions/deletions

LinkedList<String> list = new LinkedList<>();
```

---

## LinkedList Creation

### Basic Creation
```java
import java.util.LinkedList;
import java.util.List;

// As List
List<String> list1 = new LinkedList<>();

// As Deque
Deque<String> deque = new LinkedList<>();

// As Queue
Queue<String> queue = new LinkedList<>();

// Concrete type
LinkedList<String> linkedList = new LinkedList<>();
```

### Initialize with Values
```java
// From collection
LinkedList<String> list1 = new LinkedList<>(Arrays.asList("A", "B", "C"));

// Using Stream
LinkedList<Integer> list2 = Stream.of(1, 2, 3, 4, 5)
.collect(Collectors.toCollection(LinkedList::new));
```

---

## Adding Elements

### Add at End
```java
LinkedList<String> list = new LinkedList<>();

// Add to end
list.add("Apple");
list.add("Banana");
// list = [Apple, Banana]

// Or use addLast (equivalent)
list.addLast("Orange");
// list = [Apple, Banana, Orange]
```

### Add at Beginning
```java
LinkedList<String> list = new LinkedList<>();
list.add("Banana");
list.add("Orange");

// Add to beginning
list.addFirst("Apple");
// list = [Apple, Banana, Orange]
```

### Add at Index
```java
LinkedList<String> list = new LinkedList<>();
list.add("Apple");
list.add("Orange");

// Insert at index
list.add(1, "Banana");
// list = [Apple, Banana, Orange]
```

### Add Multiple Elements
```java
LinkedList<String> list = new LinkedList<>();
list.add("Apple");

// AddAll at end
list.addAll(Arrays.asList("Banana", "Orange"));
// list = [Apple, Banana, Orange]

// AddAll at index
list.addAll(1, Arrays.asList("Mango", "Grape"));
// list = [Apple, Mango, Grape, Banana, Orange]
```

---

## Accessing Elements

### Get by Index
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Get by index (O(n) operation)
String first = list.get(0); // "Apple"
String second = list.get(1); // "Banana"
```

### Get First and Last (O(1))
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Get first element
String first = list.getFirst(); // "Apple"

// Get last element
String last = list.getLast(); // "Orange"

// Peek (returns null if empty, doesn't throw exception)
String peekFirst = list.peekFirst(); // "Apple"
String peekLast = list.peekLast(); // "Orange"

// Empty list
LinkedList<String> empty = new LinkedList<>();
// String error = empty.getFirst(); // Throws NoSuchElementException
String safe = empty.peekFirst(); // null (safe)
```

---

## Removing Elements

### Remove by Index
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Remove by index (O(n) operation)
String removed = list.remove(1); // "Banana"
// list = [Apple, Orange]
```

### Remove by Object
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Remove first occurrence
boolean removed = list.remove("Banana"); // true
// list = [Apple, Orange]
```

### Remove First and Last (O(1))
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Remove first
String first = list.removeFirst(); // "Apple"
// list = [Banana, Orange]

// Remove last
String last = list.removeLast(); // "Orange"
// list = [Banana]

// Poll (returns null if empty, doesn't throw exception)
String pollFirst = list.pollFirst(); // "Banana"
String pollLast = list.pollLast(); // null (empty now)
```

### Remove First/Last Occurrence
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("A", "B", "C", "B", "D"));

// Remove first occurrence
boolean removed1 = list.removeFirstOccurrence("B"); // true
// list = [A, C, B, D]

// Remove last occurrence
boolean removed2 = list.removeLastOccurrence("B"); // true
// list = [A, C, D]
```

### Clear All
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana"));

list.clear(); // Removes all elements
System.out.println(list.size()); // 0
```

---

## Deque Operations

### Push and Pop (Stack Operations)
```java
LinkedList<String> stack = new LinkedList<>();

// Push (add to front)
stack.push("First");
stack.push("Second");
stack.push("Third");
// stack = [Third, Second, First]

// Pop (remove from front)
String popped = stack.pop(); // "Third"
// stack = [Second, First]

// Peek (view front without removing)
String top = stack.peek(); // "Second"
```

### Offer and Poll (Queue Operations)
```java
LinkedList<String> queue = new LinkedList<>();

// Offer (add to end)
queue.offer("First");
queue.offer("Second");
queue.offer("Third");
// queue = [First, Second, Third]

// Poll (remove from front)
String polled = queue.poll(); // "First"
// queue = [Second, Third]

// Peek (view front)
String front = queue.peek(); // "Second"
```

### Double-Ended Operations
```java
LinkedList<String> deque = new LinkedList<>();

// Add to both ends
deque.offerFirst("Middle");
deque.offerFirst("First");
deque.offerLast("Last");
// deque = [First, Middle, Last]

// Remove from both ends
String first = deque.pollFirst(); // "First"
String last = deque.pollLast(); // "Last"
// deque = [Middle]
```

---

## Queue Operations

### Element vs Peek
```java
LinkedList<String> queue = new LinkedList<>();
queue.add("Apple");

// Element: throws exception if empty
String element = queue.element(); // "Apple"

// Peek: returns null if empty
String peek = queue.peek(); // "Apple"

// Empty queue
queue.clear();
// String error = queue.element(); // Throws NoSuchElementException
String safe = queue.peek(); // null
```

### Add vs Offer
```java
LinkedList<String> queue = new LinkedList<>();

// Add: throws exception on failure
boolean added = queue.add("Apple"); // true

// Offer: returns false on failure (for capacity-restricted queues)
boolean offered = queue.offer("Banana"); // true
// For LinkedList, both behave similarly
```

---

## Searching

### Index Of
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("A", "B", "C", "B", "D"));

// First occurrence
int firstIndex = list.indexOf("B"); // 1

// Last occurrence
int lastIndex = list.lastIndexOf("B"); // 3

// Not found
int notFound = list.indexOf("E"); // -1
```

### Contains
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

boolean hasApple = list.contains("Apple"); // true
boolean hasMango = list.contains("Mango"); // false
```

---

## Iteration

### For Loop
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Traditional for loop (not recommended for LinkedList - O(n²))
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

### Enhanced For Loop
```java
// For-each (recommended - O(n))
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
    // Safe removal during iteration
    if (fruit.equals("Banana")) {
        iterator.remove();
    }
}
```

### ListIterator (Bidirectional)
```java
ListIterator<String> listIterator = list.listIterator();

// Forward
while (listIterator.hasNext()) {
    System.out.println(listIterator.next());
}

// Backward
while (listIterator.hasPrevious()) {
    System.out.println(listIterator.previous());
}
```

### Descending Iterator
```java
Iterator<String> descIterator = list.descendingIterator();
while (descIterator.hasNext()) {
    System.out.println(descIterator.next()); // Reverse order
}
```

---

## Common Patterns

### Use as Stack (LIFO)
```java
LinkedList<Integer> stack = new LinkedList<>();

// Push elements
stack.push(1);
stack.push(2);
stack.push(3);

// Pop elements
while (!stack.isEmpty()) {
    System.out.println(stack.pop()); // 3, 2, 1
}
```

### Use as Queue (FIFO)
```java
LinkedList<String> queue = new LinkedList<>();

// Enqueue
queue.offer("First");
queue.offer("Second");
queue.offer("Third");

// Dequeue
while (!queue.isEmpty()) {
    System.out.println(queue.poll()); // First, Second, Third
}
```

### Reverse List
```java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1, 2, 3, 4, 5));

Collections.reverse(list);
// list = [5, 4, 3, 2, 1]

// Or create reversed copy
LinkedList<Integer> reversed = new LinkedList<>();
list.descendingIterator().forEachRemaining(reversed::add);
```

### Convert to Array
```java
LinkedList<String> list = new LinkedList<>(Arrays.asList("Apple", "Banana", "Orange"));

// To Object array
Object[] objArray = list.toArray();

// To typed array
String[] strArray = list.toArray(new String[0]);
```

---

## ArrayList vs LinkedList

### When to Use LinkedList
```java
// ✅ Frequent insertions/deletions at beginning or end
LinkedList<String> list = new LinkedList<>();
list.addFirst("New Item"); // O(1)
list.removeFirst(); // O(1)

// ✅ Use as Stack or Queue
LinkedList<String> stack = new LinkedList<>();
stack.push("Item");
stack.pop();

// ✅ Frequent insertions/deletions in middle (if you have iterator)
ListIterator<String> it = list.listIterator();
it.add("New"); // O(1) with iterator
```

### When to Use ArrayList
```java
// ✅ Frequent random access
ArrayList<String> arrayList = new ArrayList<>();
String element = arrayList.get(100); // O(1)

// ✅ Iteration and reading
for (String s : arrayList) { // Faster than LinkedList
    System.out.println(s);
}

// ✅ Less memory overhead
// ArrayList uses less memory per element
```

---

## Best Practices

### 1. Avoid get(index) in Loops
```java
// ❌ Bad - O(n²) for LinkedList
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}

// ✅ Good - O(n)
for (String item : list) {
    System.out.println(item);
}
```

### 2. Use Appropriate Methods
```java
// ✅ Use addFirst/addLast for beginning/end
list.addFirst("First"); // O(1)
list.addLast("Last"); // O(1)

// ❌ Avoid add(0, element) if you can use addFirst
list.add(0, "First"); // Same as addFirst but less clear
```

### 3. Choose Right Data Structure
```java
// Need random access? Use ArrayList
List<String> randomAccess = new ArrayList<>();

// Need frequent insert/delete at ends? Use LinkedList
List<String> frequentMod = new LinkedList<>();

// Need both? Consider ArrayDeque for deque operations
Deque<String> deque = new ArrayDeque<>(); // Often faster than LinkedList
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `list.add(element)` | Add element to end | `boolean` - always true | `list.add("Apple");` |
| `list.add(index, element)` | Insert element at index | `void` - shifts elements | `list.add(0, "First");` |
| `list.addFirst(element)` | Add to beginning (O(1)) | `void` - efficient | `list.addFirst("First");` |
| `list.addLast(element)` | Add to end (O(1)) | `void` - efficient | `list.addLast("Last");` |
| `list.addAll(collection)` | Add all elements to end | `boolean` - true if changed | `list.addAll(Arrays.asList("A", "B"));` |
| `list.get(index)` | Get element at index (O(n)) | `E` - element at index | `String s = list.get(0);` |
| `list.getFirst()` | Get first element (O(1)) | `E` - first element, throws if empty | `String first = list.getFirst();` |
| `list.getLast()` | Get last element (O(1)) | `E` - last element, throws if empty | `String last = list.getLast();` |
| `list.peekFirst()` | Get first without removing (O(1)) | `E` - first element or null if empty | `String first = list.peekFirst();` |
| `list.peekLast()` | Get last without removing (O(1)) | `E` - last element or null if empty | `String last = list.peekLast();` |
| `list.set(index, element)` | Replace element at index | `E` - previous element at index | `String old = list.set(0, "New");` |
| `list.remove(index)` | Remove element at index | `E` - removed element | `String removed = list.remove(0);` |
| `list.remove(object)` | Remove first occurrence | `boolean` - true if removed, false if not found | `boolean done = list.remove("Apple");` |
| `list.removeFirst()` | Remove first element (O(1)) | `E` - removed first element, throws if empty | `String first = list.removeFirst();` |
| `list.removeLast()` | Remove last element (O(1)) | `E` - removed last element, throws if empty | `String last = list.removeLast();` |
| `list.pollFirst()` | Remove first element (O(1)) | `E` - removed first or null if empty | `String first = list.pollFirst();` |
| `list.pollLast()` | Remove last element (O(1)) | `E` - removed last or null if empty | `String last = list.pollLast();` |
| `list.removeFirstOccurrence(obj)` | Remove first occurrence | `boolean` - true if removed, false otherwise | `boolean done = list.removeFirstOccurrence("A");` |
| `list.removeLastOccurrence(obj)` | Remove last occurrence | `boolean` - true if removed, false otherwise | `boolean done = list.removeLastOccurrence("A");` |
| `list.clear()` | Remove all elements | `void` - empties the list | `list.clear();` |
| `list.contains(object)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = list.contains("Apple");` |
| `list.indexOf(object)` | Find first occurrence index | `int` - index if found, -1 if not found | `int idx = list.indexOf("Apple");` |
| `list.lastIndexOf(object)` | Find last occurrence index | `int` - last index if found, -1 if not found | `int idx = list.lastIndexOf("Apple");` |
| `list.size()` | Get number of elements | `int` - count of elements | `int size = list.size();` |
| `list.isEmpty()` | Check if list is empty | `boolean` - true if no elements, false otherwise | `boolean empty = list.isEmpty();` |
| `list.push(element)` | Push to front (stack operation) | `void` - adds to beginning | `list.push("Item");` |
| `list.pop()` | Pop from front (stack operation) | `E` - removed first element, throws if empty | `String item = list.pop();` |
| `list.offer(element)` | Add to end (queue operation) | `boolean` - always true for LinkedList | `list.offer("Item");` |
| `list.poll()` | Remove from front (queue operation) | `E` - removed first or null if empty | `String item = list.poll();` |
| `list.peek()` | View front element (queue operation) | `E` - first element or null if empty | `String front = list.peek();` |
| `list.element()` | View front element | `E` - first element, throws if empty | `String front = list.element();` |
| `list.offerFirst(element)` | Add to beginning (deque operation) | `boolean` - always true | `list.offerFirst("First");` |
| `list.offerLast(element)` | Add to end (deque operation) | `boolean` - always true | `list.offerLast("Last");` |
| `list.iterator()` | Get forward iterator | `Iterator<E>` - forward iterator | `Iterator<String> it = list.iterator();` |
| `list.listIterator()` | Get bidirectional iterator | `ListIterator<E>` - can go both directions | `ListIterator<String> it = list.listIterator();` |
| `list.descendingIterator()` | Get reverse iterator | `Iterator<E>` - iterates in reverse | `Iterator<String> it = list.descendingIterator();` |
| `list.toArray()` | Convert to Object array | `Object[]` - array of elements | `Object[] arr = list.toArray();` |
| `list.toArray(array)` | Convert to typed array | `T[]` - typed array | `String[] arr = list.toArray(new String[0]);` |
| `list.clone()` | Create shallow copy | `Object` - cloned LinkedList | `LinkedList<E> copy = (LinkedList<E>) list.clone();` |
| `Collections.reverse(list)` | Reverse order of elements | `void` - reverses in-place | `Collections.reverse(list);` |
