Here’s the complete explanation of the **Three Sum** problem solution in well-structured **Markdown** format:

---

# 🔍 3Sum Problem: Find Unique Triplets That Sum to Zero

---

## 💬 Problem Statement

Given an integer array `nums`, return **all the unique triplets** `[nums[i], nums[j], nums[k]]` such that:

* `i != j`, `i != k`, `j != k`
* `nums[i] + nums[j] + nums[k] == 0`

Return the solution set **without duplicates**.

---

## 💡 Intuition & Core Idea

We want to find **three numbers** that add up to **0**.
This can be broken into:

> Fix one number `a`, and find two numbers `b` and `c` such that `b + c = -a`.

This reduces the 3Sum problem into a **2Sum problem** with a fixed target.

We:

* Sort the array to simplify duplicate handling
* Fix one element (`a`) in each iteration
* Use **two pointers** to find the pair `b` and `c`

---

## ⚙️ Code Walkthrough (Step-by-Step)

```java
class Solution {
    List<List<Integer>> res = new ArrayList(); // Final list of triplets

    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums); // Step 1: Sort the array

        for (int i = 0; i < nums.length; i++) {
            // Step 2: Avoid duplicate 'a'
            if (i == 0 || nums[i - 1] != nums[i]) {
                twoSumSorted(i + 1, nums.length - 1, nums, 0 - nums[i]); // Step 3: Find twoSum with target = -a
            }
        }
        return res;
    }

    void twoSumSorted(int i, int j, int[] nums, int target) {
        int a1 = nums[i - 1]; // This is our fixed 'a'

        while (i < j) {
            if (nums[i] + nums[j] > target) {
                j--; // Decrease sum
            } else if (nums[i] + nums[j] < target) {
                i++; // Increase sum
            } else {
                // Found a valid triplet
                List<Integer> list = new ArrayList();
                list.add(a1);
                list.add(nums[i]);
                list.add(nums[j]);
                res.add(list);

                // Step 4: Skip duplicate b and c
                while (i < j && nums[i] == nums[i + 1]) i++;
                while (i < j && nums[j] == nums[j - 1]) j--;
                i++; j--;
            }
        }
    }
}
```

---

## 🧠 Concepts & Patterns Used

* **Sorting** to handle duplicates and enable two-pointer technique
* **Two Pointers** to find pairs for 2Sum in `O(n)` time
* **Hashing Avoided**: Instead of using HashSet, duplicates are handled via sorted array
* **Reduction**: 3Sum → 2Sum with target `-a`

---

## 🔍 How to Identify the Pattern

| Situation                        | Clue                                                   |
| -------------------------------- | ------------------------------------------------------ |
| Triplets required from array     | Use nested loop or reduce to 2Sum                      |
| No duplicate triplets allowed    | Sorting helps in deduplication                         |
| Time constraint (avoid `O(n^3)`) | Use sorting + two pointers for better than brute force |

---

## ✅ Summary (Quick Revision)

* Sort array
* Loop with index `i` to fix first element
* Apply **two-pointer** technique for remaining pair
* Skip duplicates for all three values (`a`, `b`, `c`)
* Store valid triplets in result list

---

## 🔁 Dry Run (Step-by-Step)

### Input: `[-1, 0, 1, 2, -1, -4]`

1. Sorted: `[-4, -1, -1, 0, 1, 2]`
2. Fix `a = -4`, look for `b + c = 4` → No match
3. Fix `a = -1`, look for `b + c = 1`

    * Found `0 + 1 = 1` → `[-1, 0, 1]`
    * Skip duplicate `-1`
4. Fix `a = 0`, look for `b + c = 0`

    * Found `-1 + 1 = 0` → `[0, -1, 1]` (already included)
5. Final answer: `[[-1, -1, 2], [-1, 0, 1]]`

---

## 💼 Real-Life Use Cases

* **Recommendation Systems**: Finding triplets of user behavior patterns
* **Finance**: Triangular arbitrage detection
* **Debugging Logs**: Identifying patterns of three interrelated events

---

## ⏱️ Time & Space Complexity

| Metric   | Value                                                 |
| -------- | ----------------------------------------------------- |
| 🕒 Time  | `O(n^2)` (due to outer loop + two-pointer inner loop) |
| 💾 Space | `O(1)` (excluding output list)                        |

---

## 📌 Final Notes

* Sorting simplifies both **searching** and **duplicate handling**
* Avoid brute force (`O(n^3)`) using **Two Sum optimization**
* Be mindful of edge cases: empty array, duplicates, all zeroes

---

Let me know if you'd like the same explanation in **recursive**, **brute-force**, or **HashSet** variant.
