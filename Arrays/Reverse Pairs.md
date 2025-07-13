# Reverse Pairs Problem - Full Explanation

## 🔍 Problem Statement

Given an array `nums`, count the number of **reverse pairs** such that:

> For all `(i, j)` with `i < j`, a **reverse pair** is when:
>
> $nums[i] > 2 * nums[j]$

---

## 🧠 Intuition

A brute-force method to check every pair `(i, j)` would be $O(n^2)$, which is too slow for large arrays.

However, the condition $nums[i] > 2 * nums[j]$ suggests a **sorted order** can help — i.e., if we sort part of the array, we can find such pairs more efficiently.

Hence, use **Merge Sort**, which allows us to:

* Count reverse pairs during the merge step
* Sort the array as we go

---

## 💡 Concepts & Patterns Used

* **Merge Sort** (Divide and Conquer)
* **Binary Search-like Counting** (Counting reverse pairs during merge)
* **Two Pointers** (While merging)

---

## 🔎 How to Identify the Pattern

* Counting condition-based pairs with `i < j` is often solvable using **modified merge sort**
* Condition involves comparison like `nums[i] > 2 * nums[j]` => not simple sorting, but counting relations => Divide and Conquer fits well

---

## ✅ Code Explanation (Step-by-Step)

```java
class Solution {
    public int reversePairs(int[] nums) {
        return mergeSort(nums, 0 , nums.length -1);
    }

    public int mergeSort(int[] nums, int left, int right){
        if(left == right) return 0; // base case

        int mid = (left + right) / 2;

        // recursive sort and count
        int count = mergeSort(nums, left, mid) + mergeSort(nums, mid+1, right);

        // Count reverse pairs between two halves
        int j = mid + 1;
        for (int i = left; i <= mid; i++){
            while(j <= right && (long) nums[i] > 2L * nums[j]){
                j++;
            }
            count += j - (mid + 1);
        }

        // Merge two sorted halves
        merge(nums, left, mid, right);

        return count;
    }

    private void merge(int [] nums, int left, int mid, int right){
        List<Integer> temp = new ArrayList<>();
        int i = left, j = mid + 1;

        while(i <= mid && j <= right){
            if (nums[i] <= nums[j]){
                temp.add(nums[i++]);
            } else {
                temp.add(nums[j++]);
            }
        }

        while(i <= mid) temp.add(nums[i++]);
        while(j <= right) temp.add(nums[j++]);

        // Copy sorted part back to original array
        for(int k = left; k <= right; k++){
            nums[k] = temp.get(k - left);
        }
    }
}
```

---

## 🧪 Step-by-Step Dry Run

### Input:

```
nums = [1, 3, 2, 3, 1]
```

### Merge Sort Steps:

* Split: \[1, 3, 2] and \[3, 1]
* Sort \[1, 3, 2] -> \[1], \[3,2] -> count reverse pairs between sorted \[1] and \[2,3]
* For pair (3,1) -> 3 > 2\*1 -> valid => count += 1
* Continue this through merge steps

**Total reverse pairs = 2**

---

## 📅 Real-Life Use Cases

* **Stock trading**: Detecting cases where an older price is significantly higher than a future one.
* **Data anomaly detection**: Finding outliers where a value is more than double of future value
* **Optimizing queries**: Sorting while counting custom conditions

---

## ⏱️ Time and Space Complexity

### Time Complexity:

* Merge sort: O(n log n)
* For every merge step, at most O(n) time to count pairs
* ✅ **Overall: O(n log n)**

### Space Complexity:

* O(n) for the temporary array during merge
* ✅ **Overall: O(n)**

---

## 🔹 Summary (Quick Revision)

* Use **Merge Sort** to divide and conquer
* While merging, count pairs where `nums[i] > 2 * nums[j]`
* Merge halves back while keeping array sorted
* Time: O(n log n), Space: O(n)
* Handles large arrays efficiently

---

## 🌐 Extra

Would you like variations of this problem or to see it solved using **Segment Tree**, **Fenwick Tree (BIT)** or **TreeMap**?
