# Java Data Structures - Master Guide

**Your Complete Reference for Java Data Structures and Algorithms**

Welcome to the comprehensive Java Data Structures guide! This master document serves as the entry point to detailed guides covering all major data structures in Java, complete with operations, examples, and best practices.

---

## 📚 Complete Guide Collection

### Linear Data Structures

#### 1. [Arrays](Java-Array-Operations.md)
**Fixed-size, contiguous memory structure**
- Basic operations (access, modify, search)
- Array manipulation (copy, fill, sort)
- Multi-dimensional arrays
- 7 types of comparator methods
- **Best for:** Fast random access, fixed-size collections
- **Time Complexity:** Access O(1), Search O(n), Insert/Delete O(n)

#### 2. [String](Java-String-Operations.md)
**Immutable character sequence**
- String creation and manipulation
- Searching and substring operations
- StringBuilder and StringBuffer
- String comparison and formatting
- **Best for:** Text processing, immutable text data
- **Time Complexity:** charAt O(1), substring O(n), concat O(n)

#### 3. [ArrayList](Java-ArrayList-Operations.md)
**Dynamic array with automatic resizing**
- Adding and removing elements
- Sorting and searching
- Bulk operations and subList
- Stream operations
- **Best for:** Dynamic collections, frequent random access
- **Time Complexity:** Access O(1), Add O(1) amortized, Insert/Delete O(n)

#### 4. [LinkedList](Java-LinkedList-Operations.md)
**Doubly-linked list implementation**
- Add/remove at both ends (O(1))
- Deque and Queue operations
- Stack operations (push/pop)
- Bidirectional iteration
- **Best for:** Frequent insertions/deletions at ends, implementing queues/deques
- **Time Complexity:** Access O(n), Insert/Delete at ends O(1)

---

### Set Data Structures

#### 5. [HashSet](Java-HashSet-Operations.md)
**Unordered collection of unique elements**
- Adding and removing elements
- Set operations (union, intersection, difference)
- Contains and bulk operations
- **Best for:** Fast lookups, eliminating duplicates, membership testing
- **Time Complexity:** Add/Remove/Contains O(1) average

#### 6. [TreeSet](Java-TreeSet-Operations.md)
**Sorted set with Red-Black tree implementation**
- Sorted order maintenance
- Navigation methods (first, last, floor, ceiling, lower, higher)
- Range operations (subSet, headSet, tailSet)
- Custom comparators
- **Best for:** Sorted unique elements, range queries, ordered iteration
- **Time Complexity:** Add/Remove/Contains O(log n)

---

### Map Data Structures

#### 7. [HashMap/Map](Java-HashMap-Operations.md)
**Key-value pairs with fast lookup**
- Put, get, and remove operations
- Compute and merge operations
- Entry set iteration
- Java 8+ enhancements
- **Best for:** Fast key-value lookups, caching, counting frequencies
- **Time Complexity:** Put/Get/Remove O(1) average

---

### Stack and Queue

#### 8. [Stack](Java-Stack-Operations.md)
**LIFO (Last In, First Out) structure**
- Push and pop operations
- Peek without removal
- Legacy Stack vs modern Deque
- **Best for:** Undo operations, expression evaluation, DFS, backtracking
- **Time Complexity:** Push/Pop/Peek O(1)

#### 9. [Queue](Java-Queue-Operations.md)
**FIFO (First In, First Out) structure**
- Queue, PriorityQueue, Deque implementations
- Offer/poll/peek operations
- Priority ordering
- **Best for:** BFS, task scheduling, order processing, event handling
- **Time Complexity:** Offer/Poll/Peek O(1) for Queue, O(log n) for PriorityQueue

---

### Tree Data Structures

#### 10. [Trees (Binary Trees, BST)](Java-Trees-Operations.md)
**Hierarchical tree structures**
- TreeNode implementation
- Tree traversals (preorder, inorder, postorder, level-order)
- Binary Search Tree operations
- Common tree algorithms (LCA, path sum, diameter)
- **Best for:** Hierarchical data, searching/sorting, expression trees
- **Time Complexity:** BST operations O(log n) balanced, O(n) worst case

