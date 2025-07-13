Here’s the complete explanation in markdown format for the **Merge Two Sorted Linked Lists** problem including the code breakdown, dry run, real-world analogy, and how to identify the pattern effectively.

---

# 🔗 Merge Two Sorted Linked Lists — Full Explanation

## 🔍 Problem Statement

Given two **sorted linked lists** `list1` and `list2`, merge them into a single **sorted** linked list and return the head.

### Example:

Input:
`list1: 1 → 2 → 4`
`list2: 1 → 3 → 4`
Output:
`1 → 1 → 2 → 3 → 4 → 4`

---

## 🧠 Intuition

Both input lists are already **sorted**.
Just like how we merge in the merge step of **Merge Sort**, we can **pick the smaller head node** from the two lists, attach it to our merged list, and repeat.

To keep things clean and avoid edge case handling for the first node, we use a **dummy node**.

---

## 💡 Concepts & Patterns Used

* **Linked List Traversal**
* **Two Pointers** (for two lists)
* **Merge Pattern** (used in Merge Sort)
* **Dummy Node** Technique (simplifies edge case handling)

---

## 🔍 How to Identify the Pattern

Clues to choose this approach:

* Two sorted lists → Merge logic from merge sort
* Build a new list from two existing ones → Use a dummy node
* Comparing elements → Two-pointer or simultaneous traversal

---

## ✅ Step-by-Step Code Explanation

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Create a dummy node to simplify handling of head
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;

        // Traverse both lists until one becomes null
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                current.next = list1;     // Pick list1 node
                list1 = list1.next;       // Move list1 forward
            } else {
                current.next = list2;     // Pick list2 node
                list2 = list2.next;       // Move list2 forward
            }
            current = current.next;       // Move current pointer
        }

        // If any list still has remaining elements, attach directly
        if (list1 != null) {
            current.next = list1;
        } else {
            current.next = list2;
        }

        // Return the head of the merged list (skipping dummy)
        return dummy.next;
    }
}
```

---

## 🔄 Step-by-Step Dry Run

### Input:

`list1 = 1 → 2 → 4`
`list2 = 1 → 3 → 4`

### Execution:

* `dummy` created, `current` points to `dummy`
* Compare `1 vs 1` → pick `list1`, move `list1`
* Compare `2 vs 1` → pick `list2`, move `list2`
* Compare `2 vs 3` → pick `list1`, move `list1`
* Compare `4 vs 3` → pick `list2`, move `list2`
* Compare `4 vs 4` → pick `list1`, move `list1`
* `list1` is null → attach remaining `list2`

### Output:

`1 → 1 → 2 → 3 → 4 → 4`

---

## 📦 Real-Life Use Case

* **Merging multiple sorted data streams** (e.g., log files from distributed systems)
* **Combining sorted data** from sensors, databases, or distributed sources
* **Streaming Merge Joins** in database systems

---

## ⏱️ Time & Space Complexity

### ✅ Time Complexity:

* Each node is visited once → **O(n + m)**
  where `n` = length of `list1`, `m` = length of `list2`

### ✅ Space Complexity:

* No extra data structure (in-place merge) → **O(1)**
  (excluding space used by input/output lists)

---

## 📌 Summary (Quick Revision)

* Use a **dummy node** to simplify handling of merged list
* Compare heads of both lists, attach smaller one to result
* Continue until both lists are traversed
* Time: O(n + m), Space: O(1)
* Applies classic **merge step** logic

---

## 🧠 Tips for Interviews

* Clarify: Is in-place merge allowed?
* If lists are large or streamed: emphasize in-place merge to save memory
* Edge Cases: One or both lists are empty

---

Would you like to see the **recursive version** of the same solution next?
