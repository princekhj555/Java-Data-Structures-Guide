# Java String Operations Guide

A comprehensive guide to working with Strings in Java, covering creation, manipulation, searching, and common operations.

## Table of Contents
1. [String Basics](#string-basics)
2. [String Creation](#string-creation)
3. [String Properties](#string-properties)
4. [Character Access](#character-access)
5. [String Comparison](#string-comparison)
6. [String Search](#string-search)
7. [String Modification](#string-modification)
8. [String Splitting and Joining](#string-splitting-and-joining)
9. [String Conversion](#string-conversion)
10. [StringBuilder and StringBuffer](#stringbuilder-and-stringbuffer)
11. [Common Patterns](#common-patterns)

---

## String Basics

### Immutability
```java
// Strings are immutable in Java
String str = "Hello";
str.concat(" World"); // Creates new string, doesn't modify original
System.out.println(str); // Still prints "Hello"

// To use the result, assign it
str = str.concat(" World");
System.out.println(str); // Now prints "Hello World"
```

---

## String Creation

### String Literals
```java
// Using string literals (stored in string pool)
String str1 = "Hello";
String str2 = "Hello"; // Points to same object in pool

System.out.println(str1 == str2); // true (same reference)
```

### Using new Keyword
```java
// Using new keyword (creates new object)
String str3 = new String("Hello");
String str4 = new String("Hello");

System.out.println(str3 == str4); // false (different objects)
System.out.println(str3.equals(str4)); // true (same content)
```

### From Character Array
```java
char[] chars = {'H', 'e', 'l', 'l', 'o'};
String str = new String(chars);
System.out.println(str); // "Hello"

// From part of array
String partial = new String(chars, 1, 3); // "ell"
```

### From Bytes
```java
byte[] bytes = {72, 101, 108, 108, 111};
String str = new String(bytes);
System.out.println(str); // "Hello"
```

---

## String Properties

### Length
```java
String str = "Hello World";
int length = str.length(); // 11

// Check if empty
boolean isEmpty = str.isEmpty(); // false
boolean isBlank = str.isBlank(); // false (Java 11+)

// Empty vs blank
String empty = "";
String blank = " ";
System.out.println(empty.isEmpty()); // true
System.out.println(blank.isEmpty()); // false
System.out.println(blank.isBlank()); // true (Java 11+)
```

---

## Character Access

### Get Character at Index
```java
String str = "Hello";
char ch = str.charAt(0); // 'H'
char last = str.charAt(str.length() - 1); // 'o'
```

### Get Character Array
```java
String str = "Hello";
char[] chars = str.toCharArray();
// chars = ['H', 'e', 'l', 'l', 'o']

// Iterate through characters
for (char c : chars) {
    System.out.println(c);
}
```

### Code Points (Unicode)
```java
String str = "Hello 🌍";
int codePoint = str.codePointAt(6); // Gets code point of emoji

// Iterate through code points
str.codePoints().forEach(cp -> {
    System.out.println(Character.toChars(cp));
});
```

---

## String Comparison

### Equality Check
```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = "hello";

// Content comparison
boolean equal = str1.equals(str2); // true
boolean notEqual = str1.equals(str3); // false

// Case-insensitive comparison
boolean equalIgnoreCase = str1.equalsIgnoreCase(str3); // true
```

### Lexicographic Comparison
```java
String str1 = "Apple";
String str2 = "Banana";

int result = str1.compareTo(str2); // Negative (Apple < Banana)
// Returns: negative if str1 < str2, 0 if equal, positive if str1 > str2

int caseInsensitive = str1.compareToIgnoreCase("apple"); // 0
```

### Reference Comparison
```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = new String("Hello");

System.out.println(str1 == str2); // true (same reference in pool)
System.out.println(str1 == str3); // false (different objects)
System.out.println(str1.equals(str3)); // true (same content)
```

---

## String Search

### Contains
```java
String str = "Hello World";

boolean contains = str.contains("World"); // true
boolean notContains = str.contains("Java"); // false
```

### Index Of
```java
String str = "Hello World Hello";

// Find first occurrence
int firstIndex = str.indexOf("Hello"); // 0
int worldIndex = str.indexOf("World"); // 6

// Find from specific index
int secondHello = str.indexOf("Hello", 1); // 12

// Find character
int oIndex = str.indexOf('o'); // 4

// Not found returns -1
int notFound = str.indexOf("Java"); // -1
```

### Last Index Of
```java
String str = "Hello World Hello";

// Find last occurrence
int lastHello = str.lastIndexOf("Hello"); // 12
int lastO = str.lastIndexOf('o'); // 13
```

### Starts With / Ends With
```java
String str = "Hello World";

boolean startsWithHello = str.startsWith("Hello"); // true
boolean startsWithWorld = str.startsWith("World"); // false

// Check from offset
boolean worldAt6 = str.startsWith("World", 6); // true

boolean endsWithWorld = str.endsWith("World"); // true
boolean endsWithHello = str.endsWith("Hello"); // false
```

### Matches (Regex)
```java
String email = "user@example.com";
boolean isValidEmail = email.matches("^[\\w.-]+@[\\w.-]+\\.[a-zA-Z]{2,}$");

String phone = "123-456-7890";
boolean isPhone = phone.matches("\\d{3}-\\d{3}-\\d{4}"); // true
```

---

## String Modification

### Substring
```java
String str = "Hello World";

// From index to end
String sub1 = str.substring(6); // "World"

// From start index to end index (exclusive)
String sub2 = str.substring(0, 5); // "Hello"
String sub3 = str.substring(6, 11); // "World"
```

### Replace
```java
String str = "Hello World";

// Replace all occurrences
String replaced = str.replace('o', 'a'); // "Hella Warld"
String replacedStr = str.replace("World", "Java"); // "Hello Java"

// Replace first occurrence
String replaceFirst = str.replaceFirst("l", "L"); // "HeLlo World"

// Replace all matching regex
String replaceAll = str.replaceAll("[aeiou]", "*"); // "H*ll* W*rld"
```

### Trim and Strip
```java
String str = " Hello World ";

// Remove leading and trailing whitespace
String trimmed = str.trim(); // "Hello World"

// Java 11+ - Unicode-aware whitespace removal
String stripped = str.strip(); // "Hello World"
String stripLeading = str.stripLeading(); // "Hello World "
String stripTrailing = str.stripTrailing(); // " Hello World"
```

### Case Conversion
```java
String str = "Hello World";

String upper = str.toUpperCase(); // "HELLO WORLD"
String lower = str.toLowerCase(); // "hello world"
```

### Concat
```java
String str1 = "Hello";
String str2 = "World";

String concat = str1.concat(" ").concat(str2); // "Hello World"

// Using + operator (preferred)
String combined = str1 + " " + str2; // "Hello World"
```

### Repeat (Java 11+)
```java
String str = "Hello";
String repeated = str.repeat(3); // "HelloHelloHello"

String dash = "-";
String line = dash.repeat(20); // "--------------------"
```

---

## String Splitting and Joining

### Split
```java
String str = "apple,banana,orange";

// Split by delimiter
String[] fruits = str.split(",");
// ["apple", "banana", "orange"]

// Split with limit
String[] limited = str.split(",", 2);
// ["apple", "banana,orange"]

// Split by regex
String text = "one1two2three3";
String[] parts = text.split("\\d+");
// ["one", "two", "three"]
```

### Join
```java
// Join with delimiter (Java 8+)
String joined = String.join(", ", "apple", "banana", "orange");
// "apple, banana, orange"

// Join array
String[] words = {"Hello", "World"};
String sentence = String.join(" ", words); // "Hello World"

// Join list
List<String> list = Arrays.asList("Java", "Python", "C++");
String languages = String.join(", ", list);
// "Java, Python, C++"
```

---

## String Conversion

### To String
```java
// Primitive to string
String intStr = String.valueOf(123); // "123"
String doubleStr = String.valueOf(45.67); // "45.67"
String boolStr = String.valueOf(true); // "true"

// Object to string
Object obj = new Integer(100);
String objStr = String.valueOf(obj); // "100"

// Using toString()
Integer num = 42;
String numStr = num.toString(); // "42"
```

### From String
```java
String str = "123";

// String to primitives
int num = Integer.parseInt(str); // 123
double d = Double.parseDouble("45.67"); // 45.67
boolean b = Boolean.parseBoolean("true"); // true
long l = Long.parseLong("1000000"); // 1000000

// With wrapper classes
Integer integer = Integer.valueOf(str); // 123
Double doubleObj = Double.valueOf("45.67"); // 45.67
```

### Format Strings
```java
// Using String.format()
String formatted = String.format("Name: %s, Age: %d", "John", 25);
// "Name: John, Age: 25"

String price = String.format("Price: $%.2f", 19.99);
// "Price: $19.99"

// Formatting numbers
String padded = String.format("%05d", 42); // "00042"
String hex = String.format("%x", 255); // "ff"

// Using formatted() (Java 15+)
String result = "Hello %s".formatted("World"); // "Hello World"
```

---

## StringBuilder and StringBuffer

### StringBuilder (Not Thread-Safe, Faster)
```java
StringBuilder sb = new StringBuilder();

// Append
sb.append("Hello");
sb.append(" ");
sb.append("World");

// Insert
sb.insert(6, "Beautiful ");
// "Hello Beautiful World"

// Replace
sb.replace(6, 16, "Amazing");
// "Hello Amazing World"

// Delete
sb.delete(5, 13);
// "Hello World"

// Reverse
sb.reverse();
// "dlroW olleH"

// Convert to string
String result = sb.toString();
```

### StringBuffer (Thread-Safe, Slower)
```java
StringBuffer sbf = new StringBuffer("Hello");

// Same methods as StringBuilder
sbf.append(" World");
sbf.insert(6, "Java ");
String result = sbf.toString(); // "Hello Java World"
```

### Efficient String Building
```java
// Bad - creates many objects
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i + ","; // Don't do this!
}

// Good - efficient
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i).append(",");
}
String result = sb.toString();
```

---

## Common Patterns

### Palindrome Check
```java
public static boolean isPalindrome(String str) {
    String cleaned = str.replaceAll("[^a-zA-Z0-9]", "").toLowerCase();
    return cleaned.equals(new StringBuilder(cleaned).reverse().toString());
}

// Usage
boolean result = isPalindrome("A man, a plan, a canal: Panama"); // true
```

### Reverse String
```java
String str = "Hello";

// Using StringBuilder
String reversed = new StringBuilder(str).reverse().toString();
// "olleH"

// Manual approach
String manual = "";
for (int i = str.length() - 1; i >= 0; i--) {
    manual += str.charAt(i);
}
```

### Count Characters
```java
String str = "Hello World";
Map<Character, Integer> charCount = new HashMap<>();

for (char c : str.toCharArray()) {
    charCount.put(c, charCount.getOrDefault(c, 0) + 1);
}
// {H=1, e=1, l=3, o=2, =1, W=1, r=1, d=1}
```

### Remove Duplicates
```java
String str = "programming";

// Using LinkedHashSet to maintain order
String unique = str.chars()
.distinct()
.mapToObj(c -> String.valueOf((char) c))
.collect(Collectors.joining());
// "progamin"
```

### Anagram Check
```java
public static boolean areAnagrams(String str1, String str2) {
    if (str1.length() != str2.length()) return false;
    char[] arr1 = str1.toLowerCase().toCharArray();
    char[] arr2 = str2.toLowerCase().toCharArray();
    Arrays.sort(arr1);
    Arrays.sort(arr2);
    return Arrays.equals(arr1, arr2);
}

// Usage
boolean result = areAnagrams("listen", "silent"); // true
```

---

## Best Practices

### 1. Use equals() for Comparison
```java
// ❌ Bad - compares references
if (str1 == str2) { }

// ✅ Good - compares content
if (str1.equals(str2)) { }
```

### 2. Use StringBuilder for Concatenation
```java
// ❌ Bad - inefficient in loops
String result = "";
for (String s : strings) {
    result += s;
}

// ✅ Good - efficient
StringBuilder sb = new StringBuilder();
for (String s : strings) {
    sb.append(s);
}
```

### 3. Use String.join() for Multiple Strings
```java
// ❌ Less readable
String result = str1 + ", " + str2 + ", " + str3;

// ✅ More readable
String result = String.join(", ", str1, str2, str3);
```

### 4. Check null Before Operations
```java
String str = getUserInput();

// ✅ Safe
if (str != null && !str.isEmpty()) {
    System.out.println(str.toUpperCase());
}
```

---

## Quick Reference Table

| Method | Description | Return Type/Values | Example |
|--------|-------------|-------------------|---------|
| `str.length()` | Get string length (number of characters) | `int` - length of string | `int len = str.length();` |
| `str.isEmpty()` | Check if string is empty | `boolean` - true if length is 0, false otherwise | `boolean empty = str.isEmpty();` |
| `str.isBlank()` | Check if string is empty or whitespace (Java 11+) | `boolean` - true if empty/whitespace, false otherwise | `boolean blank = str.isBlank();` |
| `str.charAt(index)` | Get character at specific index | `char` - character at index | `char ch = str.charAt(0);` |
| `str.toCharArray()` | Convert string to character array | `char[]` - array of characters | `char[] chars = str.toCharArray();` |
| `str.equals(other)` | Compare string content for equality | `boolean` - true if equal, false otherwise | `boolean eq = str.equals("Hello");` |
| `str.equalsIgnoreCase(other)` | Compare strings ignoring case | `boolean` - true if equal ignoring case, false otherwise | `boolean eq = str.equalsIgnoreCase("hello");` |
| `str.compareTo(other)` | Lexicographic comparison | `int` - negative if less, 0 if equal, positive if greater | `int cmp = str1.compareTo(str2);` |
| `str.compareToIgnoreCase(other)` | Case-insensitive lexicographic comparison | `int` - comparison result ignoring case | `int cmp = str1.compareToIgnoreCase(str2);` |
| `str.contains(sequence)` | Check if string contains sequence | `boolean` - true if contains, false otherwise | `boolean has = str.contains("Hello");` |
| `str.indexOf(str/char)` | Find first occurrence index | `int` - index if found, -1 if not found | `int idx = str.indexOf("World");` |
| `str.indexOf(str, fromIndex)` | Find occurrence starting from index | `int` - index if found, -1 if not found | `int idx = str.indexOf("o", 5);` |
| `str.lastIndexOf(str/char)` | Find last occurrence index | `int` - last index if found, -1 if not found | `int idx = str.lastIndexOf("o");` |
| `str.startsWith(prefix)` | Check if string starts with prefix | `boolean` - true if starts with prefix, false otherwise | `boolean starts = str.startsWith("Hello");` |
| `str.endsWith(suffix)` | Check if string ends with suffix | `boolean` - true if ends with suffix, false otherwise | `boolean ends = str.endsWith("World");` |
| `str.matches(regex)` | Check if string matches regex pattern | `boolean` - true if matches, false otherwise | `boolean match = str.matches("\\d+");` |
| `str.substring(start)` | Extract substring from start to end | `String` - new substring from start | `String sub = str.substring(5);` |
| `str.substring(start, end)` | Extract substring from start to end (exclusive) | `String` - new substring in range | `String sub = str.substring(0, 5);` |
| `str.replace(old, new)` | Replace all occurrences of character/string | `String` - new string with replacements | `String rep = str.replace("o", "a");` |
| `str.replaceFirst(regex, replacement)` | Replace first match of regex | `String` - new string with first replacement | `String rep = str.replaceFirst("\\d", "X");` |
| `str.replaceAll(regex, replacement)` | Replace all matches of regex | `String` - new string with all replacements | `String rep = str.replaceAll("\\d", "X");` |
| `str.trim()` | Remove leading and trailing whitespace | `String` - new trimmed string | `String trimmed = str.trim();` |
| `str.strip()` | Unicode-aware whitespace removal (Java 11+) | `String` - new stripped string | `String stripped = str.strip();` |
| `str.stripLeading()` | Remove leading whitespace (Java 11+) | `String` - string without leading whitespace | `String s = str.stripLeading();` |
| `str.stripTrailing()` | Remove trailing whitespace (Java 11+) | `String` - string without trailing whitespace | `String s = str.stripTrailing();` |
| `str.toUpperCase()` | Convert to uppercase | `String` - new uppercase string | `String upper = str.toUpperCase();` |
| `str.toLowerCase()` | Convert to lowercase | `String` - new lowercase string | `String lower = str.toLowerCase();` |
| `str.concat(other)` | Concatenate strings | `String` - new concatenated string | `String result = str.concat(" World");` |
| `str.repeat(count)` | Repeat string n times (Java 11+) | `String` - new repeated string | `String rep = str.repeat(3);` |
| `str.split(regex)` | Split string by regex delimiter | `String[]` - array of split parts | `String[] parts = str.split(",");` |
| `str.split(regex, limit)` | Split with maximum number of splits | `String[]` - array with limited splits | `String[] parts = str.split(",", 2);` |
| `String.join(delimiter, elements)` | Join strings with delimiter | `String` - joined string | `String joined = String.join(", ", "a", "b");` |
| `String.valueOf(value)` | Convert any type to string | `String` - string representation | `String s = String.valueOf(123);` |
| `String.format(format, args)` | Create formatted string | `String` - formatted string | `String s = String.format("Age: %d", 25);` |
| `str.formatted(args)` | Format string (Java 15+) | `String` - formatted string | `String s = "Age: %d".formatted(25);` |
| `Integer.parseInt(str)` | Parse string to int | `int` - parsed integer value | `int num = Integer.parseInt("123");` |
| `Double.parseDouble(str)` | Parse string to double | `double` - parsed double value | `double d = Double.parseDouble("45.67");` |
| `sb.append(str)` | Append to StringBuilder | `StringBuilder` - this builder for chaining | `sb.append("Hello");` |
| `sb.insert(offset, str)` | Insert string at position | `StringBuilder` - this builder for chaining | `sb.insert(0, "Start");` |
| `sb.delete(start, end)` | Delete characters in range | `StringBuilder` - this builder for chaining | `sb.delete(0, 5);` |
| `sb.replace(start, end, str)` | Replace range with string | `StringBuilder` - this builder for chaining | `sb.replace(0, 5, "New");` |
| `sb.reverse()` | Reverse the string | `StringBuilder` - this builder reversed | `sb.reverse();` |
| `sb.toString()` | Convert StringBuilder to String | `String` - final string result | `String s = sb.toString();` |