---

## 🎯 Quick Selection Guide

### Choose Data Structure Based on Your Needs:

| Need | Best Choice | Alternative |
|------|-------------|-------------|
| Fast random access by index | **Array**, **ArrayList** | - |
| Frequent insertions/deletions at ends | **LinkedList**, **Deque** | ArrayDeque |
| Unique elements, fast lookup | **HashSet** | TreeSet (if ordering needed) |
| Key-value pairs, fast lookup | **HashMap** | TreeMap (if ordering needed) |
| Sorted unique elements | **TreeSet** | PriorityQueue (partial order) |
| LIFO operations (undo, DFS) | **Deque** (as Stack) | Stack (legacy) |
| FIFO operations (BFS, queues) | **ArrayDeque**, **LinkedList** | - |
| Priority-based processing | **PriorityQueue** | TreeSet |
| Hierarchical relationships | **Trees** | - |
| Text manipulation | **String**, **StringBuilder** | StringBuffer (thread-safe) |

---

## ⚡ Time Complexity Comparison

### Access Operations
| Data Structure | Access by Index/Key | Search |
|---------------|---------------------|--------|
| Array | O(1) | O(n) or O(log n) if sorted |
| ArrayList | O(1) | O(n) |
| LinkedList | O(n) | O(n) |
| HashMap | O(1) average | O(n) for values |
| HashSet | - | O(1) average |
| TreeSet | - | O(log n) |
| Stack | O(n) | O(n) |
| Queue | O(n) | O(n) |

### Insertion/Deletion Operations
| Data Structure | Insert | Delete |
|---------------|--------|--------|
| Array | O(n) | O(n) |
| ArrayList | O(1) at end, O(n) at middle | O(n) |
| LinkedList | O(1) at ends, O(n) at middle | O(1) at ends, O(n) at middle |
| HashMap | O(1) average | O(1) average |
| HashSet | O(1) average | O(1) average |
| TreeSet | O(log n) | O(log n) |
| Stack | O(1) | O(1) |
| Queue | O(1) | O(1) |
| PriorityQueue | O(log n) | O(log n) |

---

## 🔍 Common Use Cases

### Arrays
- Fixed-size data storage
- Matrix operations
- Implementing other data structures
- High-performance scenarios requiring contiguous memory

### String
- Text processing and parsing
- Pattern matching
- Configuration and user input handling
- Immutable text data

### ArrayList
- Dynamic lists that grow/shrink
- Collections with frequent random access
- Storing results from queries
- General-purpose lists

### LinkedList
- Implementing queues and deques
- Undo/redo functionality (when combined with stacks)
- LRU cache implementation
- Frequent insertions/deletions at both ends

### HashMap
- Caching data
- Counting frequencies (word count, character count)
- Storing configuration key-value pairs
- Database result mapping
- Implementing graphs (adjacency list)

### HashSet
- Removing duplicates
- Membership testing
- Finding unique elements
- Set operations (union, intersection, difference)

### TreeSet
- Maintaining sorted unique elements
- Range queries (elements between x and y)
- Finding closest elements (floor, ceiling)
- Leaderboards and rankings

### Stack
- Expression evaluation (postfix, infix)
- Balanced parentheses checking
- Undo/redo operations
- Browser history (back/forward)
- Depth-First Search (DFS)
- Function call stack simulation

### Queue
- Breadth-First Search (BFS)
- Task scheduling
- Order processing systems
- Print queue management
- Request handling in web servers

### PriorityQueue
- Task scheduling with priorities
- Finding k largest/smallest elements
- Dijkstra's algorithm
- Huffman coding
- Merge k sorted lists

### Trees
- File system hierarchy
- Organization structures
- Expression trees
- Database indexing (B-trees, B+ trees)
- Decision trees
- Routing tables

