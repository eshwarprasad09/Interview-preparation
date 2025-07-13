Here's a **complete step-by-step explanation** and **interview-optimized Java solution** to find the **second highest number in an unsorted array** — without sorting and using only one pass (O(n)).

---

# 🔍 Find the 2nd Highest Number in an Unsorted Array

---

## ✅ Problem Statement

> Given an array of integers (can be positive or negative, unsorted), find the **second largest (distinct)** number.

---

## 🧠 Intuition

Sorting the array would take `O(n log n)` time, but we can do better.

### Key Idea:

* We just need the **first max** and **second max** values.
* Traverse once and **track two variables**:

    * `first` — holds the largest number so far.
    * `second` — holds the second largest.

---

## 🔍 Algorithm Strategy

1. Initialize `first = Integer.MIN_VALUE`, `second = Integer.MIN_VALUE`
2. Loop through the array:

    * If current element > `first`, update `second = first`, then `first = element`
    * Else if element < `first` but > `second`, update `second = element`
3. Return `second` (if it's updated), else return error/default

---

## 🛠️ Java Code with Inline Comments

```java
class Solution {
    public static int secondMax(int[] nums) {
        // Handle edge case: fewer than 2 distinct elements
        if (nums.length < 2) throw new IllegalArgumentException("Need at least 2 numbers");

        int first = Integer.MIN_VALUE;
        int second = Integer.MIN_VALUE;

        for (int num : nums) {
            // Skip if num is same as first to ensure distinct
            if (num == first) continue;

            if (num > first) {
                second = first;
                first = num;
            } else if (num > second) {
                second = num;
            }
        }

        // If second was never updated, array had all same elements
        if (second == Integer.MIN_VALUE) throw new RuntimeException("No second distinct max");

        return second;
    }
}
```

---

## 🔂 Dry Run Example

```java
Input: [10, 5, 20, 8, 20]
first  = -∞ → 10 → 20
second = -∞ → 10 → 10

Final result: 10
```

---

## 📚 Concepts & Patterns Used

| Concept             | Use Case                   |
| ------------------- | -------------------------- |
| Single-pass scan    | Efficient linear traversal |
| Conditional updates | Maintain two top variables |
| Distinct handling   | Avoid duplicates           |

---

## 🧠 Real-Life Use Cases

* Competition rankings (2nd best)
* Analytics dashboards (second highest sale, visit, etc.)
* Second largest bid in auctions

---

## ⏱️ Time & Space Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | `O(n)`     |
| Space  | `O(1)`     |

---

## 📌 Summary: Quick Revision Points

* ✅ Use two variables: `first`, `second`
* 🔁 Update `second` only if the new number < `first` and > `second`
* ❗ Skip duplicates to ensure **distinct values**
* 🧠 Time: `O(n)`, Space: `O(1)`

---

Let me know if you want:

* 🔁 Variant: 3rd highest / kth largest without sorting
* 📊 Min heap approach (useful for top-K problems)
* 💥 Edge-case handling for duplicates and negative numbers
