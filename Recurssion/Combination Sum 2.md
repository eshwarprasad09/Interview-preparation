Here's the full explanation of the `Combination Sum II` problem in **Markdown format**, including all your requested sections:

---

# 🧮 Combination Sum II – Full Explanation

---

## 📘 Problem Statement

Given a collection of **candidates** (which may include **duplicates**) and a target number, find all **unique combinations** where:

* The candidate numbers sum to the target.
* Each number in candidates **may only be used once** in the combination.

> Output must not contain duplicate combinations.

---

## 🧠 Intuition

* Unlike Combination Sum I, **each element may be used once only**.
* We need to **avoid duplicate combinations**, which can occur due to repeated elements in the input.
* **Sorting** helps in identifying and skipping duplicates efficiently.
* Use **backtracking** to explore all valid combinations.

---

## 💡 Concepts & Patterns Used

* **Backtracking**
* **Sorting for Duplicate Skipping**
* **DFS (Depth-First Search) Tree Traversal**
* **Pruning branches** when target becomes negative
* **For-loop recursion** where each element can be used **once**

---

## 🔍 How to Identify the Pattern

| Symptom / Requirement       | Pattern Used                |
| --------------------------- | --------------------------- |
| All combinations of numbers | Backtracking                |
| Can’t reuse the same number | Recursive call with `i + 1` |
| Input contains duplicates   | Sort and skip duplicates    |
| Target sum with constraints | DFS + pruning               |

---

## ✅ Java Code (Step-by-Step Explained)

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates); // sort to handle duplicates
        generateCombos(0, candidates, new ArrayList<>(), result, target);
        return result;
    }

    void generateCombos(int start, int[] nums, List<Integer> curr, List<List<Integer>> result, int target) {
        if (target == 0) {
            result.add(new ArrayList<>(curr)); // Found valid combination
            return;
        }

        if (target < 0) return; // Exceeded the target, backtrack

        for (int i = start; i < nums.length; i++) {
            // Skip duplicates at the same recursion level
            if (i > start && nums[i] == nums[i - 1]) continue;

            curr.add(nums[i]); // Choose current number
            generateCombos(i + 1, nums, curr, result, target - nums[i]); // i+1 → no reuse
            curr.remove(curr.size() - 1); // Undo choice
        }
    }
}
```

---

## 🧪 Dry Run Example

### Input:

```java
candidates = [10,1,2,7,6,1,5]
target = 8
```

### Sorted:

```java
[1, 1, 2, 5, 6, 7, 10]
```

### Result:

```java
[
  [1,1,6],
  [1,2,5],
  [1,7],
  [2,6]
]
```

### Duplicate Skipping:

When `i > start && nums[i] == nums[i-1]`, it means we've already handled this value at the same level.

---

## 🧠 Summary (Quick Revision)

* Use **backtracking** to explore all combinations.
* **Sort** input to handle duplicates.
* Skip duplicate elements using: `if(i > start && nums[i] == nums[i-1]) continue;`
* Move to the next index with `i + 1` since elements can be used only once.
* Stop path if target < 0.

---

## 📊 Time & Space Complexity

| Type                | Value                                    |
| ------------------- | ---------------------------------------- |
| ⏳ Time Complexity   | O(2^n) — Exponential due to combinations |
| 💾 Space Complexity | O(k) — Stack space for recursion         |
| 📥 Output Space     | O(# of valid combinations)               |

---

## 🎯 Real-Life Applications

* Selecting a subset of products to match a price
* Creating meal plans with limited ingredients
* Budget combinations in finance or project planning

---

Would you like to see this implemented with memoization or iterative approach for large inputs?
