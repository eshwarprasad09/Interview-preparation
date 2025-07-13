Here’s a **complete Markdown-formatted explanation** of the **Dutch National Flag Algorithm**, including:

* ✅ Problem statement
* ✅ Step-by-step algorithm
* ✅ Intuition & real-life analogy
* ✅ Code with comments
* ✅ Pattern recognition
* ✅ Summary for quick revision

---

```markdown
# 🇳🇱 Dutch National Flag Algorithm - Complete Guide

---

## 📘 Problem Statement

Given an array containing only **0s, 1s, and 2s**, sort it **in-place** so that all 0s come first, followed by all 1s, then all 2s.

> This problem is often asked in coding interviews by companies like Google, Amazon, and Microsoft.

---

## 🎯 Objective

Transform an unsorted array like:

```

\[2, 0, 1, 2, 1, 0]

```

Into:

```

\[0, 0, 1, 1, 2, 2]

````

⚠️ You cannot use a sorting function or extra arrays — must be **in-place** and in **one pass** (O(n) time and O(1) space).

---

## 🧠 Intuition (Why It’s Called Dutch National Flag)

The Dutch flag has three vertical color bands:

- RED (0)
- WHITE (1)
- BLUE (2)

We treat:
- `0` → red
- `1` → white
- `2` → blue

The idea is to partition the array into 3 sections like flag stripes.

---

## 🛠️ Algorithm (3-Pointer Approach)

We use **three pointers**:

- `low`: points to the **end of 0s**
- `mid`: current element under inspection
- `high`: points to the **start of 2s**

```java
public void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;

    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums, low, mid);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else { // nums[mid] == 2
            swap(nums, mid, high);
            high--;
        }
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
````

---

## 🔄 Step-by-Step Dry Run

Input: `[2, 0, 2, 1, 1, 0]`

| Step | low | mid | high | Action             | Array                |
| ---- | --- | --- | ---- | ------------------ | -------------------- |
| 1    | 0   | 0   | 5    | Swap 2 with high   | `[0, 0, 2, 1, 1, 2]` |
| 2    | 0   | 0   | 4    | 0 → Swap with low  | `[0, 0, 2, 1, 1, 2]` |
| 3    | 1   | 1   | 4    | 0 → Swap with low  | `[0, 0, 2, 1, 1, 2]` |
| 4    | 2   | 2   | 4    | 2 → Swap with high | `[0, 0, 1, 1, 2, 2]` |
| 5    | 2   | 2   | 3    | 1 → mid++          | `[0, 0, 1, 1, 2, 2]` |
| 6    | 2   | 3   | 3    | 1 → mid++          | Done                 |

Final Array: `[0, 0, 1, 1, 2, 2]`

---

## 🧠 Pattern Recognition

| Clue in Problem                | Pattern / Strategy    |
| ------------------------------ | --------------------- |
| Only 3 distinct values (0,1,2) | 3-pointer approach    |
| In-place sorting               | Constant space (O(1)) |
| Optimal time                   | One-pass (O(n))       |

---

## 🧾 Summary for Quick Revision

* Use three pointers: `low`, `mid`, `high`
* Iterate `mid` from left to right

    * If `nums[mid] == 0`: swap with `low`, move both `low` and `mid`
    * If `nums[mid] == 1`: just move `mid`
    * If `nums[mid] == 2`: swap with `high`, move `high` (don't move `mid`)
* **Do not use extra space or sort function**

---

## ✅ Time & Space Complexity

* **Time**: O(n) — each element is inspected at most once
* **Space**: O(1) — in-place swaps

---

## 🔚 Real-Life Use Case

Efficient for problems where:

* You need to segregate data in-place with fixed categories.
* Examples: RGB color sorting, quick categorization in embedded systems, etc.

---

Would you like a visual diagram or `.pdf` download of this guide?

```

Let me know if you’d like a dry run on custom input or interview follow-up questions based on this.
```
