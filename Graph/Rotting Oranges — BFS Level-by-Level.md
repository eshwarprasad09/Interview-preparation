Here's a **complete breakdown** of **Leetcode 994. Rotting Oranges**, including the **intuition**, **BFS pattern**, and full **Java code** with inline comments and analysis.

---

# 🍊 994. Rotting Oranges — BFS Level-by-Level

---

## ✅ Problem Statement

You are given an `m x n` grid where:

* `0` = empty cell
* `1` = fresh orange
* `2` = rotten orange

Each minute:

* Any **fresh orange** adjacent (up/down/left/right) to a **rotten one** becomes **rotten**.

⏱ You must return the **minimum number of minutes** to rot all oranges, or **-1** if impossible.

---

## 🧠 Intuition

This is a **flood fill** / **multi-source BFS** problem.

### Why BFS?

* Rotten oranges spread outward **level by level (minute by minute)**.
* At every time step, multiple oranges may rot others ⇒ use **multi-source BFS** from all rotten oranges at once.

---

## 🔍 Pattern Recognition

| Observation              | Applied Pattern                |
| ------------------------ | ------------------------------ |
| Spread outward in waves  | **Breadth-First Search (BFS)** |
| Multiple start points    | **Multi-source BFS**           |
| Time progresses in steps | **Level-order traversal**      |

---

## ✅ Java Code with Full Inline Explanation

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0) return -1;

        int rows = grid.length, cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>(); // Stores {row, col}
        int freshCount = 0;

        // Step 1: Initialize queue with all initially rotten oranges
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) {
                    queue.offer(new int[]{r, c});
                } else if (grid[r][c] == 1) {
                    freshCount++;
                }
            }
        }

        if (freshCount == 0) return 0; // No fresh oranges

        int minutes = 0;
        int[][] directions = {{-1,0},{1,0},{0,-1},{0,1}}; // Up, Down, Left, Right

        // Step 2: BFS traversal — rotting level by level
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean rottedThisMinute = false;

            for (int i = 0; i < size; i++) {
                int[] pos = queue.poll();
                int r = pos[0], c = pos[1];

                for (int[] dir : directions) {
                    int nr = r + dir[0];
                    int nc = c + dir[1];

                    // If valid and fresh orange
                    if (nr >= 0 && nc >= 0 && nr < rows && nc < cols && grid[nr][nc] == 1) {
                        grid[nr][nc] = 2;       // Make it rotten
                        freshCount--;           // Decrease fresh count
                        rottedThisMinute = true;
                        queue.offer(new int[]{nr, nc}); // Add to queue for next level
                    }
                }
            }

            if (rottedThisMinute) minutes++; // Only increase time if we actually rotted some
        }

        return freshCount == 0 ? minutes : -1; // If all fresh are rotted, return time else -1
    }
}
```

---

## 🔂 Step-by-Step Dry Run

### Input Grid:

```
[ [2,1,1],
  [1,1,0],
  [0,1,1] ]
```

1. Initially, queue contains: `(0,0)` — the only rotten orange.
2. Minute 1: rot (0,1), (1,0) → queue size = 2
3. Minute 2: rot (0,2), (1,1) → queue size = 2
4. Minute 3: rot (2,1) → queue size = 1
5. Minute 4: rot (2,2) → freshCount = 0 ✅

Answer: `4 minutes`

---

## 📚 Concepts & Patterns Used

| Concept               | Why Used                            |
| --------------------- | ----------------------------------- |
| Breadth-First Search  | To simulate minute-by-minute spread |
| Multi-source BFS      | Start from all rotten oranges       |
| Level-order traversal | Each level = 1 minute               |
| Grid traversal        | Adjacency movement in 2D grid       |

---

## 🕓 Time and Space Complexity

| Metric | Complexity                |
| ------ | ------------------------- |
| Time   | `O(m * n)` — scan + BFS   |
| Space  | `O(m * n)` — queue + grid |

Where `m` and `n` are grid dimensions.

---

## 💡 Real-Life Use Cases

* Virus spread modeling in cities.
* Fire propagation in forests.
* BFS in image processing (pixel-based transformations).
* Propagation of news/information in social networks.

---

## 📌 Summary (Quick Revision)

* ✅ Use **multi-source BFS** to simulate spread.
* 🔁 Each **level** in BFS = 1 minute.
* 🧪 If all fresh oranges are rotted → return minutes.
* ❌ If fresh oranges are left unreachable → return `-1`.
* ⏱ Time: `O(m*n)` — each cell visited once.

---

Let me know if you’d like:

* ✅ Python version
* 🧪 Unit test generation
* 🔁 Grid problems using DFS or Dijkstra as well
