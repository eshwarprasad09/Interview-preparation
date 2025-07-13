Here’s a complete explanation of how to compute the **Bottom View of a Binary Tree** — step by step, with all required concepts and a working Java implementation.

---

# 🌳 Bottom View of Binary Tree

## ✅ Problem Statement

> The **bottom view** of a binary tree is the set of nodes visible when the tree is **viewed from the bottom**.

For example:

```
        20
       /  \
     8     22
    / \      \
   5  3      25
     / \
    10 14

Bottom view: 5, 10, 3, 14, 25
```

---

## 💡 Concepts & Patterns Used

* **Breadth-First Search (Level Order Traversal)**.
* Use of **Horizontal Distance (HD)** to map nodes.
* **TreeMap** or **HashMap** to keep track of the bottom-most node at each horizontal distance.
* **Queue** to perform level-order traversal with `(node, hd)` pair.

---

## 🔍 Intuition

* Assign `HD = 0` to root.
* For left child, `HD - 1`
* For right child, `HD + 1`
* In **level order**, the latest (bottom-most) node at a particular HD replaces any previous entry.
* At the end, sort the map by HD and return the values.

---

## 💻 Java Code

```java
class Solution {
    class Pair {
        TreeNode node;
        int hd;
        Pair(TreeNode n, int h) {
            node = n;
            hd = h;
        }
    }

    public List<Integer> bottomView(TreeNode root) {
        Map<Integer, Integer> map = new TreeMap<>(); // HD -> Node value
        Queue<Pair> queue = new LinkedList<>();

        queue.offer(new Pair(root, 0));

        while (!queue.isEmpty()) {
            Pair curr = queue.poll();
            TreeNode node = curr.node;
            int hd = curr.hd;

            // Overwrite the value for the same horizontal distance
            map.put(hd, node.val);

            if (node.left != null) queue.offer(new Pair(node.left, hd - 1));
            if (node.right != null) queue.offer(new Pair(node.right, hd + 1));
        }

        return new ArrayList<>(map.values());
    }
}
```

---

## 🧠 Step-by-Step Breakdown

1. Use a queue to perform BFS.
2. Track **horizontal distance (HD)** of each node.
3. For each HD, store the **last node** encountered → Bottom-most.
4. Use `TreeMap` to automatically sort by HD.
5. After BFS, collect values from the map for the bottom view.

---

## 🔁 Dry Run Example

Given Tree:

```
        1
       / \
      2   3
       \   \
        4   5
```

### HD mapping:

| Node | HD | Bottom View      |
| ---- | -- | ---------------- |
| 1    | 0  | 1                |
| 2    | -1 | 2                |
| 3    | 1  | 3                |
| 4    | 0  | 4 (overwrites 1) |
| 5    | 2  | 5                |

✅ Bottom View: `2 4 3 5`

---

## ⏱ Time & Space Complexity

| Metric           | Value                  |
| ---------------- | ---------------------- |
| Time Complexity  | **O(n)**               |
| Space Complexity | **O(n)** (map + queue) |

---

## 📌 Summary (Quick Revision)

* Use **BFS + Horizontal Distance**
* Overwrite value for each HD to get bottom-most node
* Use `TreeMap` for sorted HDs
* Time: O(n), Space: O(n)

---

Would you like the **Top View** or **Vertical Order Traversal** explained next?
