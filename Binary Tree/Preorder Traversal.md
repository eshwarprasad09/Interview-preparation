Here’s a detailed breakdown of your **Preorder Traversal** code for a binary tree, with **step-by-step explanation, concepts used, dry run, and complexity analysis**.

---

# 🌲 Binary Tree – Preorder Traversal

---

## ✅ Problem Statement

Return the **preorder traversal** of a binary tree's nodes' values.

> **Preorder Traversal Order:**
> **Node → Left → Right**

---

## 🧠 Intuition

Preorder traversal means:

1. Visit the current node.
2. Traverse the left subtree.
3. Traverse the right subtree.

We use recursion to naturally follow this order.

---

## 💡 Concepts & Patterns Used

* **Tree Traversal**
* **Depth-First Search (DFS)**
* **Recursion**

---

## ✨ Code Explanation – Step-by-Step

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<>();
        preOrder(root, res);
        return res;
    }

    public void preOrder(TreeNode root, ArrayList<Integer> res){
        if(root == null) return;                // Base case: if node is null, do nothing

        res.add(root.val);                      // Step 1: Visit current node (root)
        preOrder(root.left, res);               // Step 2: Traverse left subtree
        preOrder(root.right, res);              // Step 3: Traverse right subtree
    }
}
```

---

## 🧪 Dry Run Example

### Input Tree:

```
      1
     / \
    2   3
   / \
  4   5
```

### Execution Order:

1. Visit 1 → `res = [1]`
2. Go left to 2 → `res = [1, 2]`
3. Go left to 4 → `res = [1, 2, 4]`
4. Backtrack to 2 → go right to 5 → `res = [1, 2, 4, 5]`
5. Backtrack to 1 → go right to 3 → `res = [1, 2, 4, 5, 3]`

---

## ⏱ Time and Space Complexity

| Metric              | Value             |
| ------------------- | ----------------- |
| 🕒 Time Complexity  | O(n)              |
| 🧠 Space Complexity | O(h) (call stack) |

> Where `n` is number of nodes and `h` is the height of the tree.

---

## 📌 How to Identify This Pattern

* Problem mentions **visiting or printing nodes in a specific order**.
* Asked to **visit root before children** → use **Preorder traversal**.
* Use **recursion or stack (for iterative version)** when depth-first traversal is required.

---

## 🛠 Real-Life Use Cases

* **Serializing trees**
* **Cloning a tree**
* **Evaluating expression trees**

---

## 📋 Summary (Quick Revision)

* **Preorder:** Node → Left → Right
* Use recursion to simplify logic
* Time: O(n), Space: O(h)
* Useful for problems where you need to process root before children

---

Would you like the **iterative version using stack** or **Morris preorder traversal (O(1) space)** as well?
