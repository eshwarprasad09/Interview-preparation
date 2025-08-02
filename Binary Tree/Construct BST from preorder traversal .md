Here's a complete interview-friendly guide to solving the problem **“Construct Binary Search Tree from Preorder Traversal”** — using **recursion with upper bounds** to maintain BST rules.

---

# 🌳 Construct BST from Preorder Traversal

---

## ✅ Problem Statement

Given an integer array `preorder`, construct a **Binary Search Tree (BST)** and return its root.

📌 Example:

```text
Input: preorder = [8,5,1,7,10,12]
Output: Root of BST
```

---

## 🧠 Intuition

In **preorder traversal**, the order is:

```
Root → Left → Right
```

So:

* The **first element** is always the **root**.
* All elements after it, until the first greater element, belong to the **left subtree**.
* The rest belong to the **right subtree**.

But instead of splitting the array every time (which is costly), we maintain an **index pointer** and an **upper bound** to ensure BST properties recursively.

---

## 🔍 Pattern Recognition

| Clue from Problem                 | Pattern / Technique       |
| --------------------------------- | ------------------------- |
| Build tree from traversal         | Use recursion             |
| BST property: left < root < right | Use bounds (min/max)      |
| Traversal: preorder (Root→L→R)    | Process left before right |

---

## ✅ Java Code (With Inline Explanation)

```java
class Solution {
    private int index = 0;

    public TreeNode bstFromPreorder(int[] preorder) {
        return build(preorder, Integer.MAX_VALUE);
    }

    // Helper function to build tree with an upper bound
    private TreeNode build(int[] preorder, int bound) {
        // Base case: reached end of array or value exceeds bound
        if (index == preorder.length || preorder[index] > bound)
            return null;

        int val = preorder[index++];
        TreeNode root = new TreeNode(val);

        // All left children must be less than val
        root.left = build(preorder, val);

        // All right children must be greater than val but <= bound
        root.right = build(preorder, bound);

        return root;
    }
}
```

---

## 🔂 Dry Run Example

Input: `preorder = [8, 5, 1, 7, 10, 12]`

1. `index=0`, val=8 → root

    * left subtree: bound = 8

        * index=1, val=5

            * left: val=1 < 5
            * right: val=7 > 1 < 5
        * right: val=10 > 5 → break
    * right subtree: val=10 < ∞

        * right: val=12 > 10 < ∞

Final Tree:

```
       8
     /   \
    5     10
   / \      \
  1   7     12
```

---

## 📚 Concepts & Patterns Used

| Concept/Pattern    | Role                    |
| ------------------ | ----------------------- |
| Recursion          | Tree building           |
| Preorder Traversal | Root-first order        |
| Upper Bound (BST)  | Maintain BST properties |

---

## ⏱ Time and Space Complexity

| Metric | Complexity                            |
| ------ | ------------------------------------- |
| Time   | `O(n)` — visit each node once         |
| Space  | `O(h)` — recursion stack (h = height) |

* Balanced BST → `O(log n)` space
* Skewed BST → `O(n)` space

---

## 💡 Real-Life Use Cases

* Reconstructing tree from logs or serialized structure
* Deserializing BSTs in communication protocols
* Version control trees or hierarchical configs

---

## 📌 Summary (Quick Revision)

* ✅ Use **preorder** to guide recursion (root → left → right)
* 📈 Track **current index** in preorder array
* 🔐 Use **upper bounds** to enforce BST constraints
* 🔁 Recursively build left subtree with root as upper bound
* ⏱ Time = `O(n)`, Space = `O(h)`

---

Let me know if you’d like:

* 🧪 Python/C++ version
* 📥 Postorder or Inorder reconstruction variant
* 🔁 Build from preorder and inorder together
