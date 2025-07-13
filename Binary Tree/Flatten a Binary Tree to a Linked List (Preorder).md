Here's a complete **interview-ready explanation** for:

---

# 🔄 Flatten a Binary Tree to a Linked List (Preorder)

---

## ✅ Problem Statement

> Given the root of a binary tree, **flatten it into a "linked list"** in-place following **preorder traversal** (`root → left → right`).

📌 You must do this **in-place** (modify the tree directly).

---

## 🧠 Intuition

We are asked to transform a binary tree into a **right-skewed linked list** that matches **preorder traversal**.

### That means:

* Visit root
* Flatten left subtree
* Flatten right subtree
* Reconnect everything so that:

    * Left pointers are null
    * Right pointers point to next node in preorder

---

## 🔍 Pattern Recognition

| Clue from Problem                | Pattern to Use           |
| -------------------------------- | ------------------------ |
| Preorder traversal needed        | Use **DFS (recursive)**  |
| Modify tree in-place             | Use **pointer rewiring** |
| Connect nodes in traversal order | Use **global pointer**   |

---

## 🛠️ Concepts & Patterns Used

| Concept / Pattern              | Why It’s Used                        |
| ------------------------------ | ------------------------------------ |
| Preorder Traversal             | Visit nodes in `root → left → right` |
| DFS (Depth-First)              | Traverse entire tree                 |
| In-place Tree Manipulation     | No extra memory allowed              |
| Reverse Preorder (for optimal) | Used in recursive variant            |

---

## ✅ Full Code with Inline Comments

### Approach: Reverse Preorder + Global Previous Pointer (Postorder-like)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    // This keeps track of the previously visited node (predecessor)
    private TreeNode prev = null;

    public void flatten(TreeNode root) {
        if (root == null) return;

        // Process right subtree first
        flatten(root.right);
        // Then process left subtree
        flatten(root.left);

        // Now modify current root's right to point to prev (flattened part)
        root.right = prev;
        root.left = null; // Left must be null in final result
        prev = root; // Move prev to current
    }
}
```

---

## 🔁 Step-by-Step Dry Run

### Input:

```text
        1
       / \
      2   5
     / \   \
    3   4   6
```

### Preorder: `1 → 2 → 3 → 4 → 5 → 6`

### Flattened Output:

```text
1 → 2 → 3 → 4 → 5 → 6
```

### How it works:

* Process recursively:

    * `flatten(6)` → set `right = null`, `left = null`, `prev = 6`
    * `flatten(5)` → set `right = 6`, `left = null`, `prev = 5`
    * `flatten(4)` → `right = 5`, `prev = 4`
    * ...
* Final structure connects nodes in preorder using `right` pointers only.

---

## 📚 How to Identify the Pattern

1. **Preorder traversal** mentioned → think DFS.
2. **Linked list structure** on tree → consider pointer rewiring.
3. **In-place modification** → avoid extra data structures.

---

## ⏱️ Time & Space Complexity

| Metric            | Value                                      |
| ----------------- | ------------------------------------------ |
| Time              | `O(n)` — visit each node once              |
| Space (recursion) | `O(h)` — height of the tree (stack frames) |

For a balanced tree: `O(log n)`
For a skewed tree: `O(n)`

---

## 📌 Summary for Quick Revision

* 🔁 Use **postorder** reverse preorder (right → left → root)
* 🧠 Track a **`prev` node** to wire `right` pointers
* 🧱 Set `left = null` for all nodes
* ⏱️ Time: `O(n)`, 📦 Space: `O(h)`
* ⚡ No extra memory or dummy node needed

---

## 💡 Real-Life Use Cases

* Flattening hierarchical structures into sequences
* XML/HTML flattening
* Converting a tree to stream/iterator-based APIs
* Used in serialization or flattening file systems

---

Let me know if you want:

* 🔁 Iterative stack-based solution
* 🧱 Convert tree to doubly linked list
* 📦 JavaScript/Python equivalent code
