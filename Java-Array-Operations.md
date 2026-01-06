# Java Array Operations Guide

A comprehensive guide to working with arrays in Java, covering declaration, initialization, manipulation, and common operations.

## Table of Contents

1. [Declaration and Initialization](#declaration-and-initialization)
2. [Array Size](#array-size)
3. [Accessing Elements](#accessing-elements)
4. [Loops and Iteration](#loops-and-iteration)
5. [Sorting](#sorting)
6. [Searching](#searching)
7. [Copying Arrays](#copying-arrays)
8. [Filling and Replacing](#filling-and-replacing)
9. [Multidimensional Arrays](#multidimensional-arrays)
10. [Common Utilities](#common-utilities)

---

## Declaration and Initialization

### Declaration

```java
// Syntax 1: Preferred
int[] numbers;
String[] names;

// Syntax 2: Also valid but less common
int numbers[];
String names[];
```

### Initialization

#### Static Initialization

```java
// Initialize with values
int[] numbers = {1, 2, 3, 4, 5};
String[] fruits = {"Apple", "Banana", "Orange"};

// Empty initialization with size
int[] numbers = new int[5]; // Creates array of size 5 with default values (0)
String[] names = new String[3]; // Creates array with null values
```

#### Dynamic Initialization

```java
int size = 10;
int[] dynamicArray = new int[size];

// Initialize with values in a loop
for (int i = 0; i < dynamicArray.length; i++) {
    dynamicArray[i] = i * 2;
}
```

---

## Array Size

### Getting Length

```java
int[] numbers = {1, 2, 3, 4, 5};
int size = numbers.length; // Returns 5

// Note: length is a property, not a method (no parentheses)
System.out.println("Array size: " + numbers.length);
```

### Note on Fixed Size

```java
// Arrays in Java have fixed size once created
int[] arr = new int[5];
// arr.length is always 5
// Cannot be changed after creation
```

---

## Accessing Elements

### Get and Set Elements

```java
int[] numbers = {10, 20, 30, 40, 50};

// Access by index (0-based)
int firstElement = numbers[0];  // 10
int lastElement = numbers[numbers.length - 1]; // 50

// Modify element
numbers[2] = 100; // Changes 30 to 100
```

---

## Loops and Iteration

### For Loop

```java
int[] numbers = {1, 2, 3, 4, 5};

// Traditional for loop
for (int i = 0; i < numbers.length; i++) {
    System.out.println("Index " + i + ": " + numbers[i]);
}
```

### Enhanced For Loop (For-Each)

```java
int[] numbers = {1, 2, 3, 4, 5};

// For-each loop (no index access)
for (int num : numbers) {
    System.out.println(num);
}
```

### While Loop

```java
int[] numbers = {1, 2, 3, 4, 5};
int i = 0;

while (i < numbers.length) {
    System.out.println(numbers[i]);
    i++;
}
```

### Using Streams (Java 8+)

```java
import java.util.Arrays;
import java.util.stream.IntStream;

int[] numbers = {1, 2, 3, 4, 5};

// Iterate with streams
Arrays.stream(numbers).forEach(num -> System.out.println(num));

// With index (using IntStream)
IntStream.range(0, numbers.length)
.forEach(i -> System.out.println("Index " + i + ": " + numbers[i]));
```

---

## Sorting

### Basic Sorting

#### Ascending Order

```java
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 9};
Arrays.sort(numbers); // Sorts in ascending order
// Result: [1, 2, 5, 8, 9]

String[] names = {"Charlie", "Alice", "Bob"};
Arrays.sort(names);
// Result: ["Alice", "Bob", "Charlie"]
```

#### Descending Order

```java
import java.util.Arrays;
import java.util.Collections;

// For primitive arrays - use wrapper classes
Integer[] numbers = {5, 2, 8, 1, 9};
Arrays.sort(numbers, Collections.reverseOrder());
// Result: [9, 8, 5, 2, 1]

// For primitives, manual reversal needed
int[] primitiveNums = {5, 2, 8, 1, 9};
Arrays.sort(primitiveNums);
// Then reverse manually
for (int i = 0; i < primitiveNums.length / 2; i++) {
    int temp = primitiveNums[i];
    primitiveNums[i] = primitiveNums[primitiveNums.length - 1 - i];
    primitiveNums[primitiveNums.length - 1 - i] = temp;
}
```

### Sorting with Custom Comparator

#### Sort Objects by Property

```java
import java.util.Arrays;
import java.util.Comparator;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

Person[] people = {
    new Person("Alice", 30),
    new Person("Bob", 25),
    new Person("Charlie", 35)
};

// Sort by age (ascending)
Arrays.sort(people, Comparator.comparingInt(p -> p.age));

// Sort by age (descending)
Arrays.sort(people, (p1, p2) -> p2.age - p1.age);

// Sort by name
Arrays.sort(people, Comparator.comparing(p -> p.name));
```

#### Comparator Methods

##### 1. Basic Comparator Implementation

```java
import java.util.Comparator;

// Implementing Comparator interface
class AgeComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.age, p2.age);
    }
}

// Usage
Arrays.sort(people, new AgeComparator());
```

##### 2. Lambda Expression Comparators

```java
// Ascending order by age
Arrays.sort(people, (p1, p2) -> Integer.compare(p1.age, p2.age));

// Descending order by age
Arrays.sort(people, (p1, p2) -> Integer.compare(p2.age, p1.age));

// By name (alphabetical)
Arrays.sort(people, (p1, p2) -> p1.name.compareTo(p2.name));

// By name (reverse alphabetical)
Arrays.sort(people, (p1, p2) -> p2.name.compareTo(p1.name));
```

##### 3. Comparator Factory Methods

```java
import java.util.Comparator;

// comparingInt() - for int properties
Arrays.sort(people, Comparator.comparingInt(p -> p.age));

// comparingLong() - for long properties
Arrays.sort(items, Comparator.comparingLong(item -> item.timestamp));

// comparingDouble() - for double properties
Arrays.sort(products, Comparator.comparingDouble(p -> p.price));

// comparing() - for any Comparable type
Arrays.sort(people, Comparator.comparing(p -> p.name));
```

##### 4. Reversed Comparators

```java
// Using reversed() method
Arrays.sort(people, Comparator.comparingInt(p -> p.age).reversed());

// Using reverseOrder()
Arrays.sort(numbers, Collections.reverseOrder());

// Natural order reversed
Arrays.sort(people, Comparator.reverseOrder());
```

##### 5. Chained Comparators (Multiple Criteria)

```java
// Sort by age first, then by name
Arrays.sort(people,
Comparator.comparingInt((Person p) -> p.age)
.thenComparing(p -> p.name)
);

// Sort by age descending, then by name ascending
Arrays.sort(people,
Comparator.comparingInt((Person p) -> p.age).reversed()
.thenComparing(p -> p.name)
);

// Multiple levels of sorting
Arrays.sort(employees,
Comparator.comparing((Employee e) -> e.department)
.thenComparingInt(e -> e.salary)
.thenComparing(e -> e.name)
);
```

##### 6. Null-Safe Comparators

```java
// Handle null values (nulls first)
Arrays.sort(people,
Comparator.nullsFirst(Comparator.comparing(p -> p.name))
);

// Handle null values (nulls last)
Arrays.sort(people,
Comparator.nullsLast(Comparator.comparingInt(p -> p.age))
);

// Null-safe property access
Arrays.sort(people,
Comparator.comparing(
p -> p.name,
Comparator.nullsFirst(String::compareTo)
)
);
```

##### 7. Custom Comparison Logic

```java
// Even numbers before odd numbers
Comparator<Integer> evenFirstComparator = (a, b) -> {
    boolean aEven = a % 2 == 0;
    boolean bEven = b % 2 == 0;

    if (aEven && !bEven) return -1;
    if (!aEven && bEven) return 1;
    return Integer.compare(a, b);
};
Arrays.sort(numbers, evenFirstComparator);

// Sort strings by length, then alphabetically
Comparator<String> lengthThenAlpha =
Comparator.comparingInt(String::length)
.thenComparing(String::compareTo);
Arrays.sort(words, lengthThenAlpha);

// Sort by distance from target
int target = 10;
Comparator<Integer> closestToTarget =
Comparator.comparingInt(n -> Math.abs(n - target));
Arrays.sort(numbers, closestToTarget);
```

#### Complex Sorting Conditions

```java
import java.util.Arrays;
import java.util.Comparator;

Integer[] numbers = {5, 2, 8, 1, 9, 3};

// Sort even numbers first, then odd
Arrays.sort(numbers, (a, b) -> {
    if (a % 2 == 0 && b % 2 != 0) return -1;
    if (a % 2 != 0 && b % 2 == 0) return 1;
    return a - b;
});

// Sort by absolute distance from a target
int target = 5;
Arrays.sort(numbers, Comparator.comparingInt(n -> Math.abs(n - target)));

// Priority-based sorting (positive, zero, negative)
Arrays.sort(numbers, (a, b) -> {
    if (a > 0 && b <= 0) return -1;
    if (a <= 0 && b > 0) return 1;
    return Integer.compare(a, b);
});
```

### Partial Sorting

```java
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 9, 3, 7};

// Sort only a portion of the array (from index 1 to 5)
Arrays.sort(numbers, 1, 5);
// Result: [5, 1, 2, 8, 9, 3, 7]
//          ^ unsorted, sorted range ^, unsorted
```

---

## Searching

### Linear Search

```java
public static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i; // Return index
        }
}
return -1; // Not found
}

// Usage
int[] numbers = {5, 2, 8, 1, 9};
int index = linearSearch(numbers, 8); // Returns 2
```

### Binary Search (Requires Sorted Array)

```java
import java.util.Arrays;

int[] numbers = {1, 2, 5, 8, 9};
int index = Arrays.binarySearch(numbers, 5); // Returns 2

// If not found, returns (-(insertion point) - 1)
int notFound = Arrays.binarySearch(numbers, 7); // Returns negative value
```

### Contains Check

```java
import java.util.Arrays;

int[] numbers = {1, 2, 5, 8, 9};

// Using streams
boolean contains = Arrays.stream(numbers)
.anyMatch(n -> n == 5); // true
```

### Find All Elements (Filter)

```java
import java.util.Arrays;

int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9};

// Find all even numbers using streams
int[] evens = Arrays.stream(numbers)
.filter(n -> n % 2 == 0)
.toArray();
// Result: [2, 4, 6, 8]

// Find all numbers greater than 5
int[] greaterThan5 = Arrays.stream(numbers)
.filter(n -> n > 5)
.toArray();
// Result: [6, 7, 8, 9]

// Find all matching with custom condition
int[] divisibleBy3 = Arrays.stream(numbers)
.filter(n -> n % 3 == 0)
.toArray();
// Result: [3, 6, 9]
```

### Find All Indices

```java
import java.util.Arrays;
import java.util.stream.IntStream;

int[] numbers = {5, 2, 8, 2, 9, 2, 3};

// Find all indices of a target value
int target = 2;
int[] indices = IntStream.range(0, numbers.length)
.filter(i -> numbers[i] == target)
.toArray();
// Result: [1, 3, 5]

// Find indices where condition is true
int[] evenIndices = IntStream.range(0, numbers.length)
.filter(i -> numbers[i] % 2 == 0)
.toArray();
// Result: [1, 2, 3, 5]
```

### Find First/Last Element

```java
import java.util.Arrays;
import java.util.OptionalInt;

int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9};

// Find first element matching condition
OptionalInt first = Arrays.stream(numbers)
.filter(n -> n > 5)
.findFirst();
int firstValue = first.orElse(-1); // Returns 6

// Find last element matching condition
int last = IntStream.range(0, numbers.length)
.map(i -> numbers.length - 1 - i)
.map(i -> numbers[i])
.filter(n -> n > 5)
.findFirst()
.orElse(-1); // Returns 9

// Using manual loop for better performance
int lastMatch = -1;
for (int i = numbers.length - 1; i >= 0; i--) {
    if (numbers[i] > 5) {
        lastMatch = numbers[i];
        break;
    }
}
```

### Count Matching Elements

```java
import java.util.Arrays;

int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9};

// Count elements matching condition
long count = Arrays.stream(numbers)
.filter(n -> n % 2 == 0)
.count();
// Result: 4
```

---

## Copying Arrays

### Using Arrays.copyOf()

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};

// Copy entire array
int[] copy = Arrays.copyOf(original, original.length);

// Copy with different size
int[] larger = Arrays.copyOf(original, 10); // Pads with 0s
int[] smaller = Arrays.copyOf(original, 3);  // [1, 2, 3]
```

### Using Arrays.copyOfRange()

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};

// Copy a range (from index 1 to 4, exclusive end)
int[] range = Arrays.copyOfRange(original, 1, 4); // [2, 3, 4]
```

### Using System.arraycopy()

```java
int[] source = {1, 2, 3, 4, 5};
int[] destination = new int[5];

// System.arraycopy(src, srcPos, dest, destPos, length)
System.arraycopy(source, 0, destination, 0, source.length);
// destination is now {1, 2, 3, 4, 5}

// Copy partial array
int[] partial = new int[3];
System.arraycopy(source, 1, partial, 0, 3);
// partial is now {2, 3, 4}
```

### Using clone()

```java
int[] original = {1, 2, 3, 4, 5};
int[] copy = original.clone();
```

### Deep Copy vs Shallow Copy

```java
// For primitive arrays - all methods create deep copies
int[] primitives = {1, 2, 3};
int[] primitiveCopy = primitives.clone(); // Deep copy

// For object arrays - most methods create shallow copies
String[] objects = {"A", "B", "C"};
String[] objectCopy = objects.clone(); // Shallow copy (references copied)

// For deep copy of object arrays
Person[] people = {
    new Person("Alice", 30),
    new Person("Bob", 25)
};
Person[] deepCopy = new Person[people.length];
for (int i = 0; i < people.length; i++) {
    deepCopy[i] = new Person(people[i].name, people[i].age);
}
```

---

## Filling and Replacing

### Fill Array with Value

```java
import java.util.Arrays;

int[] numbers = new int[5];

// Fill entire array with value
Arrays.fill(numbers, 10); // [10, 10, 10, 10, 10]

// Fill a range
Arrays.fill(numbers, 1, 4, 20); // [10, 20, 20, 20, 10]
```

### Replace Elements

```java
import java.util.Arrays;

// Replace using loops
int[] numbers = {1, 2, 3, 2, 4, 2};
for (int i = 0; i < numbers.length; i++) {
    if (numbers[i] == 2) {
        numbers[i] = 20; // Replace all 2s with 20
    }
}

// Replace using streams (creates new array)
int[] replaced = Arrays.stream(numbers)
.map(n -> n == 2 ? 20 : n)
.toArray();
```

### Set All Elements

```java
import java.util.Arrays;

int[] numbers = new int[5];

// Using setAll (Java 8+)
Arrays.setAll(numbers, i -> i * 2); // [0, 2, 4, 6, 8]

// Using parallelSetAll for better performance on large arrays
Arrays.parallelSetAll(numbers, i -> i * i); // [0, 1, 4, 9, 16]
```

---

## Multidimensional Arrays

### 2D Arrays

#### Declaration and Initialization

```java
// Declaration
int[][] matrix;

// Static initialization
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Dynamic initialization
int[][] matrix = new int[3][3]; // 3x3 matrix

// Jagged arrays (variable column sizes)
int[][] jagged = new int[3][];
jagged[0] = new int[2];
jagged[1] = new int[4];
jagged[2] = new int[3];
```

#### Accessing Elements

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

int element = matrix[1][2]; // 6 (row 1, column 2)
matrix[0][1] = 20; // Change element
```

#### Iterating

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Nested for loop
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
System.out.println();
}

// Enhanced for loop
for (int[] row : matrix) {
    for (int value : row) {
        System.out.print(value + " ");
    }
System.out.println();
}
```

### 3D Arrays

```java
// 3D array (like a cube)
int[][][] cube = new int[3][3][3];

// Access element
cube[0][1][2] = 5;

// Iterate
for (int i = 0; i < cube.length; i++) {
    for (int j = 0; j < cube[i].length; j++) {
        for (int k = 0; k < cube[i][j].length; k++) {
            System.out.println(cube[i][j][k]);
        }
}
}
```

---

## Common Utilities

### Convert Array to String

```java
import java.util.Arrays;

int[] numbers = {1, 2, 3, 4, 5};
String str = Arrays.toString(numbers); // "[1, 2, 3, 4, 5]"

// For 2D arrays
int[][] matrix = {{1, 2}, {3, 4}};
String matrixStr = Arrays.deepToString(matrix); // "[[1, 2], [3, 4]]"
```

### Compare Arrays

```java
import java.util.Arrays;

int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
int[] arr3 = {1, 2, 4};

// Equality check
boolean equal = Arrays.equals(arr1, arr2); // true
boolean notEqual = Arrays.equals(arr1, arr3); // false

// For multidimensional arrays
int[][] matrix1 = {{1, 2}, {3, 4}};
int[][] matrix2 = {{1, 2}, {3, 4}};
boolean deepEqual = Arrays.deepEquals(matrix1, matrix2); // true
```

### Convert to List

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

// For object arrays
String[] array = {"A", "B", "C"};
List<String> list = Arrays.asList(array);

// For primitive arrays - need to box first
int[] primitives = {1, 2, 3};
List<Integer> intList = Arrays.stream(primitives)
.boxed()
.collect(Collectors.toList());
```

### Hash Code

```java
import java.util.Arrays;

int[] arr = {1, 2, 3};
int hash = Arrays.hashCode(arr);

// For multidimensional
int[][] matrix = {{1, 2}, {3, 4}};
int deepHash = Arrays.deepHashCode(matrix);
```

### Stream Operations

```java
import java.util.Arrays;
import java.util.stream.IntStream;

int[] numbers = {1, 2, 3, 4, 5};

// Sum
int sum = Arrays.stream(numbers).sum(); // 15

// Average
double avg = Arrays.stream(numbers).average().orElse(0.0); // 3.0

// Max/Min
int max = Arrays.stream(numbers).max().orElse(0); // 5
int min = Arrays.stream(numbers).min().orElse(0); // 1

// Filter
int[] evens = Arrays.stream(numbers)
.filter(n -> n % 2 == 0)
.toArray(); // [2, 4]

// Map
int[] doubled = Arrays.stream(numbers)
.map(n -> n * 2)
.toArray(); // [2, 4, 6, 8, 10]
```

### Parallel Operations (Java 8+)

```java
import java.util.Arrays;

int[] largeArray = new int[1000000];

// Parallel sort (faster for large arrays)
Arrays.parallelSort(largeArray);

// Parallel prefix (cumulative operation)
int[] nums = {1, 2, 3, 4, 5};
Arrays.parallelPrefix(nums, (a, b) -> a + b);
// Result: [1, 3, 6, 10, 15] (running sum)
```

---

## Best Practices

### 1. Null Checks

```java
int[] arr = getArray();

if (arr != null && arr.length > 0) {
    // Safe to use array
    System.out.println("Array has " + arr.length + " elements");
}
```

### 2. Bounds Checking

```java
int index = getUserInput();

if (index >= 0 && index < arr.length) {
    int value = arr[index];
    System.out.println("Value at index " + index + ": " + value);
} else {
System.out.println("Index out of bounds!");
}
```

### 3. Use Enhanced For Loop When Index Not Needed

```java
// ✅ Good - When you don't need the index
for (int num : numbers) {
    System.out.println(num);
}

// ❌ Less ideal - When index is not used
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

### 4. Consider ArrayList for Dynamic Sizing

```java
import java.util.ArrayList;

// If size changes frequently, use ArrayList
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.remove(0);
```

### 5. Immutable Arrays

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

// Use Collections.unmodifiableList for immutability
String[] arr = {"A", "B", "C"};
List<String> immutable = Collections.unmodifiableList(Arrays.asList(arr));

// immutable.add("D"); // ❌ Throws UnsupportedOperationException
```

---

## Performance Tips

1. **Use appropriate sorting algorithm**: `Arrays.sort()` uses dual-pivot Quicksort for primitives (O(n log n) average) and Timsort for objects
2. **Prefer primitive arrays** over boxed arrays when possible for better performance
3. **Use `Arrays.copyOf()`** instead of manual loops for copying
4. **Consider `parallelSort()`** for large arrays (>8192 elements)
5. **Use `System.arraycopy()`** for maximum performance in copying operations

---

## Common Pitfalls

### 1. Array Reference vs Copy

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = arr1; // ⚠️ Reference, not copy!

arr2[0] = 100; // ⚠️ Modifies arr1 too!
System.out.println(arr1[0]); // Output: 100

// ✅ Use clone() or copyOf() for actual copy
int[] arr3 = arr1.clone();
arr3[0] = 200; // Only modifies arr3
System.out.println(arr1[0]); // Output: 100
```

### 2. IndexOutOfBoundsException

```java
int[] arr = new int[5]; // Valid indices: 0, 1, 2, 3, 4

// arr[5] = 10; // ❌ Error! Index 5 is out of bounds

arr[4] = 10; // ✅ OK - last valid index
arr[0] = 5;  // ✅ OK - first index
```

### 3. NullPointerException

```java
String[] names = new String[3]; // ⚠️ Array exists but elements are null!

// names[0].length(); // ❌ NullPointerException!

// ✅ Initialize first
names[0] = "Alice";
int length = names[0].length(); // Now OK, returns 5
```

### 4. Fixed Size

```java
int[] arr = new int[3]; // Size is fixed at 3

// ❌ Cannot resize! No method to change array size
// arr.resize(5); // This doesn't exist

// ✅ Use ArrayList for dynamic sizing
import java.util.ArrayList;
ArrayList<Integer> dynamicList = new ArrayList<>();
dynamicList.add(1);
dynamicList.add(2);
dynamicList.add(3);
dynamicList.add(4); // Can grow as needed
```

---

## Summary

Java arrays are powerful but fixed-size data structures. Key takeaways:

- **Declaration**: `type[] name` or `type name[]`
- **Initialization**: Static `{...}` or dynamic `new type[size]`
- **Size**: Access via `.length` property (not a method)
- **Sorting**: Use `Arrays.sort()` with optional comparators
- **Copying**: Use `Arrays.copyOf()`, `clone()`, or `System.arraycopy()`
- **Utilities**: `Arrays` class provides comprehensive utility methods
- **Limitations**: Fixed size, consider `ArrayList` for dynamic needs

For more advanced operations and better flexibility, consider using Java Collections Framework (`ArrayList`, `LinkedList`, etc.).

---

## Quick Reference Table

| Method                                              | Description                                          | Return Type/Values                                          | Example                                                                              |
| --------------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `array.length`                                      | Get array size (number of elements)                  | `int` - size of array                                       | `int size = arr.length;`                                                             |
| `array[index]`                                      | Access element at specific index                     | Element type - the value at index                           | `int val = arr[0];`                                                                  |
| `array[index] = value`                              | Set element at specific index                        | `void` - modifies array in-place                            | `arr[0] = 10;`                                                                       |
| `Arrays.sort(arr)`                                  | Sort array in ascending order                        | `void` - modifies array in-place                            | `Arrays.sort(numbers);`                                                              |
| `Arrays.sort(arr, comparator)`                      | Sort array using custom comparator                   | `void` - modifies array in-place                            | `Arrays.sort(arr, Collections.reverseOrder());`                                      |
| `Arrays.sort(arr, from, to)`                        | Sort portion of array from index to index            | `void` - modifies array in-place                            | `Arrays.sort(arr, 0, 5);`                                                            |
| `Arrays.binarySearch(arr, key)`                     | Search for key in sorted array                       | `int` - index if found, negative if not found               | `int idx = Arrays.binarySearch(arr, 5);`                                             |
| `Arrays.copyOf(arr, length)`                        | Create copy of array with specified length           | New array - original unchanged                              | `int[] copy = Arrays.copyOf(arr, arr.length);`                                       |
| `Arrays.copyOfRange(arr, from, to)`                 | Copy range from array (from inclusive, to exclusive) | New array - original unchanged                              | `int[] range = Arrays.copyOfRange(arr, 1, 4);`                                       |
| `System.arraycopy(src, srcPos, dest, destPos, len)` | Copy elements from source to destination             | `void` - modifies destination array                         | `System.arraycopy(src, 0, dest, 0, 5);`                                              |
| `array.clone()`                                     | Create shallow copy of array                         | New array - original unchanged                              | `int[] copy = arr.clone();`                                                          |
| `Arrays.fill(arr, value)`                           | Fill entire array with specified value               | `void` - modifies array in-place                            | `Arrays.fill(arr, 0);`                                                               |
| `Arrays.fill(arr, from, to, value)`                 | Fill range with value (from inclusive, to exclusive) | `void` - modifies array in-place                            | `Arrays.fill(arr, 0, 5, 10);`                                                        |
| `Arrays.setAll(arr, generator)`                     | Set all elements using index-based generator         | `void` - modifies array in-place                            | `Arrays.setAll(arr, i -> i * 2);`                                                    |
| `Arrays.parallelSetAll(arr, generator)`             | Set elements in parallel using generator             | `void` - modifies array in-place                            | `Arrays.parallelSetAll(arr, i -> i * i);`                                            |
| `Arrays.toString(arr)`                              | Convert array to string representation               | `String` - formatted as [1, 2, 3]                           | `String s = Arrays.toString(arr);`                                                   |
| `Arrays.deepToString(arr)`                          | Convert multidimensional array to string             | `String` - nested format [[1, 2], [3, 4]]                   | `String s = Arrays.deepToString(matrix);`                                            |
| `Arrays.equals(arr1, arr2)`                         | Check if two arrays are equal                        | `boolean` - true if equal, false otherwise                  | `boolean eq = Arrays.equals(arr1, arr2);`                                            |
| `Arrays.deepEquals(arr1, arr2)`                     | Deep equality check for multidimensional arrays      | `boolean` - true if equal, false otherwise                  | `boolean eq = Arrays.deepEquals(m1, m2);`                                            |
| `Arrays.hashCode(arr)`                              | Calculate hash code for array                        | `int` - hash code value                                     | `int hash = Arrays.hashCode(arr);`                                                   |
| `Arrays.deepHashCode(arr)`                          | Hash code for multidimensional arrays                | `int` - deep hash code value                                | `int hash = Arrays.deepHashCode(matrix);`                                            |
| `Arrays.asList(arr)`                                | Convert array to fixed-size list                     | `List<T>` - backed by array, changes reflect                | `List<String> list = Arrays.asList(arr);`                                            |
| `Arrays.stream(arr)`                                | Create stream from array for processing              | `IntStream/Stream<T>` - stream pipeline                     | `Arrays.stream(arr).forEach(System.out::println);`                                   |
| `Arrays.stream(arr).sum()`                          | Calculate sum of all array elements                  | `int/long/double` - total sum                               | `int sum = Arrays.stream(arr).sum();`                                                |
| `Arrays.stream(arr).average()`                      | Calculate average of array elements                  | `OptionalDouble` - average if present, empty if array empty | `double avg = Arrays.stream(arr).average().orElse(0);`                               |
| `Arrays.stream(arr).max()`                          | Find maximum value in array                          | `OptionalInt` - max if present, empty if array empty        | `int max = Arrays.stream(arr).max().orElse(0);`                                      |
| `Arrays.stream(arr).min()`                          | Find minimum value in array                          | `OptionalInt` - min if present, empty if array empty        | `int min = Arrays.stream(arr).min().orElse(0);`                                      |
| `Arrays.stream(arr).filter(predicate)`              | Filter elements matching condition                   | Stream - new stream with matching elements                  | `int[] evens = Arrays.stream(arr).filter(n -> n % 2 == 0).toArray();`                |
| `Arrays.stream(arr).map(function)`                  | Transform each element using function                | Stream - new stream with transformed elements               | `int[] doubled = Arrays.stream(arr).map(n -> n * 2).toArray();`                      |
| `Arrays.stream(arr).anyMatch(predicate)`            | Check if any element matches condition               | `boolean` - true if at least one matches, false otherwise   | `boolean has5 = Arrays.stream(arr).anyMatch(n -> n == 5);`                           |
| `Arrays.stream(arr).allMatch(predicate)`            | Check if all elements match condition                | `boolean` - true if all match, false if any doesn't         | `boolean allPos = Arrays.stream(arr).allMatch(n -> n > 0);`                          |
| `Arrays.stream(arr).noneMatch(predicate)`           | Check if no elements match condition                 | `boolean` - true if none match, false if any matches        | `boolean noNeg = Arrays.stream(arr).noneMatch(n -> n < 0);`                          |
| `Arrays.stream(arr).count()`                        | Count number of elements (after filter)              | `long` - count of elements                                  | `long count = Arrays.stream(arr).filter(n -> n > 0).count();`                        |
| `Arrays.stream(arr).findFirst()`                    | Find first matching element                          | `OptionalInt` - first if present, empty if none found       | `int first = Arrays.stream(arr).filter(n -> n > 5).findFirst().orElse(-1);`          |
| `IntStream.range(0, arr.length).filter()`           | Find all indices matching condition                  | `int[]` - array of matching indices                         | `int[] indices = IntStream.range(0, arr.length).filter(i -> arr[i] == 5).toArray();` |
| `Arrays.parallelSort(arr)`                          | Sort large arrays using parallel processing          | `void` - modifies array in-place, faster for large arrays   | `Arrays.parallelSort(largeArray);`                                                   |
| `Arrays.parallelPrefix(arr, op)`                    | Apply cumulative operation (running sum, product)    | `void` - modifies array with cumulative results             | `Arrays.parallelPrefix(arr, (a, b) -> a + b);`                                       |
| `Comparator.comparingInt(keyExtractor)`             | Create comparator for int property                   | `Comparator<T>` - sorts by int field                        | `Arrays.sort(people, Comparator.comparingInt(p -> p.age));`                          |
| `Comparator.comparing(keyExtractor)`                | Create comparator for any comparable property        | `Comparator<T>` - sorts by extracted property               | `Arrays.sort(people, Comparator.comparing(p -> p.name));`                            |
| `comparator.reversed()`                             | Reverse sorting order of comparator                  | `Comparator<T>` - opposite order                            | `Arrays.sort(arr, Comparator.naturalOrder().reversed());`                            |
| `comparator.thenComparing(keyExtractor)`            | Add secondary sorting criteria                       | `Comparator<T>` - sorts by first, then by second            | `Comparator.comparingInt(p -> p.age).thenComparing(p -> p.name);`                    |
| `Comparator.nullsFirst(comparator)`                 | Handle nulls by placing them first                   | `Comparator<T>` - nulls at start, then sorted               | `Arrays.sort(arr, Comparator.nullsFirst(Comparator.naturalOrder()));`                |
| `Comparator.nullsLast(comparator)`                  | Handle nulls by placing them last                    | `Comparator<T>` - sorted first, nulls at end                | `Arrays.sort(arr, Comparator.nullsLast(Comparator.naturalOrder()));`                 |
| `Collections.reverseOrder()`                        | Create reverse natural order comparator              | `Comparator<T>` - descending order                          | `Arrays.sort(arr, Collections.reverseOrder());`                                      |
