# Longest Consecutive Sequence - Full Explanation

## 🔍 Problem Statement

Given an unsorted array of integers `nums`, return the **length of the longest consecutive elements sequence**.

> Consecutive means numbers in order like 1,2,3 or 100,101,102 without gaps or duplicates in between.

---

## 🧠 Intuition

* To find the longest **consecutive sequence**, we need numbers that appear one after the other without gaps.
* The simplest method is to **sort** the array so all numbers line up, and then count sequences by comparing neighbors.
* Skip duplicates, and reset count if a gap is found.

---

## 💡 Concepts & Patterns Used

* **Sorting** to bring numbers in order.
* **Linear Scan** to count consecutive values.
* **Edge Case Handling** (duplicate numbers, empty input).

---

## 🔎 How to Identify the Pattern

* When the problem involves order (like consecutive numbers), **sorting** is often a good starting point.
* If asked for the **longest sequence**, it suggests **sliding window** or **greedy counting**.

---

## ✅ Code Explanation (Step-by-Step)

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        int count = 1;         // max count seen so far
        int currCount = 1;     // count of current sequence

        // sort the array
        Arrays.sort(nums);

        // handle edge cases
        if(nums.length == 1) return 1;
        if(nums.length == 0) return 0;

        int curr = nums[0];

        for(int i = 1; i < nums.length; i++) {
            if(curr + 1 == nums[i]) {
                // continue the sequence
                currCount++;
                curr = nums[i];
            } else if(curr == nums[i]) {
                // skip duplicates
                continue;
            } else {
                // sequence broken, reset current count
                curr = nums[i];
                count = Math.max(count, currCount);
                currCount = 1;
            }
        }

        // in case the longest sequence ends at the last element
        return Math.max(count, currCount);
    }
}
```

---

## 🧪 Step-by-Step Dry Run

### Input:

```
nums = [100, 4, 200, 1, 3, 2]
```

### Sorted:

```
[1, 2, 3, 4, 100, 200]
```

### Processing:

* 1 -> 2 => currCount = 2
* 2 -> 3 => currCount = 3
* 3 -> 4 => currCount = 4
* 4 -> 100 => break; count = 4; currCount = 1
* 100 -> 200 => break; count = 4

**Output:** `4`

---

## 🌐 Real-Life Use Cases

* **Activity tracking**: Finding longest streak of daily logins or exercise.
* **Finance**: Detecting longest bullish/bearish run in stock prices.
* **Monitoring**: Longest uptime or consecutive healthy signals in system monitoring.

---

## ⏱️ Time & Space Complexity

### Time Complexity:

* Sorting = O(n log n)
* Traversal = O(n)
* ✅ **Overall: O(n log n)**

### Space Complexity:

* O(1) extra space (in-place sorting)
* ✅ **Overall: O(1)**

---

## 🔹 Summary (Quick Revision)

* Sort the array to bring numbers in sequence.
* Traverse the sorted array and count consecutive numbers.
* Skip duplicates.
* Reset count if a break is found.
* Return the maximum consecutive count.
* Time: O(n log n), Space: O(1)

---

## 🚀 Optimization Suggestion

If allowed to use extra space, a **HashSet** based solution can bring time complexity down to **O(n)** without sorting. Would you like that version as well?
