# Java Queue Operations Guide

A comprehensive guide to working with Queue, PriorityQueue, and Deque in Java, covering FIFO operations, priority ordering, and implementations.

## Table of Contents
1. [Queue Basics](#queue-basics)
2. [Queue Creation](#queue-creation)
3. [Adding Elements](#adding-elements)
4. [Removing Elements](#removing-elements)
5. [Examining Elements](#examining-elements)
6. [PriorityQueue](#priorityqueue)
7. [Deque Operations](#deque-operations)
8. [Common Patterns](#common-patterns)

---

## Queue Basics

### What is a Queue?
```java
import java.util.Queue;
import java.util.LinkedList;

// Queue is a FIFO (First In, First Out) data structure
// - Interface (not a class)
// - Common implementations: LinkedList, ArrayDeque, PriorityQueue
// - offer/poll/peek for graceful handling
// - add/remove/element throw exceptions

Queue<String> queue = new LinkedList<>();
```

### FIFO Principle
```java
Queue<Integer> queue = new LinkedList<>();

queue.offer(1); // [1]
queue.offer(2); // [1, 2]
queue.offer(3); // [1, 2, 3]

System.out.println(queue.poll()); // 1 (first in, first out)
System.out.println(queue.poll()); // 2
System.out.println(queue.poll()); // 3
```

### Queue Implementations Comparison
```java
// LinkedList: General-purpose queue
Queue<String> linkedQueue = new LinkedList<>();

// ArrayDeque: Faster, no capacity restrictions
Queue<String> arrayQueue = new ArrayDeque<>();

// PriorityQueue: Elements ordered by priority
Queue<Integer> priorityQueue = new PriorityQueue<>();
```

---

## Queue Creation

### LinkedList Queue
```java
import java.util.Queue;
import java.util.LinkedList;

// Create empty queue
Queue<String> queue1 = new LinkedList<>();

// Create and initialize
Queue<Integer> queue2 = new LinkedList<>(Arrays.asList(1, 2, 3));
```

### ArrayDeque Queue (Recommended)
```java
import java.util.Queue;
import java.util.ArrayDeque;

// Create empty queue (faster than LinkedList)
Queue<String> queue1 = new ArrayDeque<>();

// Create with initial capacity
Queue<Integer> queue2 = new ArrayDeque<>(100);

// From collection
Queue<String> queue3 = new ArrayDeque<>(Arrays.asList("A", "B", "C"));
```

### PriorityQueue
```java
import java.util.PriorityQueue;

// Natural ordering (min-heap)
Queue<Integer> minHeap = new PriorityQueue<>();

// Custom comparator (max-heap)
Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// With initial capacity
Queue<Integer> queue = new PriorityQueue<>(20);
```

---

## Adding Elements

### offer() - Graceful Addition
```java
Queue<String> queue = new LinkedList<>();

// offer() returns true if successful, false if failed
boolean added1 = queue.offer("First"); // true
boolean added2 = queue.offer("Second"); // true
boolean added3 = queue.offer("Third"); // true

// Queue: [First, Second, Third]
// Front: First, Rear: Third
```

### add() - Exception on Failure
```java
Queue<String> queue = new LinkedList<>();

// add() returns true or throws IllegalStateException
queue.add("First");
queue.add("Second");
queue.add("Third");

// Same result as offer() for unbounded queues
// For bounded queues, add() throws exception when full
```

### addAll() - Bulk Addition
```java
Queue<String> queue = new LinkedList<>();
List<String> items = Arrays.asList("A", "B", "C");

queue.addAll(items);
// Queue: [A, B, C]
```

---

## Removing Elements

### poll() - Remove Head (Null-Safe)
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("First", "Second", "Third"));

// poll() removes and returns head, or null if empty
String removed1 = queue.poll(); // "First"
String removed2 = queue.poll(); // "Second"
String removed3 = queue.poll(); // "Third"
String removed4 = queue.poll(); // null (empty queue)

// ✅ Safe for empty queues
```

### remove() - Remove Head (Exception on Empty)
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("First", "Second"));

// remove() throws NoSuchElementException if empty
String removed1 = queue.remove(); // "First"
String removed2 = queue.remove(); // "Second"
// String error = queue.remove(); // Throws NoSuchElementException
```

### remove(Object) - Remove Specific Element
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("A", "B", "C", "B"));

// Remove first occurrence
boolean removed = queue.remove("B"); // true, removes first B
// Queue: [A, C, B]

boolean notFound = queue.remove("D"); // false
```

### clear() - Remove All
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("A", "B", "C"));

queue.clear();
System.out.println(queue.size()); // 0
```

---

## Examining Elements

### peek() - View Head (Null-Safe)
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("First", "Second", "Third"));

// peek() returns head without removing, or null if empty
String head1 = queue.peek(); // "First"
String head2 = queue.peek(); // Still "First"
System.out.println(queue.size()); // Still 3

// Empty queue
Queue<String> empty = new LinkedList<>();
String head3 = empty.peek(); // null (no exception)
```

### element() - View Head (Exception on Empty)
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("First", "Second"));

// element() returns head or throws NoSuchElementException
String head1 = queue.element(); // "First"

// Empty queue
Queue<String> empty = new LinkedList<>();
// String error = empty.element(); // Throws NoSuchElementException
```

### contains() - Check Existence
```java
Queue<String> queue = new LinkedList<>(Arrays.asList("A", "B", "C"));

boolean hasB = queue.contains("B"); // true
boolean hasD = queue.contains("D"); // false
```

### size() and isEmpty()
```java
Queue<String> queue = new LinkedList<>();

boolean empty1 = queue.isEmpty(); // true
int size1 = queue.size(); // 0

queue.offer("Item");
boolean empty2 = queue.isEmpty(); // false
int size2 = queue.size(); // 1
```

---

## PriorityQueue

### Natural Ordering (Min-Heap)
```java
Queue<Integer> minHeap = new PriorityQueue<>();

minHeap.offer(5);
minHeap.offer(2);
minHeap.offer(8);
minHeap.offer(1);

// Elements ordered by natural ordering (ascending)
System.out.println(minHeap.poll()); // 1 (smallest)
System.out.println(minHeap.poll()); // 2
System.out.println(minHeap.poll()); // 5
System.out.println(minHeap.poll()); // 8
```

### Custom Comparator (Max-Heap)
```java
Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

maxHeap.offer(5);
maxHeap.offer(2);
maxHeap.offer(8);
maxHeap.offer(1);

// Elements ordered in descending order
System.out.println(maxHeap.poll()); // 8 (largest)
System.out.println(maxHeap.poll()); // 5
System.out.println(maxHeap.poll()); // 2
System.out.println(maxHeap.poll()); // 1
```

### Custom Objects with Priority
```java
class Task implements Comparable<Task> {
    String name;
    int priority; // Lower number = higher priority
    public Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }
@Override
public int compareTo(Task other) {
    return Integer.compare(this.priority, other.priority);
}
}

Queue<Task> taskQueue = new PriorityQueue<>();
taskQueue.offer(new Task("Low Priority", 5));
taskQueue.offer(new Task("High Priority", 1));
taskQueue.offer(new Task("Medium Priority", 3));

Task next = taskQueue.poll(); // High Priority (priority=1)
```

### Using Comparator with PriorityQueue
```java
// Sort by string length (shortest first)
Queue<String> queue = new PriorityQueue<>(Comparator.comparingInt(String::length));

queue.offer("Hello");
queue.offer("Hi");
queue.offer("World");

System.out.println(queue.poll()); // "Hi" (length 2)
System.out.println(queue.poll()); // "Hello" (length 5)
System.out.println(queue.poll()); // "World" (length 5)
```

---

## Deque Operations

### Deque as Double-Ended Queue
```java
import java.util.Deque;
import java.util.ArrayDeque;

Deque<String> deque = new ArrayDeque<>();

// Add to front
deque.offerFirst("First");
deque.offerFirst("Zero"); // [Zero, First]

// Add to rear
deque.offerLast("Second");
deque.offerLast("Third"); // [Zero, First, Second, Third]

// Remove from front
String front = deque.pollFirst(); // "Zero"

// Remove from rear
String rear = deque.pollLast(); // "Third"
```

### Deque as Queue (FIFO)
```java
Deque<String> queue = new ArrayDeque<>();

// Use as regular queue
queue.offer("First"); // or offerLast()
queue.offer("Second");
queue.offer("Third");

String item = queue.poll(); // "First" (or pollFirst())
```

### Deque as Stack (LIFO)
```java
Deque<String> stack = new ArrayDeque<>();

// Use as stack
stack.push("First"); // or offerFirst()
stack.push("Second");
stack.push("Third");

String item = stack.pop(); // "Third" (or pollFirst())
```

---

## Common Patterns

### Level-Order Tree Traversal (BFS)
```java
public static void levelOrder(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        System.out.println(node.val);
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
    }
}
```

### Moving Average
```java
class MovingAverage {
    private Queue<Integer> queue;
    private int size;
    private double sum;
    public MovingAverage(int size) {
        this.queue = new LinkedList<>();
        this.size = size;
        this.sum = 0;
    }
public double next(int val) {
    queue.offer(val);
    sum += val;
    if (queue.size() > size) {
        sum -= queue.poll();
    }
return sum / queue.size();
}
}

// Usage
MovingAverage ma = new MovingAverage(3);
System.out.println(ma.next(1)); // 1.0
System.out.println(ma.next(10)); // 5.5
System.out.println(ma.next(3)); // 4.67
System.out.println(ma.next(5)); // 6.0 (only last 3)
```

### Task Scheduler with Priority
```java
class TaskScheduler {
    private Queue<Task> queue = new PriorityQueue<>();
    public void scheduleTask(String name, int priority) {
        queue.offer(new Task(name, priority));
    }
public Task getNextTask() {
    return queue.poll(); // Returns highest priority task
}
public boolean hasTasks() {
    return !queue.isEmpty();
}
}

// Usage
TaskScheduler scheduler = new TaskScheduler();
scheduler.scheduleTask("Email", 2);
scheduler.scheduleTask("Critical Bug", 1);
scheduler.scheduleTask("Refactor", 3);

Task next = scheduler.getNextTask(); // Critical Bug (priority 1)
```

### Find K Largest Elements
```java
public static List<Integer> kLargest(int[] nums, int k) {
    // Min-heap of size k
    Queue<Integer> minHeap = new PriorityQueue<>();
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll(); // Remove smallest
        }
}
return new ArrayList<>(minHeap);
}

// Usage
int[] nums = {3, 2, 1, 5, 6, 4};
List<Integer> result = kLargest(nums, 2); // [5, 6]
```

### Sliding Window Maximum
```java
public static int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> deque = new ArrayDeque<>();
    int[] result = new int[nums.length - k + 1];
    for (int i = 0; i < nums.length; i++) {
        // Remove indices outside window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
    // Remove smaller elements
    while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
        deque.pollLast();
    }
deque.offerLast(i);
// Add to result
if (i >= k - 1) {
    result[i - k + 1] = nums[deque.peekFirst()];
}
}
return result;
}
```

### Hot Potato (Josephus Problem)
```java
public static String hotPotato(List<String> names, int num) {
    Queue<String> queue = new LinkedList<>(names);
    while (queue.size() > 1) {
        // Pass the potato num times
        for (int i = 0; i < num; i++) {
            queue.offer(queue.poll());
        }
    // Remove the person holding potato
    queue.poll();
}
return queue.poll(); // Winner
}

// Usage
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Diana");
String winner = hotPotato(names, 3); // "Diana"
```

---

## Best Practices

### 1. Choose Right Implementation
```java
// ✅ ArrayDeque for general-purpose queue (fastest)
Queue<String> queue1 = new ArrayDeque<>();

// ✅ LinkedList when frequent add/remove at both ends
Queue<String> queue2 = new LinkedList<>();

// ✅ PriorityQueue when elements need ordering
Queue<Integer> queue3 = new PriorityQueue<>();
```

### 2. Use Null-Safe Methods
```java
Queue<String> queue = new LinkedList<>();

// ✅ Null-safe methods (return null when empty)
String item1 = queue.poll(); // null if empty
String item2 = queue.peek(); // null if empty

// ❌ Exception-throwing methods
// String item3 = queue.remove(); // NoSuchElementException if empty
// String item4 = queue.element(); // NoSuchElementException if empty
```

### 3. PriorityQueue Limitations
```java
// ❌ PriorityQueue does NOT guarantee order in iteration
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(5);
pq.offer(2);
pq.offer(8);

// Don't do this!
for (Integer num : pq) {
    System.out.println(num); // Order is not guaranteed!
}

// ✅ Always use poll() for ordered retrieval
while (!pq.isEmpty()) {
    System.out.println(pq.poll()); // 2, 5, 8 (guaranteed order)
}
```

### 4. Avoid Null Elements
```java
// ❌ Most Queue implementations don't allow null
Queue<String> queue = new LinkedList<>();
// queue.offer(null); // NullPointerException in many implementations
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `queue.offer(element)` | Add element to rear (null-safe) | `boolean` - true if added, false if failed | `boolean added = queue.offer("Item");` |
| `queue.add(element)` | Add element to rear (exception on failure) | `boolean` - true if added, throws if failed | `queue.add("Item");` |
| `queue.poll()` | Remove and return head (null-safe) | `E` - head element or null if empty | `String head = queue.poll();` |
| `queue.remove()` | Remove and return head (exception on empty) | `E` - head element, throws if empty | `String head = queue.remove();` |
| `queue.peek()` | View head without removing (null-safe) | `E` - head element or null if empty | `String head = queue.peek();` |
| `queue.element()` | View head without removing (exception on empty) | `E` - head element, throws if empty | `String head = queue.element();` |
| `queue.size()` | Get number of elements | `int` - count of elements | `int size = queue.size();` |
| `queue.isEmpty()` | Check if queue is empty | `boolean` - true if empty, false otherwise | `boolean empty = queue.isEmpty();` |
| `queue.clear()` | Remove all elements | `void` - empties the queue | `queue.clear();` |
| `queue.contains(object)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = queue.contains("Item");` |
| `queue.remove(object)` | Remove first occurrence | `boolean` - true if removed, false otherwise | `boolean removed = queue.remove("Item");` |
| `queue.iterator()` | Get iterator | `Iterator<E>` - iterates from head to tail | `Iterator<String> it = queue.iterator();` |
| **Deque Methods** ||||
| `deque.offerFirst(element)` | Add to front | `boolean` - true if added | `deque.offerFirst("First");` |
| `deque.offerLast(element)` | Add to rear | `boolean` - true if added | `deque.offerLast("Last");` |
| `deque.pollFirst()` | Remove from front | `E` - front element or null if empty | `String front = deque.pollFirst();` |
| `deque.pollLast()` | Remove from rear | `E` - rear element or null if empty | `String rear = deque.pollLast();` |
| `deque.peekFirst()` | View front without removing | `E` - front element or null if empty | `String front = deque.peekFirst();` |
| `deque.peekLast()` | View rear without removing | `E` - rear element or null if empty | `String rear = deque.peekLast();` |
| **PriorityQueue Methods** ||||
| `pq.offer(element)` | Add element (reordered by priority) | `boolean` - true if added | `pq.offer(5);` |
| `pq.poll()` | Remove highest priority element | `E` - highest priority or null if empty | `Integer min = pq.poll();` |
| `pq.peek()` | View highest priority element | `E` - highest priority or null if empty | `Integer min = pq.peek();` |
| `pq.comparator()` | Get comparator used | `Comparator<E>` - comparator or null if natural | `Comparator<Integer> comp = pq.comparator();` |
