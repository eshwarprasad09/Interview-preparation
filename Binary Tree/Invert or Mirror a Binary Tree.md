Here’s a complete explanation in **Markdown format** for solving the **“Mirror (Invert) a Binary Tree”** problem, ideal for interviews and last-minute revision.

---

# 🌳 Invert / Mirror a Binary Tree

---

## ✅ Problem Statement

> Given the root of a binary tree, invert it (mirror it).
>
> That is, swap the left and right children of **every node** in the tree.

---

### 🧪 Example

Original:

```
      4
     / \
    2   7
   / \ / \
  1  3 6  9
```

Mirrored Output:

```
      4
     / \
    7   2
   / \ / \
  9  6 3  1
```

---

## 💡 Intuition

At each node, we **swap its left and right subtrees**. Then recursively invert both subtrees.

This is a perfect example of:

* **Divide & Conquer** (subproblems are smaller trees)
* **Postorder traversal** (solve subtrees before handling the root)
* **DFS recursion** (Depth-First Search)

---

## 🔍 How to Identify the Pattern

| Problem Clue                       | Pattern to Use                      |
| ---------------------------------- | ----------------------------------- |
| Binary tree + recursion allowed    | Use **DFS + recursion**             |
| Perform an operation at every node | Use **pre/post/in-order traversal** |
| Left <-> Right subtree switch      | Use **swap + recursion**            |

---

## 🛠️ Concepts & Patterns Used

| Concept / Pattern    | Usage                          |
| -------------------- | ------------------------------ |
| **DFS (Post-order)** | Visit left, right, then root   |
| **Recursion**        | Invert subtrees bottom-up      |
| **Tree traversal**   | Covers every node exactly once |
| **Swapping**         | Mirror transformation          |

---

## 🧰 Full Code with Inline Comments

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
    public TreeNode invertTree(TreeNode root) {
        // Base case: if the node is null, return null
        if (root == null) return null;

        // Step 1: Recursively invert left and right subtrees
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);

        // Step 2: Swap left and right
        root.left = right;
        root.right = left;

        // Step 3: Return the root (with children inverted)
        return root;
    }
}
```

---

## 🔄 Step-by-Step Dry Run

### Input:

```text
        1
       / \
      2   3
```

### Execution:

1. Call `invertTree(1)`
2. `invertTree(2)` → returns 2 (after inverting left/right = null)
3. `invertTree(3)` → returns 3
4. Swap `2` and `3`
5. Final tree:

```
        1
       / \
      3   2
```

---

## 🧠 Real-Life Use Case

* Image processing: Inverting pixel relationships like mirrors.
* Networking: Mirroring decision trees (e.g., firewall rule propagation).
* Game dev: Mirroring scene graphs for camera effects or object movement.

---

## ⏱️ Time and Space Complexity

| Metric                      | Complexity                                                |
| --------------------------- | --------------------------------------------------------- |
| **Time**                    | `O(n)` — each node visited once                           |
| **Space (recursive stack)** | `O(h)` — height of the tree (worst: O(n), best: O(log n)) |

---

## 📌 Summary: Quick Revision Points

* 🧠 Swap left and right children of every node.
* 🔁 Use DFS recursion: post-order preferred (invert children, then swap).
* ❌ No need for extra memory (no queue/stack needed manually).
* 📈 Time: `O(n)` — one visit per node.
* 📦 Space: `O(h)` — due to recursion stack.

---

Let me know if you want:

* ✅ Iterative version using queue (BFS)
* 📊 Diagram illustration of recursive stack
* 🔁 Mirror a subtree from a given node

This is a **must-practice** problem for recursion + tree traversal mastery!
