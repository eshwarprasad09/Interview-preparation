Here's a complete explanation of the **Binary Tree Right Side View** problem in Markdown format:

---

# 🌳 Binary Tree Right Side View

## 🧩 Problem Statement

> Given the root of a binary tree, imagine yourself standing on the **right side** of it, return the **values of the nodes you can see** ordered from top to bottom.

**Leetcode**: [Link to Problem](https://leetcode.com/problems/binary-tree-right-side-view/)

---

## 🧠 Intuition

* From the **right side view**, only the **last node of each level** is visible.
* We can perform **level order traversal (BFS)** and capture the **last node at each level**.

---

## 💡 Concepts & Patterns Used

* **Level Order Traversal (BFS)** using Queue
* **Rightmost element at each level**
* **Binary Tree traversal**
* Can also be done with **DFS + tracking depth**

---

## ✅ Algorithm (BFS Version)

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null) return result;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while(!q.isEmpty()) {
            int levelSize = q.size();
            for(int i = 0; i < levelSize; i++) {
                TreeNode node = q.poll();

                // Add the last node of each level
                if(i == levelSize - 1) {
                    result.add(node.val);
                }

                // Add children to the queue
                if(node.left != null) q.offer(node.left);
                if(node.right != null) q.offer(node.right);
            }
        }

        return result;
    }
}
```

---

## 🔍 Step-by-Step Explanation

1. **Edge Case**: If tree is empty, return empty list.
2. Use a **queue** to do level order traversal (BFS).
3. For every level:

    * Process all nodes in the current level.
    * Capture the value of the **last node** in that level (`i == size - 1`).
4. Push the children of current node (left, then right) into the queue.
5. Repeat until all levels are processed.

---

## 🧪 Dry Run

### Input Tree:

```
        1
       / \
      2   3
       \   \
        5   4
```

### BFS Levels:

* Level 1: \[1] → rightmost = 1 ✅
* Level 2: \[2, 3] → rightmost = 3 ✅
* Level 3: \[5, 4] → rightmost = 4 ✅

**Output**: `[1, 3, 4]`

---

## 📌 Pattern Recognition

* You’re asked to return **per level data** → Think **BFS / Level Order**
* You’re asked to **see nodes from a side** → Usually involves tracking positions per level
* Want **rightmost or leftmost** element per level → Only push that index to result

---

## 🔄 Alternate Approach – DFS with Depth

* Traverse **right-first** DFS.
* At each level, if that depth is not visited yet, add to result.

---

## ⏱ Time & Space Complexity

| Metric              | Value                 |
| ------------------- | --------------------- |
| 🕒 Time Complexity  | O(n)                  |
| 🧠 Space Complexity | O(n) (queue & result) |

> Where `n` is the number of nodes in the tree.

---

## 🛠 Real-Life Applications

* **Tree visualization**: What you’d see from the right or left view.
* **3D scene renderings**: Visibility of objects from a certain side.
* **Optimized scanning**: Finding boundaries or prominent elements.

---

## 📝 Summary for Interview Revision

* Use **BFS** to traverse each level.
* **Capture last node** in every level for right side view.
* Time: O(n), Space: O(n)

---

Would you like the **DFS solution** too?
