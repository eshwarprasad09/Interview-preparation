Here is a complete, interview-ready explanation and solution for:

---

# 🔍 **Count Substrings with All Unique Characters**

📌 Using the **Sliding Window** approach

---

## ✅ Problem Statement

> Given a string `s`, count how many substrings contain **all unique characters** (i.e., no duplicates within the substring).

---

## 🧠 Intuition

The naive way (O(n³)) would be to generate all substrings and check each for uniqueness. But we can do better using the **sliding window** technique.

### Key Insight:

* We want to find **all substrings where every character is unique**.
* For every **valid window** `[left...right]` where all characters are unique, we can say:

  🧠 We can form **(right - left + 1)** valid substrings ending at `right`.

---

## 🔁 Sliding Window Strategy

Use a **HashSet** to track characters in the current window:

1. Start with both `left` and `right` at index 0.
2. Expand `right` while characters are unique (add to set).
3. If duplicate found, move `left` until duplicate is removed.
4. At each step, count valid substrings ending at `right` as `right - left + 1`.

---

## ✅ Java Code with Inline Comments

```java
class Solution {
    public int countUniqueSubstrings(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int left = 0, count = 0;

        for (int right = 0; right < n; right++) {
            char currChar = s.charAt(right);

            // Shrink window from left until character is unique
            while (set.contains(currChar)) {
                set.remove(s.charAt(left));
                left++;
            }

            // Add current character to the set
            set.add(currChar);

            // Every window [left...right] has (right - left + 1) substrings ending at right
            count += right - left + 1;
        }

        return count;
    }
}
```

---

## 🔂 Dry Run

### Input:

```java
s = "abc"
```

| Window | Set       | Count Added | Total Count |
| ------ | --------- | ----------- | ----------- |
| a      | {a}       | 1           | 1           |
| ab     | {a, b}    | 2           | 3           |
| abc    | {a, b, c} | 3           | 6           |

### Output:

```java
6 substrings: a, b, c, ab, bc, abc
```

---

## 📚 Concepts & Patterns Used

| Concept        | Use Case                            |
| -------------- | ----------------------------------- |
| Sliding Window | Keep track of valid substrings      |
| Two Pointers   | Track current substring bounds      |
| HashSet        | Ensure uniqueness in current window |

---

## 🧠 Real-Life Use Case

* **Login streaks**: Longest streaks with no duplicate login from same device/IP.
* **Security analysis**: Detect intrusion-free character spans in logs.
* **Stream processing**: Count real-time windows with distinct tokens.

---

## ⏱️ Time & Space Complexity

| Metric | Value                                                         |
| ------ | ------------------------------------------------------------- |
| Time   | `O(n)` — each char added/removed at most once                 |
| Space  | `O(k)` — where `k` is charset size (26 for lowercase letters) |

---

## 📌 Summary: Quick Revision Points

* ✅ Use **HashSet** to store current window characters
* ✅ Expand `right` if char is unique, shrink `left` on duplicate
* 🔁 Add `(right - left + 1)` to count for every valid `right`
* ⏱️ Time: `O(n)`, 📦 Space: `O(k)`
* 🔥 Very popular sliding window interview problem

---

Let me know if you’d like:

* 🔁 Similar problems (e.g., longest unique substring)
* 📊 Python/C++ version
* 🔁 Sliding window template cheat sheet
