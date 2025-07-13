Here's the complete explanation of **Inorder Traversal of a Binary Tree**, structured in markdown for quick revision and deep understanding.

---

# 🌳 Binary Tree – Inorder Traversal

---

## ✅ Problem Statement

Given the `root` of a binary tree, return the **inorder traversal** of its node values.

> **Inorder Traversal Order:**
> **Left → Node → Right**

---

## 🧠 Intuition

Inorder traversal means:

1. Visit the **left subtree**
2. Visit the **current node**
3. Visit the **right subtree**

This traversal is especially important in **Binary Search Trees (BSTs)** because it returns values in **sorted order**.

---

## 💡 Concepts & Patterns Used

* **Recursion** (DFS style)
* **Stack** (for iterative version)
* **Inorder traversal** pattern: Left → Root → Right

---

## ✨ Java Code: Recursive Inorder Traversal

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }

    private void inorder(TreeNode root, List<Integer> result) {
        if (root == null) return;

        // 1. Traverse left subtree
        inorder(root.left, result);

        // 2. Visit current node
        result.add(root.val);

        // 3. Traverse right subtree
        inorder(root.right, result);
    }
}
```

---

## 🔁 Iterative Version Using Stack

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            // Reach the leftmost node
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            curr = stack.pop();             // Node with no more left children
            result.add(curr.val);          // Visit node
            curr = curr.right;             // Explore right subtree
        }

        return result;
    }
}
```

---

## 📘 Step-by-Step Dry Run

### Example Tree:

```
    1
     \
      2
     /
    3
```

### Inorder:

* Left of 1 → `null`
* Visit 1
* Go to right → 2
* Left of 2 → 3
* Left of 3 → `null`
* Visit 3
* Right of 3 → `null`
* Visit 2

**Result:** `[1, 3, 2]`

---

## 🛠 How to Identify This Pattern?

* If asked to **visit nodes in specific order**, recursion or stack-based traversal fits.
* Use **inorder** when:

    * You want **sorted values from BST**
    * Need to **process nodes between children**

---

## ⏱ Time and Space Complexity

| Metric              | Recursive         | Iterative    |
| ------------------- | ----------------- | ------------ |
| 🕒 Time Complexity  | O(n)              | O(n)         |
| 🧠 Space Complexity | O(h) (call stack) | O(n) (stack) |

> `n` = number of nodes
> `h` = height of the tree

---

## 🔍 Real-Life Use Case

* **BST validation**
* **Expression tree evaluation**
* **Sorted output from binary search tree**

---

## 📌 Summary (Quick Revision)

* **Inorder Traversal:** Left → Root → Right
* Use **recursion** for simplicity or **stack** for iteration
* Time: O(n), Space: O(h)
* Useful for **in-order** processing in trees

---

Let me know if you'd like the **Morris Traversal (O(1) space)** version too!
