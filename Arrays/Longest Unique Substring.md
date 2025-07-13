# Longest Substring Without Repeating Characters

## 🔍 Problem Statement

Given a string `s`, find the **length of the longest substring** without repeating characters.

---

## 🧠 Intuition

The key idea is to maintain a **sliding window** that keeps track of characters without duplicates.

* When we find a duplicate character, we **shrink the window** from the left until it's unique again.
* Use a **HashSet** to quickly check if a character is already in the current substring.

This avoids brute force checking all substrings.

---

## 💡 Concepts & Patterns Used

* **Sliding Window Technique**
* **Two Pointers** (Left and Right Indexes)
* **HashSet** for fast membership check (O(1))

---

## 🔍 How to Identify the Pattern

* Look for constraints around substrings or contiguous segments.
* Keywords like "longest substring without repeat" suggest a sliding window approach.
* Need to find **maximum** of something while scanning => Sliding Window + HashSet/Map.

---

## ✅ Java Code Explanation

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> seen = new HashSet<>();
        int left = 0, right = 0, maxLen = 0;

        while (right < s.length()) {
            char currChar = s.charAt(right);

            // If duplicate, shrink the window from left
            while (seen.contains(currChar)) {
                seen.remove(s.charAt(left));
                left++;
            }

            // Add new character to the set
            seen.add(currChar);
            maxLen = Math.max(maxLen, right - left + 1);
            right++;
        }

        return maxLen;
    }
}
```

---

## 🔮 Step-by-Step Dry Run

### Input:

`s = "abcabcbb"`

### Execution:

* Window starts empty, maxLen = 0
* Slide `right` through the string:

```
right=0: 'a' -> set = {a}, maxLen=1
right=1: 'b' -> set = {a,b}, maxLen=2
right=2: 'c' -> set = {a,b,c}, maxLen=3
right=3: 'a' -> duplicate! remove 'a' from left, set = {b,c}, then add 'a'
right=4: 'b' -> duplicate! remove 'b' from left, set = {c,a}, then add 'b'
...
```

### Final maxLen = 3 (`"abc"`)

---

## 🌐 Real-Life Use Case

* **Autocomplete Engines**: Ensure input substrings have no repeat characters.
* **Stream Analysis**: In data streams, detect longest unique sequences.
* **Security Systems**: Detect repeated access attempts or unique key sequences.

---

## ⏱️ Time and Space Complexity

### Time Complexity:

* O(n), where `n` is length of the string
* Each character is visited at most twice

### Space Complexity:

* O(min(n, m)) where `m` is the size of the character set (usually 128 or 256)
* Using HashSet to store current substring

---

## 🔹 Summary (Quick Revision)

* Use **Sliding Window** with **HashSet**
* Move `right` pointer to include new chars
* Move `left` pointer when duplicate found
* Keep updating max length
* Time: O(n), Space: O(n)

---

Let me know if you'd like the optimized version using **HashMap** to store character indices.
