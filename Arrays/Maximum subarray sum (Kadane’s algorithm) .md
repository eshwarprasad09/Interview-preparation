Here’s a complete breakdown and code explanation of the **Maximum Subarray Sum** problem using the famous **Kadane’s Algorithm**.

---

# 🔥 Maximum Subarray Sum (Kadane’s Algorithm)

---

## ✅ Problem Statement

> Given an integer array `nums`, find the **contiguous subarray** (containing at least one number) which has the **largest sum**, and return its sum.

📌 Example:

```text
Input: [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

---

## 🧠 Intuition

We want to find a **contiguous subarray** (not necessarily the entire array) with the **maximum possible sum**.

Instead of trying all subarrays (which takes O(n²)), we can use **dynamic programming** to build up the answer in one pass.

---

## 🔍 How to Identify the Pattern

| Clue                      | Pattern/Technique    |
| ------------------------- | -------------------- |
| “Contiguous” subarray     | Sliding Window / DP  |
| “Maximum sum”             | Prefix sum or Kadane |
| Need optimal substructure | Dynamic Programming  |
| Must run in O(n)          | Kadane’s Algorithm   |

---

## 🛠️ Kadane’s Algorithm: Core Idea

We iterate through the array and **at each index** decide:

👉 Do we **extend the previous subarray** OR **start a new subarray** from this element?

We track two values:

* `currentSum`: best subarray sum **ending at current index**
* `maxSum`: best subarray sum **found so far**

---

## ✅ Java Code with Inline Comments

```java
class Solution {
    public int maxSubArray(int[] nums) {
        // Initialize current sum and max sum with the first element
        int currentSum = nums[0];
        int maxSum = nums[0];

        // Start from the second element
        for (int i = 1; i < nums.length; i++) {
            int num = nums[i];

            // If adding current number improves sum, include it
            // Otherwise, start new subarray from current number
            currentSum = Math.max(num, currentSum + num);

            // Update global max if current subarray is better
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
}
```

---

## 🔁 Step-by-Step Dry Run

### Input:

```text
[-2,1,-3,4,-1,2,1,-5,4]
```

| i | num | currentSum           | maxSum |
| - | --- | -------------------- | ------ |
| 0 | -2  | -2                   | -2     |
| 1 | 1   | max(1, -2+1) = 1     | 1      |
| 2 | -3  | max(-3, 1 + -3) = -2 | 1      |
| 3 | 4   | max(4, -2 + 4) = 4   | 4      |
| 4 | -1  | max(-1, 4 + -1) = 3  | 4      |
| 5 | 2   | max(2, 3 + 2) = 5    | 5      |
| 6 | 1   | max(1, 5 + 1) = 6    | 6 ✅    |
| 7 | -5  | max(-5, 6 + -5) = 1  | 6      |
| 8 | 4   | max(4, 1 + 4) = 5    | 6      |

**Final Answer**: 6

---

## 📚 Concepts & Patterns Used

| Concept                | Why Used                        |
| ---------------------- | ------------------------------- |
| Dynamic Programming    | Solve with optimal substructure |
| Sliding Window Concept | Adjusting sum window as we go   |
| Greedy                 | Take best choice at each index  |

---

## ⏱️ Time and Space Complexity

| Metric    | Value  |
| --------- | ------ |
| **Time**  | `O(n)` |
| **Space** | `O(1)` |

---

## 🧠 Real-Life Use Cases

* 📈 Maximum profit window in stock trading
* 📊 Maximum engagement streak in analytics
* 🔄 Maximum contiguous sum of performance scores

---

## 📌 Summary for Quick Revision

* ✅ Kadane’s is used for **maximum contiguous subarray sum**
* 🧠 At each index, decide: **continue subarray** or **start new**
* 📦 Track `currentSum` and `maxSum`
* ⏱️ Time = `O(n)`, Space = `O(1)`
* 🔥 Handles positive, negative, and mixed values

---

Let me know if you want:

* 🧾 Index of the subarray (not just sum)
* 🔄 Variants: max product subarray, circular array, etc.
* 📊 Implementation in Python or JavaScript
