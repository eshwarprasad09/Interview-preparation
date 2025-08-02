Here’s a complete breakdown of the **Allocate Minimum Number of Pages** problem — including step-by-step code explanation, patterns used, dry run, and optimization insights.

---

# 📘 Allocate Minimum Pages — Binary Search on Answer

---

## ✅ Problem Statement

> Given an array `books[]` where `books[i]` represents the number of pages in the `i-th` book and an integer `m` (number of students), allocate books to `m` students such that:

* Every student gets **contiguous** books.
* The **maximum number of pages assigned** to a student is **minimized**.

📌 Example:

```text
books = [12, 34, 67, 90], m = 2
Output: 113
Explanation: Allocate [12,34,67] to 1st student and [90] to 2nd — max pages = 113
```

---

## 🧠 Intuition

We need to divide books into `m` parts (groups of contiguous elements) such that:

* Each student gets at least one book.
* The **maximum pages** assigned to any student is **as small as possible**.

👉 This is a **minimize the maximum** problem — use **Binary Search on Answer**.

---

## 🔍 Pattern Recognition

| Clue in Problem                    | Pattern / Technique               |
| ---------------------------------- | --------------------------------- |
| Minimize the maximum of a range    | Binary Search on Answer           |
| Constraints: contiguous allocation | Greedy validation with simulation |
| Check feasibility of a guess       | Helper function                   |

---

## ✅ Java Code with Full Explanation

```java
class Solution {
    // Main function
    public int findPages(int[] books, int m) {
        int n = books.length;
        if (n < m) return -1; // Not enough books for all students

        int low = getMax(books);   // Minimum pages a student can get (largest book)
        int high = getSum(books);  // Maximum pages (one student gets all books)
        int result = -1;

        // Binary search on possible answers
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isFeasible(books, m, mid)) {
                result = mid;      // Try to minimize max pages
                high = mid - 1;
            } else {
                low = mid + 1;     // Increase pages per student
            }
        }

        return result;
    }

    // Helper: Check if allocation is possible with maxPages limit
    private boolean isFeasible(int[] books, int students, int maxPages) {
        int count = 1; // at least 1 student
        int pages = 0;

        for (int book : books) {
            if (pages + book > maxPages) {
                count++;           // assign to next student
                pages = book;      // reset pages
                if (count > students) return false;
            } else {
                pages += book;
            }
        }

        return true;
    }

    // Helper: Maximum value in array (for binary search lower bound)
    private int getMax(int[] arr) {
        int max = arr[0];
        for (int num : arr) max = Math.max(max, num);
        return max;
    }

    // Helper: Total sum of array (for binary search upper bound)
    private int getSum(int[] arr) {
        int sum = 0;
        for (int num : arr) sum += num;
        return sum;
    }
}
```

---

## 🔁 Step-by-Step Dry Run

```java
books = [12, 34, 67, 90], students = 2
```

* Low = max(books) = 90
* High = sum(books) = 203

### Binary Search Iterations:

1. mid = 146 → feasible (students = 2)
2. mid = 118 → feasible
3. mid = 104 → **not** feasible (needs 3 students)
4. mid = 111 → not feasible
5. mid = 114 → not feasible
6. mid = 116 → not feasible
7. mid = 117 → not feasible
8. mid = 118 → feasible ✅

**Answer: 113**

---

## 📚 Concepts & Patterns Used

| Concept                 | Role                                 |
| ----------------------- | ------------------------------------ |
| Binary Search on Answer | Optimize answer in value space       |
| Greedy + Simulation     | Check if a guessed value is feasible |
| Minimize the Maximum    | Common binary search pattern         |

---

## ⏱️ Time & Space Complexity

| Metric | Value                        |
| ------ | ---------------------------- |
| Time   | `O(n * log(sum - max))`      |
| Space  | `O(1)` — no extra space used |

* `n` = number of books
* Binary search range = `sum(books) - max(books)`

---

## 💡 Real-Life Use Case

* Distributing work evenly across teams
* Allocating files to disks to minimize max disk load
* Printing pages to printers while balancing load

---

## 📌 Summary (Quick Revision)

* 🧠 Convert problem to **minimize the maximum** → use binary search
* 🔁 Binary search range: `max(books)` to `sum(books)`
* ✅ Validate guess with **greedy simulation**
* ⏱️ Time = `O(n * log range)`
* 🚫 Not possible if books < students → return `-1`

---

Let me know if you'd like:

* 🧪 Test case-based explanation
* 🧠 Allocate books to students and return the **distribution**
* 💬 Python version of the same code
