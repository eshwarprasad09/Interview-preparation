Here's a complete explanation of **Topological Sort**, tailored for interviews, with step-by-step reasoning, intuition, algorithms, patterns, and Java code—all in **Markdown** format for easy study and revision.

---

# 📚 Topological Sort – Explained for Interviews

---

## ✅ What is Topological Sort?

> Topological Sorting of a **Directed Acyclic Graph (DAG)** is a **linear ordering** of its vertices such that for every directed edge `u → v`, vertex `u` comes **before** `v` in the ordering.

📌 Only valid for **DAGs** (Directed Acyclic Graphs)

---

## 🧠 Intuition

Imagine you're scheduling tasks:

* Some tasks must be done **before** others.
* E.g., In a course schedule, **Course A → Course B** means you must take **A before B**.

Topological sort finds the correct order to perform such tasks.

---

## 🔍 Real-Life Examples

* **Course Schedule** (prerequisite structure)
* **Build Systems** (dependency order)
* **Compilation Order** (order of modules/files)
* **Package Managers** (install dependencies first)

---

## 💡 Concepts & Patterns Used

* **Graph Traversal**
* **DFS + Stack**
* **Kahn’s Algorithm (BFS + Indegree array)**
* **Cycle Detection** (detect invalid input)

---

## 📌 Two Ways to Implement Topological Sort

### 1. **DFS-Based Topological Sort**

* Traverse the graph in DFS manner.
* Push node to a **stack** after exploring its children.
* Reverse the stack for result.

### 2. **Kahn's Algorithm (BFS + Indegree)**

* Count indegree of all nodes.
* Add all 0-indegree nodes to queue.
* While queue not empty:

    * Remove a node
    * Reduce indegree of its neighbors
    * Add new 0-indegree nodes to queue

---

## 🧑‍💻 Java Code – DFS Approach

```java
class Solution {
    public int[] topoSort(int V, List<List<Integer>> adj) {
        boolean[] visited = new boolean[V];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited, stack);
            }
        }

        int[] result = new int[V];
        int i = 0;
        while (!stack.isEmpty()) {
            result[i++] = stack.pop();
        }
        return result;
    }

    void dfs(int node, List<List<Integer>> adj, boolean[] visited, Stack<Integer> stack) {
        visited[node] = true;
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor, adj, visited, stack);
            }
        }
        stack.push(node); // push after exploring all neighbors
    }
}
```

---

## 🧑‍💻 Java Code – Kahn’s Algorithm (BFS)

```java
class Solution {
    public int[] topoSort(int V, List<List<Integer>> adj) {
        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int neighbor : adj.get(i)) {
                indegree[neighbor]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) q.offer(i);
        }

        int[] result = new int[V];
        int index = 0;

        while (!q.isEmpty()) {
            int node = q.poll();
            result[index++] = node;

            for (int neighbor : adj.get(node)) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    q.offer(neighbor);
                }
            }
        }

        return result;
    }
}
```

---

## 🧪 Dry Run Example

### Graph:

```
V = 6
Edges:
5 → 0  
5 → 2  
4 → 0  
4 → 1  
2 → 3  
3 → 1
```

Topological Sort Output (One of the valid orders):

```
5 4 2 3 1 0
```

---

## 🔬 Time & Space Complexity

| Approach   | Time Complexity | Space Complexity |
| ---------- | --------------- | ---------------- |
| DFS        | O(V + E)        | O(V + E)         |
| Kahn (BFS) | O(V + E)        | O(V + E)         |

Where:

* `V` = vertices
* `E` = edges

---

## 🧩 Summary (Quick Revision)

* Topological Sort is for **DAGs only**
* It gives a valid order of tasks/dependencies
* Two main approaches:

    * DFS + Stack
    * Kahn's (Indegree + Queue)
* Time Complexity: **O(V + E)**
* Used in scheduling, compiler design, dependency resolution

---

Would you like:

* **Cycle detection in DAGs**
* **Topological Sort with course schedule problem**
* **Code in other languages or more dry runs?**

Let me know!