---

## 📖 How to Use This Guide

1. **Start with the Master Guide** (this document) to understand available data structures
2. **Select the appropriate data structure** based on your requirements using the Quick Selection Guide
3. **Navigate to the specific guide** by clicking on the data structure name
4. **Reference the Quick Reference Tables** in each guide for method signatures and examples
5. **Study the code examples** to understand implementation patterns
6. **Practice with common patterns** provided in each guide

---

## 💡 Best Practices

### 1. Choose the Right Data Structure
```java
// ❌ Bad: Using ArrayList for frequent insertions at beginning
List<String> list = new ArrayList<>();
for (int i = 0; i < 10000; i++) {
    list.add(0, "Item"); // O(n) each time!
}

// ✅ Good: Using LinkedList for frequent insertions at beginning
List<String> list = new LinkedList<>();
for (int i = 0; i < 10000; i++) {
    list.add(0, "Item"); // O(1) each time!
}
```

### 2. Use Appropriate Interfaces
```java
// ✅ Good: Program to interfaces
List<String> list = new ArrayList<>();
Set<Integer> set = new HashSet<>();
Map<String, Integer> map = new HashMap<>();
Queue<String> queue = new LinkedList<>();

// ❌ Avoid: Programming to concrete implementations
ArrayList<String> list = new ArrayList<>();
HashSet<Integer> set = new HashSet<>();
```

### 3. Consider Thread Safety
```java
// Thread-safe alternatives when needed
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
Set<Integer> syncSet = Collections.synchronizedSet(new HashSet<>());
Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
Queue<String> concurrentQueue = new ConcurrentLinkedQueue<>();
```

### 4. Prefer Modern Alternatives
```java
// ❌ Legacy (slower, synchronized)
Vector<String> vector = new Vector<>();
Hashtable<String, Integer> hashtable = new Hashtable<>();
Stack<String> stack = new Stack<>();

// ✅ Modern (faster, not synchronized by default)
List<String> list = new ArrayList<>();
Map<String, Integer> map = new HashMap<>();
Deque<String> stack = new ArrayDeque<>();
```

### 5. Use Generics for Type Safety
```java
// ❌ Raw types (no type safety)
List list = new ArrayList();
list.add("String");
list.add(123); // Runtime error risk

// ✅ Generics (compile-time type safety)
List<String> list = new ArrayList<>();
list.add("String");
// list.add(123); // Compile-time error
```

---

## 🚀 Performance Tips

### 1. Initialize with Capacity When Size is Known
```java
// Better performance when size is known
List<String> list = new ArrayList<>(1000);
Map<String, Integer> map = new HashMap<>(1000);
Set<Integer> set = new HashSet<>(1000);
```

### 2. Use Primitive Streams for Better Performance
```java
// ✅ Primitive streams (no boxing overhead)
int sum = IntStream.range(0, 1000).sum();

// ❌ Object streams (boxing overhead)
int sum = Stream.iterate(0, i -> i + 1).limit(1000)
.mapToInt(Integer::intValue).sum();
```

### 3. Choose Appropriate Collection for Use Case
```java
// For frequent contains() checks
Set<String> set = new HashSet<>(); // O(1)
// Not: List<String> list = new ArrayList<>(); // O(n)

// For maintaining insertion order
Map<String, Integer> map = new LinkedHashMap<>();
// Not always: HashMap (no order guarantee)

// For sorted iteration
Set<Integer> set = new TreeSet<>();
// Not always: HashSet (no order)
```

---

## 📝 Learning Path

### Beginner Level
1. Start with [Arrays](Java-Array-Operations-Guide.md) - Foundation of all data structures
2. Learn [String](Java-String-Operations-Guide.md) - Essential for text processing
3. Master [ArrayList](Java-ArrayList-Operations-Guide.md) - Most commonly used collection

