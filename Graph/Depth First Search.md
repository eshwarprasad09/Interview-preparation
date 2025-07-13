Here’s a **complete guide** to **Depth-First Search (DFS)** — including when and how to use it, step-by-step breakdown, and implementation patterns used in coding interviews.

---

# 🧠 Depth First Search (DFS) – Master Guide

## ✅ What is DFS?

> **DFS** is an algorithm for traversing or searching tree or graph data structures. It starts at the root (or any arbitrary node), explores as far as possible along each branch before backtracking.

---

## 💡 Concepts & Patterns Used

* **Recursion** or **explicit stack**
* **Backtracking** (undo the state when backtracking)
* **Visited Set / Boolean Array** (to avoid cycles in graphs)
* Can be used for:

    * Trees (preorder, inorder, postorder)
    * Graphs (connected components, path finding)
    * Matrix (flood fill, island problems)

---

## 📌 How to Identify DFS Pattern?

* **You must explore all paths** from a starting point (e.g., find all combinations, all paths).
* Tree or Graph traversal where backtracking is needed.
* Matrix traversal (e.g., island counting).
* Recursion is a natural choice, stack simulation possible.

---

## 💻 DFS Template (Graph or Matrix)

```java
void dfs(int node, Set<Integer> visited, Map<Integer, List<Integer>> graph) {
    if (visited.contains(node)) return;

    visited.add(node); // mark as visited

    for (int neighbor : graph.get(node)) {
        dfs(neighbor, visited, graph); // recurse deeper
    }
}
```

---

## ✅ Step-by-Step Breakdown of DFS on a Graph

### Problem: Print all nodes in a graph using DFS

```java
void dfs(int node, boolean[] visited, List<List<Integer>> graph) {
    visited[node] = true;
    System.out.print(node + " ");

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor, visited, graph);
        }
    }
}
```

### Input Graph as Adjacency List:

```
0 -> [1, 2]
1 -> [0, 3]
2 -> [0]
3 -> [1]
```

### Dry Run from node 0:

* Visit 0
* Visit 1
* Visit 3
* Backtrack
* Visit 2

### Output:

`0 1 3 2`

---

## 🧪 DFS on 2D Grid Example (Matrix DFS)

```java
void dfs(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length 
        || grid[i][j] == '0') return;

    grid[i][j] = '0'; // Mark visited

    dfs(grid, i+1, j);
    dfs(grid, i-1, j);
    dfs(grid, i, j+1);
    dfs(grid, i, j-1);
}
```

Used in: **Number of Islands**, **Flood Fill**, **Connected Components**

---

## 🚀 Real-Life Use Cases

* Puzzle solving (e.g., Sudoku)
* Web crawling
* Maze solving
* Game AI (e.g., move generation)
* Social network analysis (friend suggestions)
* Path finding in maps

---

## ⏱ Time and Space Complexity

| Structure | Time Complexity | Space Complexity                           |
| --------- | --------------- | ------------------------------------------ |
| Tree      | O(n)            | O(h) → height of tree                      |
| Graph     | O(V + E)        | O(V) (visited set + recursion stack)       |
| Grid      | O(m × n)        | O(m × n) (visited matrix / implicit stack) |

---

## 🔹 Summary (Quick Revision)

* **Recursive or Stack-based**
* Useful in tree/graph traversal, backtracking, exploring all paths
* Mark nodes visited to avoid cycles
* Time: O(nodes + edges)
* Space: O(depth)

---

## Bonus

Would you like code for:

* Iterative DFS using stack?
* DFS for cycle detection in a graph?
* DFS for finding connected components?
  Let me know — I can explain each in detail with diagrams.
