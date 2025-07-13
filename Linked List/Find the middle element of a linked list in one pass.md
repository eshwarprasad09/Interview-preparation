Here’s a complete, **interview-ready explanation** and solution for:

---

# 🔍 **Find the Middle Element of a Linked List (in One Pass)**

---

## ✅ Problem Statement

> Given the head of a singly linked list, return the **middle node**.
>
> * If there are two middle nodes, return the **second** one.

📌 Example:

```text
Input: 1 → 2 → 3 → 4 → 5 → null
Output: 3

Input: 1 → 2 → 3 → 4 → null
Output: 3
```

---

## 🧠 Intuition

We want to find the **middle** of a linked list in just **one pass** — without counting nodes or storing them.

---

## 🔁 Two Pointer (Tortoise & Hare) Strategy

Use:

* `slow` pointer → moves **1 step at a time**
* `fast` pointer → moves **2 steps at a time**

### Key Insight:

* When `fast` reaches the **end** of the list, `slow` will be at the **middle**.

This is a classic **tortoise and hare** algorithm used in:

* Linked list cycles
* Middle detection
* Palindrome problems

---

## 🛠️ Full Java Code with Inline Comments

```java
// Definition for singly-linked list node
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
    }
}

class Solution {
    public ListNode middleNode(ListNode head) {
        // Initialize both pointers at head
        ListNode slow = head;
        ListNode fast = head;

        // Move fast by 2 and slow by 1
        while (fast != null && fast.next != null) {
            slow = slow.next;          // 1 step
            fast = fast.next.next;     // 2 steps
        }

        // When fast reaches the end, slow is at middle
        return slow;
    }
}
```

---

## 🔂 Dry Run Example

### Input:

```text
1 → 2 → 3 → 4 → 5
```

| Step | `slow` | `fast`        |
| ---- | ------ | ------------- |
| 0    | 1      | 1             |
| 1    | 2      | 3             |
| 2    | 3      | 5             |
| 3    | —      | null   → stop |

Return `slow = 3`

---

## 📚 Concepts & Patterns Used

| Concept               | Why Used                              |
| --------------------- | ------------------------------------- |
| Tortoise and Hare     | Efficient middle detection            |
| Fast & Slow Pointers  | Save space and improve speed          |
| Linked List Traversal | Handles even/odd-length lists cleanly |

---

## ⏱️ Time and Space Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | `O(n)`     |
| Space  | `O(1)`     |

---

## 🧠 Real-Life Use Case

* Divide a linked list into two halves efficiently.
* Used in merge sort, palindrome check, cycle detection in linked lists.

---

## 📌 Summary: Quick Revision Points

* ✅ Use two pointers: `slow` and `fast`
* 🚶 `slow` moves 1 step, 🏃 `fast` moves 2 steps
* 🧠 When `fast` reaches end, `slow` is in the middle
* ⏱️ Time: `O(n)`, 📦 Space: `O(1)`
* ⚡ Works for both even and odd length lists

---

Let me know if you want:

* 📊 Print first half vs second half split
* 💥 Version for **doubly** linked list
* 🔁 Extend this to **find Kth node from end** (another classic)
