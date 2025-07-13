Here's the complete explanation of the **`reverseWords`** method in Java, structured in **interview-style markdown**:

---

# 🔄 Reverse Words in a String – Full Explanation (Java)

---

## 🔍 Problem Statement

Given a string `s`, reverse the order of **words** in the string.

* A word is defined as a sequence of **non-space characters**.
* The words are separated by **one or more spaces**.
* Output must not have leading or trailing spaces, and words must be separated by **a single space**.

---

## 🧠 Intuition

* You're reversing the **order of words**, not characters.
* Use two pointers to scan from the **end of the string** to extract words.
* **Trim extra spaces**, then **manually scan** backwards to construct the reversed string word by word.

---

## 💡 Concepts & Patterns Used

* Two-pointer technique (from end to start)
* `StringBuilder` for efficient string manipulation
* Manual trimming and space handling

---

## ✅ Java Code with Explanation

```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim(); // remove leading and trailing spaces
        StringBuilder str = new StringBuilder();

        int start = s.length() - 1;
        int end = start;

        while (start >= 0) {
            // Move start to the beginning of the word
            while (start >= 0 && s.charAt(start) != ' ') {
                start--;
            }

            // Append the word from start+1 to end
            str.append(s.substring(start + 1, end + 1)).append(" ");

            // Skip multiple spaces
            while (start >= 0 && s.charAt(start) == ' ') {
                start--;
            }

            // Move end pointer to new word end
            end = start;
        }

        // Trim final string to remove the trailing space
        return str.toString().trim();
    }
}
```

---

## 🔎 How to Identify the Pattern

* You're working with **words**, not characters → look for space-based logic.
* You want to process **from the end** → use **reverse traversal**.
* Avoiding built-in split and reverse methods → manual space parsing with **two pointers** is ideal.

---

## 🔄 Step-by-Step Dry Run

### Input:

```java
s = "  the sky   is blue  "
```

### After `.trim()`:

`s = "the sky   is blue"`

### Traversal:

* Start from end = 18, move to find `"blue"`
* Append `"blue "`
* Move to `"is"`, append → `"blue is "`
* Move to `"sky"`, append → `"blue is sky "`
* Move to `"the"`, append → `"blue is sky the "`
* Final result after `.trim()` → **"blue is sky the"**

---

## 🧵 Real-Life Use Cases

* Reversing words in input strings
* Chat message formatting
* Logging/printing messages in reverse

---

## ⏱️ Time & Space Complexity

| Complexity | Description                              |
| ---------- | ---------------------------------------- |
| ⏰ Time     | O(n) → one pass through the string       |
| 🧠 Space   | O(n) → for the result in `StringBuilder` |

---

## 📌 Summary (Quick Revision)

* Trim leading/trailing spaces first
* Use two pointers from the **end of the string**
* Extract each word and build in reverse order
* Trim the final result to remove trailing spaces

---

Let me know if you'd like the solution using built-in `split()` and `Collections.reverse()` for simplicity or for **in-place reverse** approaches!
