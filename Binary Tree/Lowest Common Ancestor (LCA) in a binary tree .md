Here's a **complete step-by-step explanation** of the **Lowest Common Ancestor (LCA) in a Binary Tree** — perfect for interviews, notes, and revisions.

---

# 🔍 Lowest Common Ancestor (LCA) in a Binary Tree

---

## ✅ Problem Statement

> Given the root of a binary tree and two nodes `p` and `q`, return their **lowest common ancestor (LCA)**.

### Definition:

> The **lowest common ancestor** of two nodes `p` and `q` is the **lowest (i.e., deepest)** node in the tree that has **both `p` and `q` as descendants** (a node can be a descendant of itself).

---

## 🧠 Intuition

Think of the LCA as the point **where paths to `p` and `q` split**. If you go down from the root:

* If both nodes are in the **left subtree**, recurse left.
* If both are in the **right subtree**, recurse right.
* If they split, the current node is the LCA.

---

## 🔍 Pattern & Strategy

| Clue from problem         | Implied Strategy             |
| ------------------------- | ---------------------------- |
| Binary Tree               | Use **DFS Traversal**        |
| Must explore both sides   | Use **post-order** traversal |
| Two targets, lowest split | Use **divide & conquer**     |

---

## 🛠️ Concepts & Patterns Used

| Concept                    | Use                                  |
| -------------------------- | ------------------------------------ |
| DFS Traversal (Post-order) | Explore left/right before decision   |
| Recursive Tree Search      | Search both subtrees for `p` and `q` |
| Divide and Conquer         | Combine answers from subtrees        |

---

## 🧰 Full Code with Inline Comments

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case 1: if root is null, no LCA
        if (root == null) return null;

        // Base case 2: if root matches p or q, root might be LCA
        if (root == p || root == q) return root;

        // Recur for left and right subtrees
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // Case 1: p and q are found on both sides → current node is LCA
        if (left != null && right != null) return root;

        // Case 2: only one side returned a node → bubble it up
        return left != null ? left : right;
    }
}
```

---

## 🔄 Step-by-Step Dry Run

### Input Tree:

```
       3
      / \
     5   1
    / \ / \
   6  2 0  8
     / \
    7   4
```

### `p = 5`, `q = 1`

* `LCA(3)` → calls `LCA(5)` and `LCA(1)`
* Both calls return the nodes themselves → LCA is `3`

### `p = 5`, `q = 4`

* Left subtree of `3` returns `5` (LCA is below)
* Right is null → return `5`

---

## 🧠 Real-Life Use Case

* **Org chart**: Find common manager of two employees.
* **Filesystem**: Find common parent directory of two files.
* **Version control**: Find common ancestor of two git branches.

---

## ⏱️ Time and Space Complexity

| Metric    | Complexity                                    |
| --------- | --------------------------------------------- |
| **Time**  | `O(n)` — every node visited once              |
| **Space** | `O(h)` — height of the tree (recursion stack) |

---

## 📌 Summary: Quick Revision Points

* ✅ Use **post-order DFS**: left → right → root
* 🔁 Return `p`, `q`, or null at each step
* 🧠 If both sides return non-null → current node is LCA
* 📦 Space = O(h), Time = O(n)
* 🔁 Use recursive divide & conquer to locate split point

---

Let me know if you’d like:

* 📊 Iterative version using parent map
* 🧠 Generalization for **N-ary tree**
* ✅ Application in **BST** where optimizations are possible (`O(log n)` for balanced BSTs)
