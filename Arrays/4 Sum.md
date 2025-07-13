# Four Sum Problem - Full Explanation

## 🔍 Problem Statement

Given an array `nums` and an integer `target`, return all **unique quadruplets** `[a, b, c, d]` such that:

> $a + b + c + d = target$

Return the answer as a list of lists. The solution set must not contain duplicate quadruplets.

---

## 🧠 Intuition

* We need to find **all sets of 4 numbers** that add up to a given target.
* Naively checking all 4-number combinations is $O(n^4)$, which is inefficient.
* So, we use sorting + two-pointer technique inside a double loop.
* To avoid duplicates, sort the array and skip repeated elements using conditions.

---

## 💡 Concepts & Patterns Used

* **Two Pointers** (inside nested loops)
* **Sorting** (to help detect duplicates and guide pointer movement)
* **HashSet** (to ensure unique quadruplets)
* **Avoiding Duplicates** using sorted array and checks

---

## ⚖️ How to Identify the Pattern

* When asked to find multiple elements that sum to a value (2-sum, 3-sum, 4-sum)
* Combination problem with a constraint -> often solved by reducing to smaller problems
* Sorting + Two-pointer + Duplicate Skipping is a common approach

---

## ✅ Code Explanation

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Set<List<Integer>> setAns = new HashSet<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue; // skip duplicates for i

            for (int j = i + 1; j < nums.length; j++) {
                int p1 = j + 1;
                int p2 = nums.length - 1;

                while (p1 < p2) {
                    long sum = (long) nums[i] + (long) nums[j] + (long) nums[p1] + (long) nums[p2];

                    if (sum > target) {
                        p2--;
                    } else if (sum < target) {
                        p1++;
                    } else {
                        List<Integer> quad = Arrays.asList(nums[i], nums[j], nums[p1], nums[p2]);
                        setAns.add(quad);
                        p1++;
                        p2--;
                    }
                }
            }
        }
        ans.addAll(setAns);
        return ans;
    }
}
```

---

## ✏️ Step-by-Step Dry Run

### Input:

```java
nums = [1, 0, -1, 0, -2, 2], target = 0
```

### Sorted:

```java
[-2, -1, 0, 0, 1, 2]
```

### Possible Quadruplets:

```
-2, -1, 1, 2 => 0
-2,  0, 0, 2 => 0
-1,  0, 0, 1 => 0
```

---

## 📅 Real-Life Use Case

* Finding 4 expenses that match a given total budget
* Matching 4 ingredients to meet a specific nutritional value
* In database queries: matching 4 attributes with exact combined value

---

## ⏱️ Time and Space Complexity

### Time Complexity:

* Sorting: O(n log n)
* Four nested traversal using two loops + two-pointer: O(n^3)
* ✅ **Overall: O(n^3)**

### Space Complexity:

* Output list + HashSet: O(k) where k is number of quadruplets
* ✅ **Overall: O(k)**

---

## 🔹 Summary (Quick Revision)

* Sort the array
* Use 2 loops for first 2 elements, then 2 pointers for the rest
* Avoid duplicates with skip conditions and Set
* Time: O(n^3), Space: O(k)
* Common pattern for n-sum problems

---

## 🌐 Bonus

Want to explore the same problem using **backtracking** or using a **Map-based approach**?
