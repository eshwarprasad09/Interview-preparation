Here's a complete breakdown of the **Combination Sum** problem in **Markdown format** with all your requested elements:

---

# 🔢 Combination Sum Problem – Full Explanation

---

## 📘 Problem Statement

Given an array of **distinct integers** `candidates[]` and a **target integer**, return **all unique combinations** where the chosen numbers sum to the target.

Each number can be **used unlimited times**.

> Constraints:

* No duplicate combinations in the result
* Same number can be reused multiple times

---

## 🧠 Intuition

* We are exploring all **possible combinations** of numbers that sum up to the target.
* We can **reuse the same number**, so we don’t move the pointer forward in recursive calls after choosing a number.
* When sum > target → stop exploring
* When sum == target → valid combination found

---

## 💡 Concepts & Patterns Used

* **Backtracking**
* **Recursion**
* **DFS (Depth-First Search) on Tree of Choices**
* **Pruning branches** when target becomes negative

---

## 🔍 Pattern Recognition & Algorithm Choice

| Symptom / Pattern                             | Choice                                    |
| --------------------------------------------- | ----------------------------------------- |
| Need all combinations                         | Backtracking                              |
| Can use the same element multiple times       | Recursive call without incrementing index |
| Need to avoid duplicates                      | Start index instead of full loop          |
| Need to **build** and **remove** combinations | Use List and backtracking                 |

---

## ✅ Java Code (Step-by-Step Explained)

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> comb = new ArrayList<>();
        generateCombination(0, candidates, new ArrayList<>(), comb, target);
        return comb;
    }

    void generateCombination(int start, int[] nums, List<Integer> current, List<List<Integer>> comb, int target) {
        if (target == 0) {
            // Found a valid combination
            comb.add(new ArrayList<>(current));
            return;
        }
        if (target < 0) {
            // Overshot the target, backtrack
            return;
        }

        for (int i = start; i < nums.length; i++) {
            // Choose the number
            current.add(nums[i]);
            // Reuse the same number by not incrementing i
            generateCombination(i, nums, current, comb, target - nums[i]);
            // Undo the choice (backtrack)
            current.remove(current.size() - 1);
        }
    }
}
```

---

## 🔁 Dry Run

### Input:

```java
candidates = [2, 3, 6, 7]
target = 7
```

### Execution:

1. Start with 2 → \[2, 2, 2, 2] → overshoot
2. \[2, 2, 3] → valid
3. \[7] → valid
4. \[3, 2, 2] → same as \[2, 2, 3], but starting index avoids duplicate

### Output:

```java
[
  [2, 2, 3],
  [7]
]
```

---

## 📌 Summary – Quick Revision

* Use **backtracking** with recursion
* Keep a running list and target
* Recurse with same index if number can be reused
* Stop path if target < 0
* Add combination to result when target == 0

---

## 🕒 Time & Space Complexity

| Type     | Value                                  |
| -------- | -------------------------------------- |
| ⏳ Time   | Exponential: O(2^T) where T = target   |
| 💾 Space | O(target) for recursion stack & result |

---

## 🧩 Real Life Use Cases

* Coin change problems
* Meal planning under a calorie budget
* Subset generation for budgets or constraints

---

Would you like to also see a **Combination Sum II** version (where elements can be used only once)?
