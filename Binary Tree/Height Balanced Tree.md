A **height-balanced tree**, most commonly referred to in the context of a **Balanced Binary Tree**, is a binary tree in which **the depth of the two subtrees of every node never differs by more than one**.

---

## ✅ Definition

> A binary tree is **height-balanced** if, for every node:
>
> $$
> $$

\| \text{height(left subtree)} - \text{height(right subtree)} | \leq 1
]

---

## 📚 Use Case

* Height-balanced trees avoid degeneration to a **linked list**, ensuring **efficient operations** like:

    * Insertion: O(log n)
    * Deletion: O(log n)
    * Search: O(log n)

Balanced trees ensure optimal time complexity for all basic operations.

---

## 💡 Concepts & Patterns Used

* **Post-order traversal** (Left → Right → Node)
* **Recursive height calculation**
* **Early pruning** if any subtree is already unbalanced

---

## 💻 Java Code (Leetcode 110 - [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/))

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    // Returns -1 if unbalanced, otherwise returns height
    private int checkHeight(TreeNode node) {
        if (node == null) return 0;

        int leftHeight = checkHeight(node.left);
        if (leftHeight == -1) return -1;

        int rightHeight = checkHeight(node.right);
        if (rightHeight == -1) return -1;

        if (Math.abs(leftHeight - rightHeight) > 1) return -1;

        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

---

## 🧠 Intuition & Explanation

* We calculate the height **bottom-up**.
* If any subtree is unbalanced, return `-1` early.
* Otherwise, return actual height of the subtree.
* If any subtree returns `-1`, we propagate `-1` upwards and return `false` overall.

---

## 🧪 Dry Run

### Tree:

```
       1
      / \
     2   3
    /
   4
```

* Node 4 → height = 1
* Node 2 → left height = 1, right height = 0 → balanced → height = 2
* Node 3 → height = 1
* Node 1 → left = 2, right = 1 → balanced → height = 3 ✅

---

## ⏱ Time & Space Complexity

| Metric              | Value                                                             |
| ------------------- | ----------------------------------------------------------------- |
| 🕒 Time Complexity  | O(n) – one visit per node                                         |
| 🧠 Space Complexity | O(h) – recursion stack (O(log n) in balanced case, O(n) in worst) |

---

## 📌 Summary (Quick Revision)

* A tree is balanced if all nodes satisfy:
  `|height(left) - height(right)| ≤ 1`
* Use **post-order traversal** to compute height & detect imbalance.
* Return `-1` early for unbalanced subtrees.
* Time: O(n), Space: O(h)

---

Would you like code to **build a height-balanced BST from a sorted array** next?
