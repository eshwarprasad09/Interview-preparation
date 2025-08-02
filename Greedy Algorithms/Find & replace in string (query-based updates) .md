Let's go through the **"Find & Replace in String"** problem with full explanation, code, dry run, and key insights.

---

# 🧩 Problem: Find & Replace in String

You’re given:

* A string `S`
* Arrays:

    * `indexes[]` – positions in `S`
    * `sources[]` – substrings to match from those positions
    * `targets[]` – replacement strings

**Task**: For each position `i` in `indexes[]`, **if** `sources[i]` matches the substring in `S` starting at `indexes[i]`, **replace** it with `targets[i]`.

### 🧪 Example

```text
S = "abcd"
indexes = [0, 2]
sources = ["a", "cd"]
targets = ["eee", "ffff"]

Result: "eeebffff"
```

---

## ✅ Category & Concept

* **Greedy** / **String Manipulation**
* **Index Mapping**
* **Query-based updates**
* Related to: “Batch string update”, “Find and replace pattern”

---

## 🎯 Intuition

* For each replacement, we must:

    1. Check if `sources[i]` matches substring from `indexes[i]`
    2. If yes, replace it with `targets[i]`
* Multiple replacements can overlap → **process in order of increasing index**.

---

## 💡 Approach

1. Build a **map** of valid replacement operations:

   ```java
   map[index] = (source, target)
   ```
2. Sort replacements by **decreasing index** (to avoid messing up indices as you modify string).
3. Use a `StringBuilder` to build the result from left to right.

---

## 💻 Full Java Code with Inline Comments

```java
import java.util.*;

public class FindAndReplaceInString {

    public static String findReplaceString(String s, int[] indices, String[] sources, String[] targets) {
        // Map from index to (source, target) pair
        Map<Integer, Integer> replaceMap = new HashMap<>();

        for (int i = 0; i < indices.length; i++) {
            int idx = indices[i];
            String src = sources[i];

            // Check if the source matches the string at the given index
            if (s.startsWith(src, idx)) {
                replaceMap.put(idx, i); // Store index of replacement
            }
        }

        // Build final string
        StringBuilder result = new StringBuilder();
        int i = 0;

        while (i < s.length()) {
            if (replaceMap.containsKey(i)) {
                int k = replaceMap.get(i); // original index in sources/targets arrays
                result.append(targets[k]); // Append replacement
                i += sources[k].length();  // Move forward by length of source
            } else {
                result.append(s.charAt(i));
                i++;
            }
        }

        return result.toString();
    }

    public static void main(String[] args) {
        String S = "abcd";
        int[] indexes = {0, 2};
        String[] sources = {"a", "cd"};
        String[] targets = {"eee", "ffff"};

        String result = findReplaceString(S, indexes, sources, targets);
        System.out.println("Result: " + result); // eeebffff
    }
}
```

---

## 🔍 Dry Run

Given:

```
S = "abcd"
indexes = [0, 2]
sources = ["a", "cd"]
targets = ["eee", "ffff"]
```

Steps:

* Check index 0: S.startsWith("a", 0) → true → map\[0] = 0
* Check index 2: S.startsWith("cd", 2) → true → map\[2] = 1

Build result:

* i=0 → in map → replace "a" → append "eee" → i += 1 → i=1
* i=1 → not in map → append "b" → i=2
* i=2 → in map → replace "cd" → append "ffff" → i += 2 → i=4 (done)

Output: `"eeebffff"`

---

## 📘 Concepts & Patterns Used

* **HashMap** for quick index lookup
* **String.startsWith()** to verify source match
* **StringBuilder** for efficient construction
* **Greedy traversal** left to right
* Process only **valid** replacements

---

## 📋 Summary (Quick Revision)

* Build `Map<index, replacement>` where match is valid
* Use `StringBuilder` to build the output string
* Move pointer `i` and decide:

    * Replace substring (if map contains index)
    * Or just copy character

---

## 🧠 How to Identify This Pattern

* Problem says “find and replace substrings **only if** they match”
* Replacement is **not character-based**, but **substring-based**
* Index is important → think of **mapping** or **position-wise** operations

---

## 🕒 Time and Space Complexity

* **Time**: `O(N + M)`

    * N = length of input string `s`
    * M = number of replacements
* **Space**: `O(M)` – map + result builder

---

## 🎯 Real-World Applications

* **Text Editor** "Find and Replace" feature
* **Search and tagging** systems (e.g., replacing @mentions or hashtags)
* **Macro replacement** in compilers
* **Automated form filling** or placeholder updates

---

## ✅ Edge Cases

* Index out of range → already handled with `startsWith()`
* Overlapping matches → safe due to left-to-right handling

---

If you want a **version where overlapping replacements are allowed**, or in **descending order**, I can help you adjust that too.

Let me know if you'd like the **Python** or **C++** version!
