Here is a complete explanation for the classic **“Find Minimum in a Rotated Sorted Array”** problem — with **intuition, algorithm breakdown, patterns, code with comments, dry run, and complexity analysis** — perfect for interviews and quick revision.

---

# 🔍 Find Minimum in a Rotated Sorted Array

---

## ✅ Problem Statement

> Given a **rotated sorted array** of **distinct** integers, return the **minimum element**.

📌 The array was originally sorted in ascending order, but was rotated at some unknown pivot:

```java
Input: nums = [4,5,6,7,0,1,2]
Output: 0
```

---

## 🧠 Intuition

Even though the array is rotated, the two parts are still sorted. So we can use **Binary Search** to find the **inflection point** where the rotation occurred — that point is the **minimum** element.

---

## 🔍 How to Identify the Pattern

| Clue from Problem           | Pattern to Use                    |
| --------------------------- | --------------------------------- |
| Array is rotated            | Binary Search in rotated array    |
| Array is sorted in parts    | Binary Search logic still applies |
| Ask for **minimum**         | Inflection point detection        |
| Time complexity requirement | Binary Search → `O(log n)`        |

---

## 🛠️ Concepts & Patterns Used

| Concept             | Why Used                               |
| ------------------- | -------------------------------------- |
| **Binary Search**   | Efficient search in a sorted array     |
| **Modified Binary** | Because array is rotated at pivot      |
| **Two-pointer**     | To narrow down the range (left, right) |

---

## 🔁 Algorithm Strategy

1. Initialize `left = 0`, `right = n - 1`
2. While `left < right`:

    * Find `mid = (left + right) / 2`
    * If `nums[mid] > nums[right]`, minimum must be **right of mid**
    * Else, minimum is at `mid` or **left of mid**
3. Return `nums[left]` when loop ends

---

## ✅ Full Java Code with Comments

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        // Binary search to find the smallest element
        while (left < right) {
            int mid = left + (right - left) / 2;

            // If middle element is greater than rightmost,
            // the minimum is in the right half
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                // Else the minimum is at mid or to the left of mid
                right = mid;
            }
        }

        // After loop, left == right is the index of the smallest element
        return nums[left];
    }
}
```

---

## 🔂 Dry Run

### Input:

```java
nums = [4,5,6,7,0,1,2]
```

### Iterations:

```
left=0, right=6 → mid=3 → nums[3]=7 > nums[6]=2 → left = mid+1 = 4
left=4, right=6 → mid=5 → nums[5]=1 < nums[6]=2 → right = mid = 5
left=4, right=5 → mid=4 → nums[4]=0 < nums[5]=1 → right = mid = 4
left == right → return nums[4] = 0
```

---

## 🧠 Real-Life Use Cases

* Searching in rotated logs
* Finding the earliest version in rotated deployments
* Efficient rotation detection in circular buffers

---

## ⏱️ Time & Space Complexity

| Metric    | Complexity              |
| --------- | ----------------------- |
| **Time**  | `O(log n)`              |
| **Space** | `O(1)` — constant space |

---

## 📌 Summary: Quick Revision Points

* 🔁 Use **binary search** to find the minimum
* ✅ Compare `nums[mid]` vs `nums[right]`
* 🔽 If `nums[mid] > nums[right]`, min is in right half
* 🔼 Else, it’s in left half (including mid)
* ⏱️ Time: `O(log n)`, Space: `O(1)`

---

Let me know if you’d like:

* ✅ Variant with **duplicate elements**
* 🔁 Find **index of rotation**
* 🔄 Search for an element in rotated array
