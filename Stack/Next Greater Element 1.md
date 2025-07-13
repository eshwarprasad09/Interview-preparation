Here’s a **complete Markdown explanation** of the `nextGreaterElement` problem and your Java code solution. This version is fully annotated with **intuition, patterns, data structures, dry run, and performance analysis** — ideal for interviews, notes, and revision.

---

# 🔍 Next Greater Element I — Explained

---

## ✅ Problem

> You are given two arrays `nums1` and `nums2` where:
>
> * `nums1` is a subset of `nums2`
> * For each element in `nums1`, find the **next greater element** in `nums2`
> * If no such element exists, return `-1`

### 🧪 Example:

```java
nums1 = [4, 1, 2], nums2 = [1, 3, 4, 2]
Output: [-1, 3, -1]
```

---

## 💡 Intuition

You are being asked, for each number in `nums1`, what is the **next greater element to its right in `nums2`**?

Instead of doing a brute-force scan for each element (which would take O(n²)), we preprocess `nums2` once using a **monotonic stack** and a **map** to store each number’s next greater number.

---

## 🛠️ Concepts & Patterns Used

| Concept / Pattern   | Why It’s Used                                                                  |
| ------------------- | ------------------------------------------------------------------------------ |
| **Monotonic Stack** | To maintain decreasing numbers and find next greater elements in linear time   |
| **HashMap**         | To store and lookup next greater elements quickly                              |
| **Greedy**          | We make a local decision: if `num > stack.peek()`, it must be the next greater |
| **Preprocessing**   | Build the answer for all of `nums2`, and lookup for each `nums1[i]`            |

---

## 🧠 How to Identify This Pattern

| Clue from Problem      | Implied Pattern                                    |
| ---------------------- | -------------------------------------------------- |
| "Next greater element" | Stack-based processing (monotonic stack)           |
| "Subset array" lookup  | Preprocess with a `HashMap`                        |
| "Right of the element" | Implies right-side traversal or forward processing |

---

## 🧰 Full Code With Inline Comment Explanation

```java
public static int[] nextGreaterElement(int[] nums1, int[] nums2) {
    int[] ans = new int[nums1.length]; // output array

    Stack<Integer> stack = new Stack<>(); // monotonic decreasing stack
    HashMap<Integer, Integer> map = new HashMap<>(); // map from num -> next greater

    // Step 1: Preprocess nums2 to build map of next greater elements
    for (int num : nums2) {
        // If the current number is greater than the stack top,
        // it's the next greater element for elements in the stack
        while (!stack.isEmpty() && num > stack.peek()) {
            map.put(stack.pop(), num);
        }
        // Add current number to the stack
        stack.push(num);
    }

    // Step 2: Build the answer for nums1 by checking the map
    int i = 0;
    for (int num : nums1) {
        // If num has a next greater element, use it; otherwise, use -1
        ans[i++] = map.getOrDefault(num, -1);
    }

    return ans;
}
```

---

## 🔄 Step-by-Step Dry Run

### Input:

```java
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]
```

### Preprocess `nums2` using stack:

| Iteration | num | Stack   | Map (next greater)    |
| --------- | --- | ------- | --------------------- |
| 1         | 1   | \[1]    | {}                    |
| 2         | 3   | \[3]    | {1 → 3}               |
| 3         | 4   | \[4]    | {1 → 3, 3 → 4}        |
| 4         | 2   | \[4, 2] | (no greater for 4, 2) |

Stack ends with: `[4, 2]`

### Build `ans[]` for `nums1 = [4, 1, 2]`:

* 4 → not in map → -1
* 1 → in map → 3
* 2 → not in map → -1

✅ Output: `[-1, 3, -1]`

---

## 📚 Real-Life Use Case

* **Stock Price Analysis**: Find the next day when stock price will be higher.
* **Temperature Forecast**: "Next warmer day" problem.
* **Task Deadlines**: When will a higher priority task come next?

---

## ⏱️ Time & Space Complexity

| Metric   | Complexity |
| -------- | ---------- |
| **Time** | `O(n + m)` |

* `n` = length of `nums1`, `m` = length of `nums2`
* Traverse `nums2` once, `nums1` once

\| **Space**          | `O(m)`

* Stack + HashMap to store next greater mappings for all of `nums2`

---

## 📝 Summary: Quick Revision Points

* ✅ Use **Monotonic Stack** to process `nums2` in `O(n)`
* ✅ Use **HashMap** to store `nextGreater` values for each number in `nums2`
* ✅ For each number in `nums1`, just lookup the map
* ✅ Time: `O(n + m)`, Space: `O(m)`
* ✅ Pattern: **Stack + Map + Preprocessing**

---

Let me know if you’d like this pattern generalized for similar problems like:

* "Daily Temperatures"
* "Next Greater Element II" (circular array)
  Or want an animated visual!
