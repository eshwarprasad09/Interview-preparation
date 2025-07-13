Here is the **complete explanation** of your `nextPermutation` Java solution in clean, well-structured **Markdown format**.

---

````markdown
# 🔁 Next Permutation (Java) - Complete Step-by-Step Explanation

---

## 📘 Problem Statement (Leetcode 31 - Next Permutation)

> Given an array of integers `nums`, rearrange the numbers into the **lexicographically next greater permutation** of numbers.  
> If such arrangement is not possible (i.e., the current permutation is the highest), rearrange it as the **lowest possible order** (i.e., sorted in ascending order).

---

## 🔎 Intuition and Goal

- Find the **next permutation** that is **just greater** than the current one.
- If the current permutation is the **last one**, return the **first permutation** by sorting.
- Do all this **in-place** with **O(1) extra space**.

---

## 🧠 Step-by-Step Explanation

```java
public void nextPermutation(int[] nums)
````

### 🔹 Step 1: Find the Break Point

```java
for(int i = nums.length-1; i>0; i--){
    if(nums[i-1] < nums[i]){
        breakPoint = i-1;
        isBreakPoint = true;
        break;
    }
}
```

* Traverse **from right to left**.
* Find the **first pair** where `nums[i-1] < nums[i]`.

    * That index `i-1` is the **break point**.
    * It means we can still make a bigger permutation.
* If no such point is found (fully descending order), the array is the **last permutation**.

---

### 🔹 Step 2: If No Break Point, Reverse Entire Array

```java
if(!isBreakPoint){
    reverse(nums, 0, nums.length-1);
}
```

* If `nums` is in descending order, it's the **last permutation**.
* So, return the **first permutation** by reversing it.

---

### 🔹 Step 3: If Break Point Found, Find Closest Greater Element on the Right

```java
int closestEle = breakPoint+1;
for(int i = nums.length-1; i>breakPoint+1; i--){
    if(nums[i] > nums[breakPoint] && nums[i] <= nums[closestEle]){
        closestEle = i;
        break;
    }
}
```

* From the right, find the **smallest number greater than** `nums[breakPoint]`.
* It ensures we get the **next smallest** permutation (not a jump ahead).
* Update `closestEle` with its index.

---

### 🔹 Step 4: Swap Break Point with Closest Greater Element

```java
int temp = nums[breakPoint];
nums[breakPoint] = nums[closestEle];
nums[closestEle] = temp;
```

* Swap to place the slightly bigger number at the break point.

---

### 🔹 Step 5: Reverse the Right Part

```java
reverse(nums, breakPoint+1, nums.length-1);
```

* Reverse the portion **right of the break point** to make it the **smallest order**.
* Because it was in descending order.

---

### 🔧 Helper: Reverse Method

```java
public void reverse(int[] nums, int start, int end){
    while(start < end){
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```

* Standard in-place array reversal.

---

## 📦 Concepts & Patterns Used

| Concept                   | Explanation                                                         |
| ------------------------- | ------------------------------------------------------------------- |
| Greedy + Two Pointer      | Greedy to find the closest next permutation, two-pointer to reverse |
| Lexicographic Permutation | Similar to how "next permutation" works in dictionary order         |
| Array Traversal (reverse) | Reverse tail part to ensure the smallest order after swap           |
| Constant Space            | All operations done in-place with `O(1)` space                      |

---

## 🔎 How to Identify the Pattern / Choose Algorithm

| Cue in Problem                    | Pattern / Strategy to Apply                 |
| --------------------------------- | ------------------------------------------- |
| "Next permutation"                | Lexicographical ordering + swap and reverse |
| "In-place" without extra memory   | Constant space → modify original array      |
| Array of integers                 | Two-pointer strategy applicable             |
| Want closest larger configuration | Greedy search from end to start             |

---

## 🧠 Summary for Quick Revision (Bullet Points)

* ✅ Traverse from the end to find **first decreasing pair** → **break point**
* ✅ If no break point → reverse entire array (last → first permutation)
* ✅ Else, find **smallest number greater than break point value** on the right
* ✅ Swap it with break point
* ✅ Reverse the right half after the break point
* ✅ Use constant space (`O(1)`)

---

## 📌 Example

### Input:

```java
nums = [1, 2, 3]
```

### Output:

```java
[1, 3, 2]
```

---

### Input:

```java
nums = [3, 2, 1]
```

### Output:

```java
[1, 2, 3]  // reversed because it was the last permutation
```

---

## 📘 Extra Tip

This is the exact logic used in C++'s `std::next_permutation` function.

Let me know if you'd like a **dry-run walkthrough** of an example or a **visual diagram** to understand the logic better!

```

Would you like a `.md` or `.pdf` file download version of this response?
```
# 🧪 Edge Case Inputs for Next Permutation

## ✅ 1. Already the **Last Permutation**
- Input: `[3, 2, 1]`
- Reason: This is in descending order → reverse to get the smallest
- Output: `[1, 2, 3]`

---

## ✅ 2. Already the **First Permutation**
- Input: `[1, 2, 3]`
- Reason: Lexicographically smallest → next should be smallest increase
- Output: `[1, 3, 2]`

---

## ✅ 3. Duplicate Elements
- Input: `[1, 1, 5]`
- Reason: To check if it handles duplicates correctly
- Output: `[1, 5, 1]`

---

## ✅ 4. All Elements Same
- Input: `[2, 2, 2]`
- Reason: No possible next permutation → result is same
- Output: `[2, 2, 2]`

---

## ✅ 5. Two Elements in Ascending Order
- Input: `[1, 2]`
- Reason: Simple base case → swap them
- Output: `[2, 1]`

---

## ✅ 6. Two Elements in Descending Order
- Input: `[2, 1]`
- Reason: Already last → reverse to get smallest
- Output: `[1, 2]`

---

## ✅ 7. Longer Array with Middle Breakpoint
- Input: `[1, 3, 5, 4, 2]`
- Reason: Breakpoint at index 1 (`3 < 5`) → next perm should be `[1, 4, 2, 3, 5]`
- Output: `[1, 4, 2, 3, 5]`

---

## ✅ 8. Zero and Negative Numbers
- Input: `[-1, 0, -2]`
- Reason: Handles negative numbers → break point at index 0
- Output: `[0, -2, -1]`

---

## ✅ 9. Large Numbers at End
- Input: `[1, 2, 9, 8, 7, 6]`
- Reason: Breakpoint at index 1 → swap with 6 → reverse right part
- Output: `[1, 6, 2, 7, 8, 9]`

---

## ✅ 10. Single Element
- Input: `[1]`
- Reason: No permutation possible
- Output: `[1]`

---

Let me know if you'd like a table or visual dry-run for these inputs!
