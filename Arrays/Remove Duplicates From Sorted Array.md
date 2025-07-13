Here’s a full, structured explanation of the **Remove Duplicates from Sorted Array** problem and the solution you've shared — explained step-by-step with **intuition**, **patterns**, **dry run**, **real-life use case**, and **complexity analysis** in **Markdown** format:

---

# 🚫 Remove Duplicates from Sorted Array

---

## 💬 Problem Statement

Given a **sorted integer array `nums`**, remove the **duplicates in-place** such that each element appears only once and return the new length.

> Do not allocate extra space for another array. You must do this by modifying the input array in-place with `O(1)` extra memory.

---

## 💡 Intuition

Since the array is **already sorted**, **duplicates are adjacent**.
This allows us to use the **two-pointer technique**:

* `i` keeps track of the **position of the last unique element**
* `j` scans through the array to **find the next unique element**

When `nums[j] != nums[i]`, it means we've found a **new unique element**, so we:

* Move `i` forward
* Copy the new value to `nums[i]`

---

## ✅ Final Code

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0; // Pointer to place next unique element

        for (int j = 0; j < nums.length; j++) {
            if (nums[i] != nums[j]) {
                i++;              // Move i to next position
                nums[i] = nums[j]; // Place the unique element
            }
        }

        return i + 1; // Length is index + 1
    }
}
```

---

## 📌 Concepts & Patterns Used

* **Two Pointer Technique**
* **In-place Array Manipulation**
* **Sorted Array Optimization** (since duplicates are together)

---

## 🤔 How to Identify the Pattern / Choose the Algorithm

| Condition                      | Pattern to Apply                                  |
| ------------------------------ | ------------------------------------------------- |
| Array is **sorted**            | Use **two pointers**                              |
| Remove **duplicates in-place** | Modify the array using **index-based operations** |
| **O(1)** extra memory required | Avoid using extra space (like Set/List)           |

---

## ✨ Summary (Quick Revision)

* Use two pointers `i` and `j`
* `i` tracks the **position of the last unique** element
* `j` traverses the array
* If `nums[i] != nums[j]`, move `i` forward and **copy** `nums[j]` to `nums[i]`
* Return `i + 1` as the new length

---

## 🔁 Step-by-Step Dry Run

### Input: `nums = [0, 0, 1, 1, 2, 3, 3]`

| Step | `i` | `j` | `nums[i]` | `nums[j]` | Action                 |
| ---- | --- | --- | --------- | --------- | ---------------------- |
| 1    | 0   | 0   | 0         | 0         | same — skip            |
| 2    | 0   | 1   | 0         | 0         | same — skip            |
| 3    | 0   | 2   | 0         | 1         | `i++ → 1`, `nums[1]=1` |
| 4    | 1   | 3   | 1         | 1         | same — skip            |
| 5    | 1   | 4   | 1         | 2         | `i++ → 2`, `nums[2]=2` |
| 6    | 2   | 5   | 2         | 3         | `i++ → 3`, `nums[3]=3` |
| 7    | 3   | 6   | 3         | 3         | same — skip            |

🔹 **Final array (first part)**: `[0, 1, 2, 3, ...]`
🔹 **Return value**: `4`

---

## 💼 Real-Life Use Case

* **Email deduplication** from sorted mailing lists
* Removing duplicate IDs from **sorted log files**
* Data preprocessing before model training

---

## ⏱️ Time & Space Complexity

| Metric   | Complexity        |
| -------- | ----------------- |
| 🕒 Time  | `O(n)`            |
| 💾 Space | `O(1)` (in-place) |

---

## ✅ Output

* Modified input array up to returned length: contains only **unique elements**
* Return the **new length** `i + 1`

---

Let me know if you want the **extended version** that also returns the new array or needs explanation for an **unsorted array** version using a `Set`.
