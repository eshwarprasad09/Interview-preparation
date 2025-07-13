Here's a **complete guide** to solving the classic **"Merge M Sorted Arrays"** problem — one of the most common problems in system design, data engineering, and interviews at companies like Google, Amazon, and Microsoft.

---

# 🔄 Merge M Sorted Arrays

---

## ✅ Problem Statement

> You are given `M` sorted arrays (each of size `N`, or variable length). Merge them into a single **sorted array**.

📌 Example:

```text
Input:
[[1, 4, 5],
 [1, 3, 4],
 [2, 6]]

Output:
[1, 1, 2, 3, 4, 4, 5, 6]
```

---

## 🧠 Intuition

This is a generalization of **merging two sorted arrays**.

### Naive Approach:

* Merge one by one using `Arrays.sort()` or `Collections.sort()`
* Time: `O(n log n)`, inefficient if M and N are large

### Optimal Approach:

Use a **Min Heap (PriorityQueue)** to always get the **smallest current element** from the arrays.

---

## 🔍 How to Identify the Pattern

| Clue from Problem              | Pattern to Use         |
| ------------------------------ | ---------------------- |
| Merge sorted structures        | **Heap (K-way merge)** |
| Always extract minimum element | **Min Heap**           |
| Streaming/merging sorted data  | **Priority Queue**     |

---

## ✅ Java Code Using Min Heap (PriorityQueue)

```java
import java.util.*;

class Solution {
    // Custom class to track element and its origin
    static class Node {
        int val;
        int row;  // which array
        int col;  // index within that array

        Node(int val, int row, int col) {
            this.val = val;
            this.row = row;
            this.col = col;
        }
    }

    public static List<Integer> mergeKSortedArrays(List<List<Integer>> arrays) {
        PriorityQueue<Node> minHeap = new PriorityQueue<>(Comparator.comparingInt(n -> n.val));
        List<Integer> result = new ArrayList<>();

        // Step 1: Insert first element of each array into minHeap
        for (int i = 0; i < arrays.size(); i++) {
            if (!arrays.get(i).isEmpty()) {
                minHeap.offer(new Node(arrays.get(i).get(0), i, 0));
            }
        }

        // Step 2: Extract min and insert next from same array
        while (!minHeap.isEmpty()) {
            Node node = minHeap.poll();
            result.add(node.val);

            int nextCol = node.col + 1;
            if (nextCol < arrays.get(node.row).size()) {
                int nextVal = arrays.get(node.row).get(nextCol);
                minHeap.offer(new Node(nextVal, node.row, nextCol));
            }
        }

        return result;
    }
}
```

---

## 🔂 Dry Run Example

### Input:

```java
[
 [1, 4, 5],
 [1, 3, 4],
 [2, 6]
]
```

### Execution Steps:

1. Add first elements: `1 (arr0)`, `1 (arr1)`, `2 (arr2)` → heap = \[1, 1, 2]
2. Poll 1 → add 4 from arr0 → heap = \[1, 2, 4]
3. Poll 1 → add 3 from arr1 → heap = \[2, 4, 3]
4. Poll 2 → add 6 from arr2 → heap = \[3, 4, 6]
5. And so on…

Final Output: `[1, 1, 2, 3, 4, 4, 5, 6]`

---

## ⏱️ Time and Space Complexity

| Metric         | Complexity                                                    |
| -------------- | ------------------------------------------------------------- |
| Time           | `O(N * log M)` where N = total elements, M = number of arrays |
| Space (heap)   | `O(M)`                                                        |
| Space (output) | `O(N)`                                                        |

---

## 📚 Concepts & Patterns Used

| Concept/Pattern  | Use Case                         |
| ---------------- | -------------------------------- |
| Min Heap         | Efficient minimum extraction     |
| Greedy Selection | Always select the smallest next  |
| K-way Merge      | Standard divide-and-conquer task |

---

## 🧠 Real-Life Use Cases

* Merging multiple sorted logs
* K-way merge in external sorting (big data)
* Merging streaming time-series data
* File merging in database engines

---

## 📌 Summary: Quick Revision Points

* ✅ Use a **Min Heap** to get the next smallest element
* 🔁 Insert the next element from same array after poll
* ⏱️ Time: `O(N log M)`, Space: `O(M)` (heap)
* ⚡ Handles different lengths of arrays
* 🧠 Use custom class or tuple to track source of element

---

Let me know if you want:

* ✅ Python version of this code
* 🔁 Iterative merge of 2 arrays at a time (slower)
* 📊 Merge sorted **linked lists** version (Leetcode #23)
