Here's a complete guide to **Breadth-First Search (BFS)** for interviews, including intuition, when to use, patterns, dry run, and complexities in **Markdown** format.

---

# 🔍 Breadth-First Search (BFS) – Master Guide

---

## ✅ What is BFS?

> **Breadth-First Search (BFS)** is an algorithm for traversing **level by level** or **layer by layer** in graphs or trees.
> It uses a **Queue** (FIFO) to explore neighbors before going deeper.

---

## 🧠 Intuition Behind BFS

* Start at the root or source node.
* Visit all immediate neighbors first.
* Then move to their neighbors.
* Continue this way in a **level-order** fashion.

**Used when:**

* Finding the shortest path in unweighted graphs.
* Layer-wise traversal is required (e.g., Tree Level Order).
* First match or closest match is desired.

---

## 💡 Concepts & Patterns Used

* **Queue Data Structure** (First-In-First-Out)
* **Visited Set / Boolean Array**
* Common in:

    * Graphs (shortest path)
    * Trees (level order traversal)
    * Matrix (multi-directional movement)

---

## 🧩 How to Identify BFS Pattern?

Choose **BFS** when:

* You need the **shortest path** or **minimum moves**.
* Problem mentions **levels**, **steps**, **distance**.
* Requires **simultaneous exploration** in all directions (e.g., rot propagation, multi-source BFS).

---

## 🧪 Code Template (Graph BFS)

```java
void bfs(int startNode, Map<Integer, List<Integer>> graph) {
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> visited = new HashSet<>();

    queue.add(startNode);
    visited.add(startNode);

    while (!queue.isEmpty()) {
        int current = queue.poll();
        System.out.print(current + " ");

        for (int neighbor : graph.get(current)) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.add(neighbor);
            }
        }
    }
}
```

---

## 🧠 BFS on Tree (Level Order Traversal)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < size; i++) {
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

---

## 🔁 Step-by-Step Dry Run

Graph:

```
0 -> [1, 2]
1 -> [0, 3]
2 -> [0]
3 -> [1]
```

**BFS from node 0:**

* Queue: \[0]
* Visit 0 → Queue: \[1, 2]
* Visit 1 → Queue: \[2, 3]
* Visit 2 → Queue: \[3]
* Visit 3 → Done

**Output:** `0 1 2 3`

---

## 🎯 Real-Life Use Cases

* Shortest path in maps (e.g., Google Maps)
* Social networks (e.g., friend suggestion)
* Web crawling
* Networking (BFS in routing protocols)
* AI – path finding in games
* Spread simulation (rot spread, fire, infection)

---

## ⏱️ Time and Space Complexity

| Problem Type | Time Complexity | Space Complexity |
| ------------ | --------------- | ---------------- |
| Graph        | O(V + E)        | O(V)             |
| Tree         | O(n)            | O(n)             |
| Grid         | O(m × n)        | O(m × n)         |

---

## 🧷 Summary (Quick Revision)

* **Uses Queue** to explore neighbors first
* Best for **shortest path** problems in unweighted graphs
* Common in **level order**, **step count**, **multi-source** problems
* Time: O(V + E) for graphs
* Space: O(V)

---

## 📦 Bonus BFS Variants

Let me know if you want code/guide for:

* BFS for shortest path in grid
* Multi-source BFS (like Rotten Oranges 🍊)
* Word Ladder using BFS
* BFS with level tracking

I can prepare a detailed explanation for each!
