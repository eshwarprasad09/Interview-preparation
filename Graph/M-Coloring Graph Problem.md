Here's a detailed breakdown of the **M-Coloring Graph Problem**, including full explanation, code, algorithm intuition, and category.

---

# 🎨 M-Coloring Graph Problem – Complete Guide

> **Problem**: Given an undirected graph and `M` colors, check if the graph can be colored such that no two adjacent vertices have the same color.

---

## ✅ **Category / Concept**

* **Topic**: Graph Theory
* **Sub-category**: **Backtracking**
* **Patterns**:

    * Backtracking
    * Graph coloring
    * Constraint satisfaction

---

## 🧠 Intuition

* You are given a graph (as an adjacency matrix or list).
* You have to assign one of `M` colors to each node such that **no two adjacent nodes share the same color**.
* Try assigning a color to a node, and recursively try for the next node.
* If at any point, no color can be assigned (conflict with adjacent nodes), **backtrack** and try a different color.

---

## 🔍 How to Identify This as a Backtracking Problem?

* The problem involves **multiple choices** (M colors for N nodes).
* There are **constraints** (adjacent nodes can't share colors).
* You need to **explore all possibilities** and **backtrack** when stuck.
* This fits the **backtracking pattern**: "Try → Check → Backtrack → Repeat".

---

## 🔁 Concepts & Patterns Used

* **Backtracking**
* **Graph traversal**
* **Recursion**
* **Adjacency matrix** for edge representation

---

## 📌 Summary (For Quick Revision)

* Try assigning each color to the current node.
* Check if assignment is safe (no adjacent node has the same color).
* Recur for next node.
* If all nodes are colored without conflict → return true.
* Else backtrack.

---

## 💻 Full Java Code with Step-by-Step Inline Comments

```java
public class MColoring {

    // Function to check if current color assignment is safe
    private static boolean isSafe(int node, int[] color, boolean[][] graph, int n, int col) {
        for (int i = 0; i < n; i++) {
            // Check adjacent node
            if (graph[node][i] && color[i] == col) {
                return false; // adjacent node has same color
            }
        }
        return true;
    }

    // Recursive function to try coloring the graph
    private static boolean solve(int node, int[] color, int m, int n, boolean[][] graph) {
        if (node == n) {
            return true; // All nodes are successfully colored
        }

        // Try all m colors for current node
        for (int col = 1; col <= m; col++) {
            if (isSafe(node, color, graph, n, col)) {
                color[node] = col; // Assign color

                // Recursively assign color to next node
                if (solve(node + 1, color, m, n, graph)) {
                    return true;
                }

                // Backtrack if coloring doesn't lead to solution
                color[node] = 0;
            }
        }

        return false; // No color possible for this node
    }

    // Main function to initialize and solve the problem
    public static boolean graphColoring(boolean[][] graph, int m, int n) {
        int[] color = new int[n]; // Color array initialized to 0 (no color)
        return solve(0, color, m, n, graph);
    }

    // Example usage
    public static void main(String[] args) {
        int n = 4, m = 3;
        boolean[][] graph = {
            {false, true, true, true},
            {true, false, true, false},
            {true, true, false, true},
            {true, false, true, false}
        };

        System.out.println(graphColoring(graph, m, n));  // Output: true
    }
}
```

---

## 🧠 Step-by-Step Dry Run

Input:

```java
n = 4
m = 3
graph = {
  {F, T, T, T},
  {T, F, T, F},
  {T, T, F, T},
  {T, F, T, F}
}
```

1. Start with node 0

    * Try color 1 → safe
2. Move to node 1

    * Color 1 not safe (adjacent to node 0)
    * Try color 2 → safe
3. Move to node 2

    * Colors 1 and 2 not safe (adjacent to 0 and 1)
    * Try color 3 → safe
4. Move to node 3

    * Try color 1 → not safe (adjacent to 0)
    * Try color 2 → safe

✅ All nodes colored: `[1, 2, 3, 2]` → return `true`

---

## 🪜 Step-by-Step in Layman’s Terms

1. Start with the first house (node) → give it a paint color (color 1).
2. Move to the second house → can't paint with the same color as the first, try next.
3. Keep moving forward.
4. If stuck (no valid color left), go back (backtrack) and try a different color for the previous house.
5. Repeat until all houses are painted properly or no way to do it.

---

## 📦 Real-Life Use Case

* **Map coloring**: No two adjacent countries should have the same color.
* **Scheduling**: Assign time slots to tasks where some tasks can't run in parallel (adjacency = conflict).
* **Register allocation** in compilers.

---

## ⏱️ Time & Space Complexity

### Time Complexity:

* Worst-case: `O(m^n)` where:

    * `m` = number of colors
    * `n` = number of vertices

Because we try all `m` colors for each of `n` nodes.

### Space Complexity:

* `O(n)` for the color array
* `O(n)` for recursion stack

---

## 📘 How to Choose This Algorithm?

* Ask yourself:

    * Are there **choices** to make for each step? → Yes
    * Are there **constraints** to satisfy? → Yes
    * Do we need to explore **all combinations**? → Yes
    * Does it need **rollback (backtrack)** on failure? → Yes

→ It's a **backtracking problem**.

---

## ✅ Conclusion

* M-coloring is a classic **backtracking + constraint satisfaction** problem.
* Useful for problems involving **choices under restrictions**.
* Practice similar problems like:

    * N-Queens
    * Sudoku solver
    * Crossword filler

---

Would you like me to generate **Python or C++ version** of this too?
