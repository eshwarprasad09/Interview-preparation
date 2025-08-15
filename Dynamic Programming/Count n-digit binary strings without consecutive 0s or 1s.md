Let's solve the classic **Dynamic Programming (DP)** problem:

---

# ✅ Count n-digit binary strings without consecutive 0s or 1s

---

## 📘 Problem Statement

Given an integer `n`, count the number of **n-digit binary strings** that do **not** contain **consecutive 0s** or **consecutive 1s**.

👉 For example:

* `n = 3`
  Valid strings: `"010"`, `"101"`, `"010"`, `"101"`, `"010"`, etc.
  Invalid: `"000"`, `"111"`, `"001"`, `"110"`, etc. (contains consecutive 0s or 1s)

---

## 🔍 Category & Concept

* ✅ **Dynamic Programming**
* ✅ **Recursion + Memoization / Tabulation**
* ❌ Not Greedy or Backtracking
* Problem type: **Digit DP** or **Sequence DP**

---

## 🤔 Intuition & Observations

At each position in the string, we can place:

* `'0'` → **only if previous was `'1'`**
* `'1'` → **only if previous was `'0'`**

We must **track what the last digit was**, to decide what we can place next.

---

## 🔄 Subproblem Definition

Let’s define two DP arrays:

* `dp0[i]`: Number of binary strings of length `i` **ending in 0**
* `dp1[i]`: Number of binary strings of length `i` **ending in 1**

### Recurrence:

* `dp0[i] = dp1[i-1]` → we can add `'0'` only after `'1'`
* `dp1[i] = dp0[i-1]` → we can add `'1'` only after `'0'`

### Base Case:

* `dp0[1] = 1` → "0"
* `dp1[1] = 1` → "1"

---

## 🧠 DP Formula

```text
For i from 2 to n:
    dp0[i] = dp1[i-1]
    dp1[i] = dp0[i-1]
```

Total valid strings of length `n` = `dp0[n] + dp1[n]`

---

## 📚 Java Code with Inline Comments

```java
public class NoConsecutiveBinaryStrings {

    public static int countStrings(int n) {
        if (n == 0) return 0;
        if (n == 1) return 2; // "0", "1"

        int[] dp0 = new int[n + 1]; // ending with 0
        int[] dp1 = new int[n + 1]; // ending with 1

        // Base cases
        dp0[1] = 1; // "0"
        dp1[1] = 1; // "1"

        for (int i = 2; i <= n; i++) {
            dp0[i] = dp1[i - 1];       // Add "0" only after "1"
            dp1[i] = dp0[i - 1];       // Add "1" only after "0"
        }

        return dp0[n] + dp1[n];
    }

    public static void main(String[] args) {
        int n = 5;
        System.out.println("Valid binary strings of length " + n + ": " + countStrings(n));
    }
}
```

---

## 🧪 Step-by-Step Dry Run (n = 3)

```
dp0[1] = 1 ("0")
dp1[1] = 1 ("1")

i = 2:
  dp0[2] = dp1[1] = 1 ("10")
  dp1[2] = dp0[1] = 1 ("01")

i = 3:
  dp0[3] = dp1[2] = 1 ("010")
  dp1[3] = dp0[2] = 1 ("101")

Answer: 2 (["010", "101"])
```

---

## ✅ Summary (For Quick Revision)

* Use **two DP arrays** → `dp0` for ending in 0, `dp1` for ending in 1
* Transition: `dp0[i] = dp1[i-1]`, `dp1[i] = dp0[i-1]`
* Base: `dp0[1] = 1`, `dp1[1] = 1`
* Final answer: `dp0[n] + dp1[n]`

---

## ⏱ Time & Space Complexity

* **Time**: `O(n)` – one pass
* **Space**: `O(n)` – two arrays of size `n+1`

    * Can be reduced to **O(1)** using variables for previous states.

---

## 🤖 Real Life Use Case

* **Communication protocols**: To avoid long sequences of 0s or 1s (e.g., Manchester encoding).
* **Data compression**: Limiting consecutive bits to avoid ambiguity.
* **Scheduling**: No back-to-back similar tasks (e.g., rest/work cycles).

---

## 🔁 Space Optimized Version

```java
public static int countStringsOptimized(int n) {
    if (n == 0) return 0;
    int prev0 = 1, prev1 = 1;

    for (int i = 2; i <= n; i++) {
        int curr0 = prev1;
        int curr1 = prev0;
        prev0 = curr0;
        prev1 = curr1;
    }

    return prev0 + prev1;
}
```

---

Let me know if you want:

* version where **no 3 consecutive 0s or 1s** allowed,
* or with a constraint like **starting with 1** or ending with 0.
