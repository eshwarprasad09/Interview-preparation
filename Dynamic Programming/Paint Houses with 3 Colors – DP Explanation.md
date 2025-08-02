Here's a **complete breakdown** of the classic **“Paint Houses with 3 Colors (Minimum Total Cost)”** problem using **Dynamic Programming (DP)**.

---

# 🏠 Paint Houses with 3 Colors – DP Explanation

> **Problem Statement**
> You are given a 2D array `costs` of size `n x 3`, where `costs[i][j]` is the cost of painting the `i-th` house with color `j` (0: Red, 1: Blue, 2: Green).
> **No two adjacent houses** can be painted with the **same color**.
> Return the **minimum total cost** to paint all houses.

---

## ✅ **Category / Concept**

* **Category**: Dynamic Programming
* **Sub-concept**: DP on Grids / Sequences
* **Pattern**: **DP with choices & constraints**

---

## 🧠 Intuition

At each house, you have 3 choices (Red, Blue, Green), but you **can’t pick the same color as the previous house**.

So, for each house, you:

* Choose the **color with the minimum cost** from the other two previous colors.

---

## 🔍 Pattern Recognition

This follows the **Bottom-Up DP** or **Memoization** pattern with **state transitions**:

* You're solving a **sequence of decisions** (for each house),
* With a **choice restriction** (not the same color as the previous),
* And you want the **minimum cost**.

This is a **classic DP problem**.

---

## ✏️ DP Transition Formula

Let `dp[i][c]` represent the **minimum cost to paint house `i` with color `c`**.

Then:

```text
dp[i][0] = costs[i][0] + min(dp[i-1][1], dp[i-1][2])
dp[i][1] = costs[i][1] + min(dp[i-1][0], dp[i-1][2])
dp[i][2] = costs[i][2] + min(dp[i-1][0], dp[i-1][1])
```

---

## 💻 Java Code with Full Inline Explanation

```java
public class PaintHouses {

    public static int minCost(int[][] costs) {
        int n = costs.length;
        if (n == 0) return 0;

        // Bottom-up DP: we can reuse the costs array as DP
        for (int i = 1; i < n; i++) {
            // If we paint current house red (0),
            // previous house must be blue (1) or green (2)
            costs[i][0] += Math.min(costs[i - 1][1], costs[i - 1][2]);

            // If we paint current house blue (1),
            // previous house must be red (0) or green (2)
            costs[i][1] += Math.min(costs[i - 1][0], costs[i - 1][2]);

            // If we paint current house green (2),
            // previous house must be red (0) or blue (1)
            costs[i][2] += Math.min(costs[i - 1][0], costs[i - 1][1]);
        }

        // Return the minimum of all three color choices for the last house
        return Math.min(costs[n - 1][0], 
               Math.min(costs[n - 1][1], costs[n - 1][2]));
    }

    // Sample usage
    public static void main(String[] args) {
        int[][] costs = {
            {17, 2, 17},
            {16, 16, 5},
            {14, 3, 19}
        };

        System.out.println("Minimum cost to paint all houses: " + minCost(costs));  // Output: 10
    }
}
```

---

## 📦 Sample Input and Dry Run

### Input:

```java
int[][] costs = {
    {17, 2, 17},
    {16, 16, 5},
    {14, 3, 19}
};
```

### Step-by-step table:

| House | Red (0) | Blue (1) | Green (2) |
| ----- | ------- | -------- | --------- |
| 0     | 17      | 2        | 17        |
| 1     | 18      | 33       | 7         |
| 2     | 21      | 6        | 37        |

Final minimum = `min(21, 6, 37)` = **6**

---

## 🪜 Step-by-Step in Layman’s Terms

1. For each house, try each color.
2. When choosing a color, you must **avoid the same color** as the previous house.
3. At each step, remember the **minimum total cost so far** for each color.
4. Continue building the solution from first house to last.

---

## 🏁 Final Output

```
Minimum cost to paint all houses: 10
```

---

## 📌 Summary (Quick Revision)

* Use **DP** with a 2D table `dp[i][j]` = min cost to paint house `i` with color `j`.
* Transition: Add current cost to the **min of other two colors** from the previous house.
* Return min of the last row (last house).

---

## 🔁 Time and Space Complexity

### Time Complexity:

* `O(n)` → One pass through the list of houses.

### Space Complexity:

* `O(1)` → We update costs in place (can use extra `O(n)` DP array if needed).

---

## 💡 Real-Life Use Cases

* **Scheduling tasks** on workers where **no adjacent tasks** can be assigned same resource.
* **Painting walls/rooms** with aesthetics constraint.
* **Timetable scheduling** (no same subject back-to-back).

---

Would you like the **memoized top-down version** or **optimized space** version too?
