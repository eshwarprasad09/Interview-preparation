Here’s a complete breakdown of the solution to **"Find Maximum Consecutive Ones"** in well-structured Markdown format for deep understanding and quick revision.

---

# 🔢 Find Maximum Consecutive Ones

---

## 💬 Problem Statement

Given a binary array `nums`, return the **maximum number of consecutive 1s** in the array.

### Example:

**Input:** `[1, 1, 0, 1, 1, 1]`
**Output:** `3`

---

## 💡 Intuition

We want to find the **longest streak** of `1`s that appear **contiguously** in the array.

* Use a `counter` to count the current streak of `1`s.
* Use `consecutive` to keep track of the **maximum** streak found so far.
* Reset `counter` to 0 whenever a `0` is encountered.

---

## ✅ Code Explanation (Java)

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int consecutive = 0;  // stores the max streak of 1s
        int counter = 0;      // counts the current streak of 1s

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 1) {
                counter++;  // increase streak count
            } else {
                consecutive = Math.max(counter, consecutive);  // update max
                counter = 0;  // reset counter when 0 is found
            }
        }

        // Check one last time in case array ends with 1
        return Math.max(counter, consecutive);
    }
}
```

---

## 🧠 Concepts & Patterns Used

* **Sliding Window / Streak Counter**
* **Array Traversal**
* **Maximum Tracking**
* **Greedy update** (Keep max only when needed)

---

## 🧩 How to Identify the Pattern

| Clue in Problem Statement | Pattern to Use       |
| ------------------------- | -------------------- |
| Array of `0`s and `1`s    | Count streaks        |
| "Maximum consecutive"     | Use counter + max    |
| Must be **contiguous**    | Reset counter on `0` |

---

## ⚙️ Step-by-Step Dry Run

### Input: `[1, 1, 0, 1, 1, 1]`

| i | nums\[i] | counter | consecutive | Action                    |
| - | -------- | ------- | ----------- | ------------------------- |
| 0 | 1        | 1       | 0           | counter++                 |
| 1 | 1        | 2       | 0           | counter++                 |
| 2 | 0        | 0       | 2           | reset counter, update max |
| 3 | 1        | 1       | 2           | counter++                 |
| 4 | 1        | 2       | 2           | counter++                 |
| 5 | 1        | 3       | 2           | counter++                 |

✅ Return `max(3, 2) = 3`

---

## 📌 Summary (Quick Revision)

* Initialize `counter` and `consecutive` to 0
* Iterate through the array

    * If `1`, increment `counter`
    * If `0`, update `consecutive` and reset `counter`
* After the loop, return `max(counter, consecutive)` to handle ending in 1

---

## 💼 Real-Life Use Case

* Detecting **uptime streaks** in server logs
* Finding **longest active session** (represented by 1s) in binary activity tracking
* Analyzing **attendance records** or **bit streams**

---

## ⏱️ Time & Space Complexity

| Metric   | Value  |
| -------- | ------ |
| 🕒 Time  | `O(n)` |
| 💾 Space | `O(1)` |

---

## 🧪 Edge Cases to Consider

* Empty array → return 0
* All `1`s → return length of array
* All `0`s → return 0
* `1`s only at start or end → ensure trailing count is considered (`return Math.max(...)` at the end)

---

Let me know if you want the **follow-up version**: *What if you could flip at most one 0 to 1?* – which uses sliding window.