### Intermediate Level
4. Understand [HashMap](Java-HashMap-Operations-Guide.md) - Key-value pair operations
5. Learn [HashSet](Java-HashSet-Operations-Guide.md) - Unique element collections
6. Study [Queue](Java-Queue-Operations-Guide.md) and [Stack](Java-Stack-Operations-Guide.md) - Essential for algorithms

### Advanced Level
7. Master [LinkedList](Java-LinkedList-Operations-Guide.md) - Complex list operations
8. Learn [TreeSet](Java-TreeSet-Operations-Guide.md) - Sorted collections and navigation
9. Study [Trees](Java-Trees-Operations-Guide.md) - Hierarchical data and advanced algorithms

---

## 🎓 Additional Resources

### Collections Framework Hierarchy
```
Collection (Interface)
├── List (Interface)
│ ├── ArrayList (Class)
│ └── LinkedList (Class)
├── Set (Interface)
│ ├── HashSet (Class)
│ └── TreeSet (Class)
└── Queue (Interface)
├── LinkedList (Class)
├── ArrayDeque (Class)
└── PriorityQueue (Class)

Map (Interface)
├── HashMap (Class)
└── TreeMap (Class)
```

### Import Statements Reference
```java
// Lists
import java.util.ArrayList;
import java.util.LinkedList;

// Sets
import java.util.HashSet;
import java.util.TreeSet;

// Maps
import java.util.HashMap;
import java.util.TreeMap;
import java.util.LinkedHashMap;

// Queues and Stacks
import java.util.Queue;
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.PriorityQueue;
import java.util.Stack; // Legacy

// Utilities
import java.util.Collections;
import java.util.Arrays;
import java.util.Comparator;
```

---

## 🔗 Quick Links

- [Array Operations Guide →](Java-Array-Operations.md)
- [String Operations Guide →](Java-String-Operations.md)
- [HashMap Operations Guide →](Java-HashMap-Operations.md)
- [HashSet Operations Guide →](Java-HashSet-Operations.md)
- [ArrayList Operations Guide →](Java-ArrayList-Operations.md)
- [TreeSet Operations Guide →](Java-TreeSet-Operations.md)
- [LinkedList Operations Guide →](Java-LinkedList-Operations.md)
- [Stack Operations Guide →](Java-Stack-Operations.md)
- [Queue Operations Guide →](Java-Queue-Operations.md)
- [Trees Operations Guide →](Java-Trees-Operations.md)

---

## 📊 Summary Table

| Data Structure | Ordered | Unique | Key-Value | Thread-Safe | Null Allowed | Main Use Case |
|---------------|---------|--------|-----------|-------------|--------------|---------------|
| **Array** | ✅ | ❌ | ❌ | ❌ | ✅ | Fixed-size random access |
| **String** | ✅ | ❌ | ❌ | ✅ (Immutable) | ❌ | Text processing |
| **ArrayList** | ✅ | ❌ | ❌ | ❌ | ✅ | Dynamic random access |
| **LinkedList** | ✅ | ❌ | ❌ | ❌ | ✅ | Frequent insertions at ends |
| **HashMap** | ❌ | ✅ Keys | ✅ | ❌ | ✅ (one null key) | Fast key-value lookup |
| **HashSet** | ❌ | ✅ | ❌ | ❌ | ✅ (one null) | Fast membership test |
| **TreeSet** | ✅ (Sorted) | ✅ | ❌ | ❌ | ❌ | Sorted unique elements |
| **Stack** | ✅ (LIFO) | ❌ | ❌ | ✅ (Legacy) | ✅ | LIFO operations |
| **Queue** | ✅ (FIFO) | ❌ | ❌ | Varies | Varies | FIFO operations |
| **PriorityQueue** | ✅ (Priority) | ❌ | ❌ | ❌ | ❌ | Priority-based processing |
| **Trees** | ✅ (Hierarchical) | Varies | ❌ | ❌ | ✅ | Hierarchical data |

---

**Happy Coding! 🚀**

*Last Updated: January 2, 2026*
