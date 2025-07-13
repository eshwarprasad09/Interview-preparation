````markdown
# 🚶‍♂️ Leetcode: Unique Paths - Detailed Explanation

**Problem Link:** [Unique Paths](https://leetcode.com/problems/unique-paths/description/)

---

## 🔍 Problem Statement

You are given a `m x n` grid. A robot starts at the **top-left corner** (0,0) and wants to reach the **bottom-right corner** (m-1,n-1). The robot can only move **right** or **down**.

🧠 **Goal:** Count how many unique paths the robot can take to reach the destination.

---

## 🧠 Intuition

Every path to the bottom-right is just a combination of **moves to the right** and **moves down**.

To go from `(0, 0)` to `(m-1, n-1)`, you need to make:

- `(m-1)` down moves
- `(n-1)` right moves

So, the total number of moves = `(m - 1) + (n - 1)` = `m + n - 2`

Out of these, choose any `(m - 1)` moves to be *downs* (the rest will be *rights*), or vice versa.

This becomes a **combinatorics** problem:  
**Number of Unique Paths** = C(m+n−2, m−1) = (m+n−2)! / [(m−1)! * (n−1)!]

---

## ✅ Solution 1: Combinatorics (Best for large grid)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        long res = 1;
        for(int i = 1; i <= m - 1; i++) {
            res = res * (n - 1 + i) / i;
        }
        return (int)res;
    }
}
````

### 🔧 Time & Space Complexity

* Time: O(m)
* Space: O(1)

---

## ✅ Solution 2: Dynamic Programming (Tabulation)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];

        // 1st row and 1st col will have only 1 path
        for(int i = 0; i < m; i++) dp[i][0] = 1;
        for(int j = 0; j < n; j++) dp[0][j] = 1;

        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }
}
```

### 🔧 Time & Space Complexity

* Time: O(m × n)
* Space: O(m × n)

---

## ✅ Solution 3: DP with Space Optimization (1D Array)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[j] = dp[j] + dp[j - 1];
            }
        }

        return dp[n - 1];
    }
}
```

### 🔧 Time & Space Complexity

* Time: O(m × n)
* Space: O(n)

---

## 🧩 Concepts & Patterns Used

* **Dynamic Programming**
* **Combinatorics**
* **Bottom-up Tabulation**
* **1D Space Optimization**

---

## 🔍 How to Identify the Pattern

* Grid-based movement where direction is constrained → **DP or combinatorics**
* If you see problems with count of distinct paths → Think **DP**
* If you’re only allowed to move right and down → **Subproblem overlap** → DP fits well

---

## ✅ Summary in Bullet Points

* Total paths = (m+n-2 choose m-1) → **Combinatorics**
* Can solve using **DP** (2D or 1D)
* DP recurrence: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`
* **Base cases**: first row and first column = 1

---

## 🧪 Step-by-Step Dry Run

### Input:

```
m = 3, n = 3
```

Grid:

```
[1, 1, 1]
[1, 2, 3]
[1, 3, 6]
```

Final answer = `6` unique paths

---

## 🧰 Real-Life Use Cases

* **Navigation systems** in grid-based games
* **Robotics path planning**
* **Grid-based optimization** (manufacturing plants, delivery paths)
* **Maze traversal** (simplified)

---

Let me know if you'd like to generate a **PDF version**, visualize paths with **diagrams**, or practice **variations** like obstacles in the grid 🚀

```
```
