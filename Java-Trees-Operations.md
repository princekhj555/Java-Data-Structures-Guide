# Java Trees Operations Guide

A comprehensive guide to working with Binary Trees, Binary Search Trees (BST), and tree algorithms in Java.

## Table of Contents

1. [Tree Basics](#tree-basics)
2. [TreeNode Implementation](#treenode-implementation)
3. [Tree Construction](#tree-construction)
4. [Tree Traversals](#tree-traversals)
5. [Binary Search Tree (BST)](#binary-search-tree-bst)
6. [Tree Properties](#tree-properties)
7. [Common Tree Algorithms](#common-tree-algorithms)
8. [Advanced Operations](#advanced-operations)

---

## Tree Basics

### What is a Tree?

```java
// Tree: Hierarchical data structure with nodes
// - Root: Top node
// - Parent: Node with children
// - Child: Node connected below parent
// - Leaf: Node with no children
// - Height: Length of longest path to leaf
// - Depth: Length of path from root

// Binary Tree: Each node has at most 2 children (left and right)
```

### Tree Terminology

```java
// 1 <- Root (depth 0)
// / \
// 2 3 <- Level 1 (depth 1)
// / \ / \
// 4 5 6 7 <- Level 2 (depth 2, leaves)

// Height of tree: 2
// Size: 7 nodes
// Leaves: 4, 5, 6, 7
// Internal nodes: 1, 2, 3
```

### Types of Binary Trees

```java
// 1. Full Binary Tree: Every node has 0 or 2 children
// 1
// / \
// 2 3
// / \
// 4 5

// 2. Complete Binary Tree: All levels filled except possibly last
// 1
// / \
// 2 3
// / \ /
// 4 5 6

// 3. Perfect Binary Tree: All internal nodes have 2 children, all leaves same level
// 1
// / \
// 2 3
// / \ / \
// 4 5 6 7

// 4. Balanced Binary Tree: Height difference of subtrees ≤ 1 for all nodes
// 5. Binary Search Tree: Left < Root < Right for all nodes
```

---

## TreeNode Implementation

### Basic TreeNode Class

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }

TreeNode(int val, TreeNode left, TreeNode right) {
    this.val = val;
    this.left = left;
    this.right = right;
}
}
```

### Generic TreeNode

```java
class TreeNode<T> {
    T data;
    TreeNode<T> left;
    TreeNode<T> right;

    TreeNode(T data) {
        this.data = data;
    }
}

// Usage
TreeNode<String> root = new TreeNode<>("Root");
TreeNode<Integer> intRoot = new TreeNode<>(10);
```

### TreeNode with Parent Reference

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode parent; // Reference to parent

    TreeNode(int val) {
        this.val = val;
    }
}
```

---

## Tree Construction

### Manual Construction

```java
// Build tree:
//   1
//  / \
// 2   3
// / \
// 4   5

TreeNode root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
```

### From Array (Level-Order)

```java
public static TreeNode buildTreeFromArray(Integer[] arr) {
    if (arr == null || arr.length == 0 || arr[0] == null) {
        return null;
    }

TreeNode root = new TreeNode(arr[0]);
Queue<TreeNode> queue = new LinkedList<>();
queue.offer(root);
int i = 1;

while (!queue.isEmpty() && i < arr.length) {
    TreeNode node = queue.poll();

    // Left child
    if (i < arr.length && arr[i] != null) {
        node.left = new TreeNode(arr[i]);
        queue.offer(node.left);
    }
i++;

// Right child
if (i < arr.length && arr[i] != null) {
    node.right = new TreeNode(arr[i]);
    queue.offer(node.right);
}
i++;
}

return root;
}

// Usage
Integer[] arr = {1, 2, 3, 4, 5, null, 6};
TreeNode root = buildTreeFromArray(arr);
```

### From Preorder and Inorder

```java
public static TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder, 0, preorder.length - 1,
    inorder, 0, inorder.length - 1);
}

private static TreeNode buildTreeHelper(int[] preorder, int preStart, int preEnd,
int[] inorder, int inStart, int inEnd) {
    if (preStart > preEnd || inStart > inEnd) {
        return null;
    }

int rootVal = preorder[preStart];
TreeNode root = new TreeNode(rootVal);

// Find root in inorder
int rootIndex = inStart;
for (int i = inStart; i <= inEnd; i++) {
    if (inorder[i] == rootVal) {
        rootIndex = i;
        break;
    }
}

int leftSize = rootIndex - inStart;
root.left = buildTreeHelper(preorder, preStart + 1, preStart + leftSize,
inorder, inStart, rootIndex - 1);
root.right = buildTreeHelper(preorder, preStart + leftSize + 1, preEnd,
inorder, rootIndex + 1, inEnd);
return root;
}
```

---

## Tree Traversals

### Preorder Traversal (Root → Left → Right)

```java
// Recursive
public static void preorder(TreeNode root) {
    if (root == null) return;
    System.out.print(root.val + " ");
    preorder(root.left);
    preorder(root.right);
}

// Iterative
public static List<Integer> preorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;

    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);

    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);

        // Push right first, then left (to process left first)
        if (node.right != null) stack.push(node.right);
        if (node.left != null) stack.push(node.left);
    }

return result;
}
```

### Inorder Traversal (Left → Root → Right)

```java
// Recursive
public static void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    System.out.print(root.val + " ");
    inorder(root.right);
}

// Iterative
public static List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode current = root;

    while (current != null || !stack.isEmpty()) {
        // Go to leftmost node
        while (current != null) {
            stack.push(current);
            current = current.left;
        }

    // Process node
    current = stack.pop();
    result.add(current.val);

    // Go to right subtree
    current = current.right;
}

return result;
}
```

### Postorder Traversal (Left → Right → Root)

```java
// Recursive
public static void postorder(TreeNode root) {
    if (root == null) return;
    postorder(root.left);
    postorder(root.right);
    System.out.print(root.val + " ");
}

// Iterative (two stacks)
public static List<Integer> postorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;

    Stack<TreeNode> stack1 = new Stack<>();
    Stack<TreeNode> stack2 = new Stack<>();
    stack1.push(root);

    while (!stack1.isEmpty()) {
        TreeNode node = stack1.pop();
        stack2.push(node);
        if (node.left != null) stack1.push(node.left);
        if (node.right != null) stack1.push(node.right);
    }

while (!stack2.isEmpty()) {
    result.add(stack2.pop().val);
}

return result;
}
```

### Level-Order Traversal (BFS)

```java
public static List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);

            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }

    result.add(level);
}

return result;
}
```

### Zigzag Level-Order Traversal

```java
public static List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean leftToRight = true;

    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();

            if (leftToRight) {
                level.add(node.val);
            } else {
            level.add(0, node.val); // Add to front
        }

    if (node.left != null) queue.offer(node.left);
    if (node.right != null) queue.offer(node.right);
}
result.add(level);
leftToRight = !leftToRight;
}

return result;
}
```

---

## Binary Search Tree (BST)

### BST Properties

```java
// Binary Search Tree property:
// - All nodes in left subtree < root
// - All nodes in right subtree > root
// - Both subtrees are also BSTs

//     5
//    / \
//   3   7
//  / \ / \
// 2  4 6  8
// Valid BST: 2 < 3 < 4 < 5 < 6 < 7 < 8
```

### BST Search

```java
public static TreeNode search(TreeNode root, int target) {
    if (root == null || root.val == target) {
        return root;
    }

if (target < root.val) {
    return search(root.left, target);
} else {
return search(root.right, target);
}
}

// Iterative
public static TreeNode searchIterative(TreeNode root, int target) {
    while (root != null && root.val != target) {
        root = (target < root.val) ? root.left : root.right;
    }
return root;
}
```

### BST Insertion

```java
public static TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }

if (val < root.val) {
    root.left = insert(root.left, val);
} else if (val > root.val) {
root.right = insert(root.right, val);
}

return root;
}

// Iterative
public static TreeNode insertIterative(TreeNode root, int val) {
    TreeNode newNode = new TreeNode(val);
    if (root == null) return newNode;

    TreeNode current = root;
    TreeNode parent = null;

    while (current != null) {
        parent = current;
        if (val < current.val) {
            current = current.left;
        } else {
        current = current.right;
    }
}

if (val < parent.val) {
    parent.left = newNode;
} else {
parent.right = newNode;
}

return root;
}
```

### BST Deletion

```java
public static TreeNode delete(TreeNode root, int key) {
    if (root == null) return null;

    if (key < root.val) {
        root.left = delete(root.left, key);
    } else if (key > root.val) {
    root.right = delete(root.right, key);
} else {
// Node to be deleted found

// Case 1: No children (leaf)
if (root.left == null && root.right == null) {
    return null;
}

// Case 2: One child
if (root.left == null) {
    return root.right;
}
if (root.right == null) {
    return root.left;
}

// Case 3: Two children
// Find inorder successor (smallest in right subtree)
TreeNode successor = findMin(root.right);
root.val = successor.val;
root.right = delete(root.right, successor.val);
}

return root;
}

private static TreeNode findMin(TreeNode node) {
    while (node.left != null) {
        node = node.left;
    }
return node;
}
```

### Validate BST

```java
public static boolean isValidBST(TreeNode root) {
    return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

private static boolean isValidBST(TreeNode node, long min, long max) {
    if (node == null) return true;

    if (node.val <= min || node.val >= max) {
        return false;
    }

return isValidBST(node.left, min, node.val) &&
isValidBST(node.right, node.val, max);
}
```

---

## Tree Properties

### Height of Tree

```java
public static int height(TreeNode root) {
    if (root == null) return -1; // Height of empty tree is -1
    // Or return 0 for height of empty tree
    return 1 + Math.max(height(root.left), height(root.right));
}
```

### Size (Number of Nodes)

```java
public static int size(TreeNode root) {
    if (root == null) return 0;
    return 1 + size(root.left) + size(root.right);
}
```

### Maximum Value

```java
public static int findMax(TreeNode root) {
    if (root == null) return Integer.MIN_VALUE;

    int leftMax = findMax(root.left);
    int rightMax = findMax(root.right);

    return Math.max(root.val, Math.max(leftMax, rightMax));
}

// For BST: rightmost node
public static int findMaxInBST(TreeNode root) {
    if (root == null) throw new IllegalArgumentException("Empty tree");

    while (root.right != null) {
        root = root.right;
    }

return root.val;
}
```

### Minimum Value

```java
public static int findMin(TreeNode root) {
    if (root == null) return Integer.MAX_VALUE;

    int leftMin = findMin(root.left);
    int rightMin = findMin(root.right);
    return Math.min(root.val, Math.min(leftMin, rightMin));
}

// For BST: leftmost node
public static int findMinInBST(TreeNode root) {
    if (root == null) throw new IllegalArgumentException("Empty tree");
    while (root.left != null) {
        root = root.left;
    }
return root.val;
}
```
### Check if Balanced

```java
public static boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}

private static int checkHeight(TreeNode node) {
    if (node == null) return 0;

    int leftHeight = checkHeight(node.left);
    if (leftHeight == -1) return -1;

    int rightHeight = checkHeight(node.right);
    if (rightHeight == -1) return -1;

    if (Math.abs(leftHeight - rightHeight) > 1) {
        return -1; // Not balanced
    }

return 1 + Math.max(leftHeight, rightHeight);
}
```

### Diameter (Longest Path Between Any Two Nodes)

```java
private static int diameter = 0;

public static int diameterOfBinaryTree(TreeNode root) {
    diameter = 0;
    height(root);
    return diameter;
}

private static int height(TreeNode node) {
    if (node == null) return 0;

    int leftHeight = height(node.left);
    int rightHeight = height(node.right);

    // Update diameter
    diameter = Math.max(diameter, leftHeight + rightHeight);

    return 1 + Math.max(leftHeight, rightHeight);
}
```

---

## Common Tree Algorithms

### Lowest Common Ancestor (LCA)

```java
// For Binary Tree
public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
        return root;
    }

TreeNode left = lowestCommonAncestor(root.left, p, q);
TreeNode right = lowestCommonAncestor(root.right, p, q);

if (left != null && right != null) {
    return root; // Both found in different subtrees
}

return left != null ? left : right;
}

// For BST (more efficient)
public static TreeNode lowestCommonAncestorBST(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) return null;

    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestorBST(root.left, p, q);
    }

if (p.val > root.val && q.val > root.val) {
    return lowestCommonAncestorBST(root.right, p, q);
}

return root;
}
```

### Invert/Mirror Tree

```java
public static TreeNode invertTree(TreeNode root) {
    if (root == null) return null;

    // Swap children
    TreeNode temp = root.left;
    root.left = root.right;
    root.right = temp;

    // Recursively invert subtrees
    invertTree(root.left);
    invertTree(root.right);

    return root;
}
```

### Serialize and Deserialize

```java
// Serialize tree to string
public static String serialize(TreeNode root) {
    if (root == null) return "null";
    return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

// Deserialize string to tree
public static TreeNode deserialize(String data) {
    Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
    return deserializeHelper(queue);
}

private static TreeNode deserializeHelper(Queue<String> queue) {
    String val = queue.poll();

    if (val.equals("null")) return null;

    TreeNode node = new TreeNode(Integer.parseInt(val));
    node.left = deserializeHelper(queue);
    node.right = deserializeHelper(queue);

    return node;
}
```

### Path Sum

```java
// Check if path from root to leaf sums to target
public static boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;

    // Leaf node
    if (root.left == null && root.right == null) {
        return root.val == targetSum;
    }

return hasPathSum(root.left, targetSum - root.val) ||
hasPathSum(root.right, targetSum - root.val);
}

// Find all paths with target sum
public static List<List<Integer>> pathSum(TreeNode root, int targetSum) {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    pathSumHelper(root, targetSum, path, result);
    return result;
}

private static void pathSumHelper(TreeNode node, int remaining,
List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;

    path.add(node.val);

    // Leaf node with target sum
    if (node.left == null && node.right == null && node.val == remaining) {
        result.add(new ArrayList<>(path));
    }

pathSumHelper(node.left, remaining - node.val, path, result);
pathSumHelper(node.right, remaining - node.val, path, result);

path.remove(path.size() - 1); // Backtrack
}
```

### Right Side View

```java
public static List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    rightSideViewHelper(root, 0, result);
    return result;
}

private static void rightSideViewHelper(TreeNode node, int level,
List<Integer> result) {
    if (node == null) return;

    // First node at this level (from right)
    if (level == result.size()) {
        result.add(node.val);
    }

// Process right first
rightSideViewHelper(node.right, level + 1, result);
rightSideViewHelper(node.left, level + 1, result);
}
```

---

## Advanced Operations

### Kth Smallest in BST

```java
private static int count = 0;
private static int result = 0;

public static int kthSmallest(TreeNode root, int k) {
    count = 0;
    inorderKth(root, k);
    return result;
}

private static void inorderKth(TreeNode node, int k) {
    if (node == null) return;

    inorderKth(node.left, k);
    count++;

    if (count == k) {
        result = node.val;
        return;
    }

inorderKth(node.right, k);
}
```

### Convert BST to Sorted Doubly Linked List

```java
private static TreeNode prev = null;
private static TreeNode head = null;

public static TreeNode bstToDoublyList(TreeNode root) {
    if (root == null) return null;

    prev = null;
    head = null;
    inorderConvert(root);

    // Connect head and tail
    head.left = prev;
    prev.right = head;

    return head;
}

private static void inorderConvert(TreeNode node) {
    if (node == null) return;

    inorderConvert(node.left);

    if (prev == null) {
        head = node; // First node
    } else {
    prev.right = node;
    node.left = prev;
}

prev = node;
inorderConvert(node.right);
}
```

### Vertical Order Traversal

```java
public static List<List<Integer>> verticalOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Map<Integer, List<Integer>> map = new TreeMap<>();
    Queue<TreeNode> nodeQueue = new LinkedList<>();
    Queue<Integer> colQueue = new LinkedList<>();

    nodeQueue.offer(root);
    colQueue.offer(0);

    while (!nodeQueue.isEmpty()) {
        TreeNode node = nodeQueue.poll();
        int col = colQueue.poll();

        map.putIfAbsent(col, new ArrayList<>());
        map.get(col).add(node.val);

        if (node.left != null) {
            nodeQueue.offer(node.left);
            colQueue.offer(col - 1);
        }

    if (node.right != null) {
        nodeQueue.offer(node.right);
        colQueue.offer(col + 1);
    }
}

result.addAll(map.values());
return result;
}
```

---

## Quick Reference Table

| Operation | Description | Time Complexity | Example |
|-----------|-------------|----------------|---------|
| **Construction** ||||
| `new TreeNode(val)` | Create node with value | O(1) | `TreeNode node = new TreeNode(5);` |
| `buildTreeFromArray(arr)` | Build tree from level-order array | O(n) | `TreeNode root = buildTreeFromArray(arr);` |
| `buildTree(preorder, inorder)` | Build from preorder and inorder | O(n) | `TreeNode root = buildTree(pre, in);` |
| **Traversals** ||||
| `preorder(root)` | Visit Root → Left → Right | O(n) | `preorder(root);` |
| `inorder(root)` | Visit Left → Root → Right | O(n) | `inorder(root);` |
| `postorder(root)` | Visit Left → Right → Root | O(n) | `postorder(root);` |
| `levelOrder(root)` | Visit level by level (BFS) | O(n) | `List<List<Integer>> levels = levelOrder(root);` |
| **BST Operations** ||||
| `search(root, target)` | Find node with target value | O(h) avg O(log n) | `TreeNode node = search(root, 5);` |
| `insert(root, val)` | Insert value maintaining BST property | O(h) avg O(log n) | `root = insert(root, 7);` |
| `delete(root, key)` | Delete node with key | O(h) avg O(log n) | `root = delete(root, 5);` |
| `isValidBST(root)` | Check if tree is valid BST | O(n) | `boolean valid = isValidBST(root);` |
| `findMin(root)` | Find minimum value (leftmost) | O(h) | `int min = findMin(root);` |
| `findMax(root)` | Find maximum value (rightmost) | O(h) | `int max = findMax(root);` |
| **Properties** ||||
| `height(root)` | Calculate tree height | O(n) | `int h = height(root);` |
| `size(root)` | Count total nodes | O(n) | `int count = size(root);` |
| `isBalanced(root)` | Check if height-balanced | O(n) | `boolean balanced = isBalanced(root);` |
| `diameterOfBinaryTree(root)` | Find longest path between any nodes | O(n) | `int diam = diameterOfBinaryTree(root);` |
| **Common Algorithms** ||||
| `lowestCommonAncestor(root, p, q)` | Find LCA of two nodes | O(n) | `TreeNode lca = lowestCommonAncestor(root, p, q);` |
| `invertTree(root)` | Mirror/flip tree | O(n) | `TreeNode inverted = invertTree(root);` |
| `serialize(root)` | Convert tree to string | O(n) | `String s = serialize(root);` |
| `deserialize(data)` | Rebuild tree from string | O(n) | `TreeNode root = deserialize(data);` |
| `hasPathSum(root, sum)` | Check if path sums to target | O(n) | `boolean has = hasPathSum(root, 22);` |
| `pathSum(root, sum)` | Find all paths with target sum | O(n) | `List<List<Integer>> paths = pathSum(root, 22);` |
| `rightSideView(root)` | Get rightmost node at each level | O(n) | `List<Integer> view = rightSideView(root);` |
| `kthSmallest(root, k)` | Find kth smallest in BST | O(k) to O(n) | `int val = kthSmallest(root, 3);` |
| `verticalOrder(root)` | Group nodes by vertical column | O(n log n) | `List<List<Integer>> vertical = verticalOrder(root);` |

**Note:** In the time complexities above:

- `n` = number of nodes
- `h` = height of tree
- For balanced BST: h = O(log n)
- For unbalanced BST (worst case): h = O(n)
