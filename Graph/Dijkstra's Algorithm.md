Here’s a complete explanation of **Dijkstra’s Algorithm** tailored for **coding interviews** – with step-by-step breakdown, Java code, intuition, how to identify the pattern, and revision bullets in **Markdown format** for easy review.

---

# 🚀 Dijkstra's Algorithm – Step-by-Step Guide

---

## ✅ Problem Statement

Given a **graph with non-negative edge weights**, find the **shortest path** from a **source node** to all other nodes.

> Input: Graph (as adjacency list or matrix), source node
> Output: Shortest distance from source to every node

---

## 🧠 Intuition

Dijkstra is a **greedy algorithm** that expands the **closest unvisited node** from the source node and relaxes (updates) its neighbors.

It ensures that **once a node’s shortest path is known, it never changes**.

---

## 📌 When to Use Dijkstra?

* Graph has **non-negative weights**
* Need shortest path from **one source** to all other nodes
* You want to avoid **Bellman-Ford’s extra time** (which works with negative edges)

---

## 💡 Concepts & Patterns Used

* **Greedy algorithm**
* **PriorityQueue / Min Heap** (for efficient selection of minimum)
* **Relaxation** of distances
* **Visited Set** or checking via distance array

---

## 🧑‍💻 Java Code – Using PriorityQueue

```java
class Solution {
    static class Pair {
        int node, dist;
        Pair(int node, int dist) {
            this.node = node;
            this.dist = dist;
        }
    }

    public int[] dijkstra(int V, List<List<List<Integer>>> adj, int S) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0;

        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.dist - b.dist);
        pq.add(new Pair(S, 0));

        while (!pq.isEmpty()) {
            Pair current = pq.poll();
            int u = current.node;
            int d = current.dist;

            for (List<Integer> neighbor : adj.get(u)) {
                int v = neighbor.get(0);
                int weight = neighbor.get(1);

                if (dist[v] > d + weight) {
                    dist[v] = d + weight;
                    pq.add(new Pair(v, dist[v]));
                }
            }
        }

        return dist;
    }
}
```

---

## 📘 Input Format for Graph

```java
V = 5;
adj = [
  [[1, 2], [4, 1]],     // 0 → 1 (2), 0 → 4 (1)
  [[2, 3]],             // 1 → 2 (3)
  [[3, 6]],             // 2 → 3 (6)
  [],                   // 3 has no neighbors
  [[2, 2], [3, 4]]      // 4 → 2 (2), 4 → 3 (4)
];
```

---

## 🧪 Dry Run

Start at node 0.

* dist\[0] = 0
* Queue: \[(0, 0)]

Expand:

* From 0 → 1 (cost 2), update dist\[1] = 2
* From 0 → 4 (cost 1), update dist\[4] = 1

Now queue: \[(4, 1), (1, 2)]

Process 4 → 2 (2 + 1 = 3), dist\[2] = 3

Continue till all shortest paths are finalized.

---

## ⏱️ Time & Space Complexity

| Operation | Complexity       |
| --------- | ---------------- |
| **Time**  | O((V + E) log V) |
| **Space** | O(V + E)         |

* `V`: number of vertices
* `E`: number of edges

---

## 🔎 How to Identify This Pattern

* Need shortest path **from one node to all others**
* Edge weights are **non-negative**
* Graph input is adjacency list/matrix
* Look for the keywords:

    * "least cost"
    * "shortest distance"
    * "weighted graph"

---

## 🔁 Real-World Use Cases

* Google Maps / GPS Navigation
* Internet routing (least latency paths)
* Game pathfinding (AI agents)
* Network packet delivery optimization

---

## 📝 Summary (Quick Revision)

* ✅ Greedy algorithm
* ✅ Works on **non-negative** weights
* ✅ Uses PriorityQueue (Min Heap)
* ✅ Time: **O((V + E) log V)**
* ✅ Returns shortest distance from a **source node**

---

Would you like:

* A working version for **2D grids**?
* Handling **paths instead of distances**?
* The **Bellman-Ford** version for graphs with negative weights?

Let me know!
