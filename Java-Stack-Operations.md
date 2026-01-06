# Java Stack Operations Guide

A comprehensive guide to working with Stack in Java, covering LIFO operations, legacy Stack class, and modern alternatives.

## Table of Contents
1. [Stack Basics](#stack-basics)
2. [Stack Creation](#stack-creation)
3. [Push and Pop](#push-and-pop)
4. [Peek Operations](#peek-operations)
5. [Search Operations](#search-operations)
6. [Checking Stack State](#checking-stack-state)
7. [Modern Alternatives](#modern-alternatives)
8. [Common Patterns](#common-patterns)

---

## Stack Basics

### What is a Stack?
```java
import java.util.Stack;

// Stack is a LIFO (Last In, First Out) data structure
// - Legacy class (extends Vector)
// - Thread-safe but slower
// - Use Deque implementations for better performance
// - push(), pop(), peek() are main operations

Stack<String> stack = new Stack<>();
```

### LIFO Principle
```java
Stack<Integer> stack = new Stack<>();

stack.push(1); // [1]
stack.push(2); // [1, 2]
stack.push(3); // [1, 2, 3]

System.out.println(stack.pop()); // 3 (last in, first out)
System.out.println(stack.pop()); // 2
System.out.println(stack.pop()); // 1
```

---

## Stack Creation

### Basic Creation
```java
import java.util.Stack;

// Create empty stack
Stack<String> stack1 = new Stack<>();

// Create and initialize
Stack<Integer> stack2 = new Stack<>();
stack2.push(1);
stack2.push(2);
stack2.push(3);
```

### From Collection
```java
// Not directly supported, need to push elements
Stack<String> stack = new Stack<>();
List<String> list = Arrays.asList("A", "B", "C");
list.forEach(stack::push);
// stack = [A, B, C] (C is on top)
```

---

## Push and Pop

### Push (Add to Top)
```java
Stack<String> stack = new Stack<>();

// Push elements (returns the pushed item)
String item1 = stack.push("First"); // Returns "First"
String item2 = stack.push("Second"); // Returns "Second"
String item3 = stack.push("Third"); // Returns "Third"

// Stack state: [First, Second, Third]
// Top: Third
```

### Pop (Remove from Top)
```java
Stack<String> stack = new Stack<>();
stack.push("First");
stack.push("Second");
stack.push("Third");

// Pop removes and returns top element
String popped1 = stack.pop(); // "Third"
String popped2 = stack.pop(); // "Second"
String popped3 = stack.pop(); // "First"

// Empty stack
// String error = stack.pop(); // Throws EmptyStackException
```

---

## Peek Operations

### Peek (View Top Without Removing)
```java
Stack<String> stack = new Stack<>();
stack.push("First");
stack.push("Second");
stack.push("Third");

// Peek returns top element without removing
String top = stack.peek(); // "Third"
System.out.println(stack.size()); // Still 3

// Peek again
String stillTop = stack.peek(); // Still "Third"

// Empty stack
Stack<String> empty = new Stack<>();
// String error = empty.peek(); // Throws EmptyStackException
```

---

## Search Operations

### Search (Find Position from Top)
```java
Stack<String> stack = new Stack<>();
stack.push("A"); // Bottom
stack.push("B");
stack.push("C");
stack.push("D"); // Top

// Search returns position from top (1-based)
int pos1 = stack.search("D"); // 1 (top)
int pos2 = stack.search("C"); // 2
int pos3 = stack.search("B"); // 3
int pos4 = stack.search("A"); // 4 (bottom)
int notFound = stack.search("E"); // -1 (not found)
```

---

## Checking Stack State

### Empty Check
```java
Stack<String> stack = new Stack<>();

// Check if empty
boolean isEmpty1 = stack.empty(); // true

stack.push("Item");
boolean isEmpty2 = stack.empty(); // false
```

### Size
```java
Stack<Integer> stack = new Stack<>();
stack.push(1);
stack.push(2);
stack.push(3);

int size = stack.size(); // 3
```

---

## Modern Alternatives

### Using ArrayDeque (Recommended)
```java
import java.util.ArrayDeque;
import java.util.Deque;

// ArrayDeque is faster and more memory efficient
Deque<String> stack = new ArrayDeque<>();

// Push (addFirst or push)
stack.push("First");
stack.push("Second");
stack.push("Third");

// Pop (removeFirst or pop)
String popped = stack.pop(); // "Third"

// Peek (peekFirst or peek)
String top = stack.peek(); // "Second"

// Check empty
boolean isEmpty = stack.isEmpty();

// Size
int size = stack.size();
```

### Using LinkedList as Stack
```java
import java.util.LinkedList;
import java.util.Deque;

// LinkedList implements Deque
Deque<String> stack = new LinkedList<>();

stack.push("First");
stack.push("Second");
String popped = stack.pop();
String top = stack.peek();
```

### Comparison: Stack vs Deque
```java
// ❌ Legacy Stack (slower, synchronized)
Stack<String> legacyStack = new Stack<>();
legacyStack.push("Item");
legacyStack.pop();

// ✅ Modern Deque (faster, not synchronized)
Deque<String> modernStack = new ArrayDeque<>();
modernStack.push("Item");
modernStack.pop();

// ✅ LinkedList (good for frequent modifications)
Deque<String> linkedStack = new LinkedList<>();
linkedStack.push("Item");
linkedStack.pop();
```

---

## Common Patterns

### Reverse String
```java
public static String reverseString(String str) {
    Stack<Character> stack = new Stack<>();
    // Push all characters
    for (char c : str.toCharArray()) {
        stack.push(c);
    }
// Pop to build reversed string
StringBuilder reversed = new StringBuilder();
while (!stack.empty()) {
    reversed.append(stack.pop());
}
return reversed.toString();
}

// Usage
String result = reverseString("Hello"); // "olleH"
```

### Balanced Parentheses
```java
public static boolean isBalanced(String expression) {
    Stack<Character> stack = new Stack<>();
    for (char c : expression.toCharArray()) {
        // Push opening brackets
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);
        }
    // Check closing brackets
    else if (c == ')' || c == ']' || c == '}') {
        if (stack.empty()) return false;
        char top = stack.pop();
        if ((c == ')' && top != '(') ||
        (c == ']' && top != '[') ||
        (c == '}' && top != '{')) {
            return false;
        }
}
}
return stack.empty(); // All brackets matched
}

// Usage
boolean valid1 = isBalanced("({[]})"); // true
boolean valid2 = isBalanced("({[})"); // false
```

### Evaluate Postfix Expression
```java
public static int evaluatePostfix(String expression) {
    Stack<Integer> stack = new Stack<>();
    for (String token : expression.split(" ")) {
        if (token.matches("\\d+")) {
            // Number: push to stack
            stack.push(Integer.parseInt(token));
        } else {
        // Operator: pop two operands
        int operand2 = stack.pop();
        int operand1 = stack.pop();
        switch (token) {
            case "+": stack.push(operand1 + operand2); break;
            case "-": stack.push(operand1 - operand2); break;
            case "*": stack.push(operand1 * operand2); break;
            case "/": stack.push(operand1 / operand2); break;
        }
}
}
return stack.pop();
}

// Usage
int result = evaluatePostfix("5 3 + 2 *"); // (5+3)*2 = 16
```

### Undo/Redo Functionality
```java
class TextEditor {
    private Stack<String> undoStack = new Stack<>();
    private Stack<String> redoStack = new Stack<>();
    private String currentText = "";
    public void write(String text) {
        undoStack.push(currentText);
        currentText += text;
        redoStack.clear(); // Clear redo stack on new action
    }
public void undo() {
    if (!undoStack.empty()) {
        redoStack.push(currentText);
        currentText = undoStack.pop();
    }
}
public void redo() {
    if (!redoStack.empty()) {
        undoStack.push(currentText);
        currentText = redoStack.pop();
    }
}
public String getText() {
    return currentText;
}
}

// Usage
TextEditor editor = new TextEditor();
editor.write("Hello");
editor.write(" World");
editor.undo(); // "Hello"
editor.redo(); // "Hello World"
```

### Browser History
```java
class BrowserHistory {
    private Stack<String> backStack = new Stack<>();
    private Stack<String> forwardStack = new Stack<>();
    private String currentPage = "home";
    public void visit(String url) {
        backStack.push(currentPage);
        currentPage = url;
        forwardStack.clear(); // Clear forward on new visit
    }
public void back() {
    if (!backStack.empty()) {
        forwardStack.push(currentPage);
        currentPage = backStack.pop();
    }
}
public void forward() {
    if (!forwardStack.empty()) {
        backStack.push(currentPage);
        currentPage = forwardStack.pop();
    }
}
public String getCurrentPage() {
    return currentPage;
}
}
```

### Depth-First Search (DFS)
```java
public static void dfs(TreeNode root) {
    if (root == null) return;
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.empty()) {
        TreeNode node = stack.pop();
        System.out.println(node.val);
        // Push right first (so left is processed first)
        if (node.right != null) stack.push(node.right);
        if (node.left != null) stack.push(node.left);
    }
}
```

---

## Best Practices

### 1. Use Deque Instead of Stack
```java
// ❌ Legacy Stack (not recommended)
Stack<String> stack = new Stack<>();

// ✅ Modern Deque (recommended)
Deque<String> stack = new ArrayDeque<>();
```

### 2. Check Empty Before Pop/Peek
```java
Stack<Integer> stack = new Stack<>();

// ❌ Can throw EmptyStackException
// int value = stack.pop();

// ✅ Safe check
if (!stack.empty()) {
    int value = stack.pop();
}

// ✅ Or use Deque with null-safe methods
Deque<Integer> deque = new ArrayDeque<>();
Integer value = deque.poll(); // Returns null if empty
```

### 3. Consider Thread Safety
```java
// Stack is synchronized (thread-safe but slower)
Stack<String> threadSafe = new Stack<>();

// ArrayDeque is not synchronized (faster)
Deque<String> notThreadSafe = new ArrayDeque<>();

// Need thread-safe Deque?
Deque<String> concurrent = new ConcurrentLinkedDeque<>();
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `stack.push(element)` | Add element to top of stack | `E` - returns pushed element | `String item = stack.push("Hello");` |
| `stack.pop()` | Remove and return top element | `E` - top element, throws if empty | `String top = stack.pop();` |
| `stack.peek()` | View top element without removing | `E` - top element, throws if empty | `String top = stack.peek();` |
| `stack.search(object)` | Find position from top (1-based) | `int` - position if found, -1 if not found | `int pos = stack.search("Item");` |
| `stack.empty()` | Check if stack is empty | `boolean` - true if empty, false otherwise | `boolean isEmpty = stack.empty();` |
| `stack.size()` | Get number of elements | `int` - count of elements | `int size = stack.size();` |
| `stack.clear()` | Remove all elements | `void` - empties the stack | `stack.clear();` |
| `stack.contains(object)` | Check if element exists | `boolean` - true if present, false otherwise | `boolean has = stack.contains("Item");` |
| `stack.get(index)` | Get element at index (0-based) | `E` - element at index | `String item = stack.get(0);` |
| `stack.set(index, element)` | Replace element at index | `E` - previous element at index | `String old = stack.set(0, "New");` |
| `stack.add(element)` | Add to top (same as push) | `boolean` - always true | `stack.add("Item");` |
| `stack.remove(object)` | Remove first occurrence | `boolean` - true if removed, false otherwise | `boolean removed = stack.remove("Item");` |
| `stack.remove(index)` | Remove element at index | `E` - removed element | `String removed = stack.remove(0);` |
| `stack.iterator()` | Get iterator from bottom to top | `Iterator<E>` - iterates from bottom | `Iterator<String> it = stack.iterator();` |
| `stack.forEach(action)` | Perform action for each element | `void` - iterates from bottom to top | `stack.forEach(System.out::println);` |
| **Deque Methods (Recommended Alternative)** ||||
| `deque.push(element)` | Add to top (front) | `void` - efficient O(1) operation | `deque.push("Item");` |
| `deque.pop()` | Remove from top (front) | `E` - top element, throws if empty | `String top = deque.pop();` |
| `deque.peek()` | View top without removing | `E` - top element or null if empty | `String top = deque.peek();` |
| `deque.poll()` | Remove from top | `E` - top element or null if empty | `String top = deque.poll();` |
| `deque.isEmpty()` | Check if empty | `boolean` - true if empty, false otherwise | `boolean empty = deque.isEmpty();` |
| `deque.size()` | Get number of elements | `int` - count of elements | `int size = deque.size();` |
