Here’s a complete explanation of the **“Rotate Linked List to the Right by `k` Places”** problem in **Markdown** format, based on your code (with corrections and improvements).

---

# 🔁 Rotate a Linked List to the Right by `k` Places

---

## 📘 Problem Statement

Given the head of a singly linked list and an integer `k`, rotate the list to the right by `k` places.

---

## 💡 Intuition

Imagine rotating a circular track:
If you move `k` steps to the right, the last `k` nodes become the new head, and the rest are attached after them.

We need to:

* Count length `n` of the list
* Compute `k = k % n` to handle `k > n`
* Find the `(n - k)`-th node
* Break the list at that point
* Reattach parts accordingly

---

## 🔧 Concepts & Patterns Used

* **Linked List Traversal**
* **Modular Arithmetic** (for `k > length`)
* **Two-pointer / Tail Manipulation**
* **List Re-linking**

---

## 🤖 Correct & Optimized Algorithm (NOT the one in your current code)

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        // Step 1: Find the length of the list
        ListNode curr = head;
        int length = 1;
        while (curr.next != null) {
            length++;
            curr = curr.next;
        }

        // Step 2: Make it circular
        curr.next = head;

        // Step 3: Find new head after (length - k % length) steps
        k = k % length;
        int stepsToNewHead = length - k;

        // Step 4: Break the loop
        ListNode newTail = curr;
        while (stepsToNewHead-- > 0) {
            newTail = newTail.next;
        }

        ListNode newHead = newTail.next;
        newTail.next = null;

        return newHead;
    }
}
```

---

## 🧠 Why Your Code Fails

Your approach does:

* Reverse the full list
* Then tries to manually “rotate” by re-pointing the last node repeatedly — but this breaks the list’s structure and causes incorrect output or `null` dereference

---

## 🔍 How to Identify the Pattern / Choose the Algorithm

| Scenario                             | Pattern                                          |
| ------------------------------------ | ------------------------------------------------ |
| Need to move nodes from end to front | Use pointer manipulation                         |
| Need to rotate list                  | Think of it as converting to a **circular list** |
| `k` can be larger than length        | Use `%` to reduce unnecessary work               |

---

## 🚀 Step-by-Step Dry Run

### Input: `1 → 2 → 3 → 4 → 5`, `k = 2`

1. Length = 5
2. `k = 2 % 5 = 2`
3. `stepsToNewHead = 5 - 2 = 3`
4. Make list circular: `5 → 1`
5. Move 3 steps from head:

    * After 3 steps, newTail = `3`, newHead = `4`
6. Break link after `3`: `3 → null`, `4 → 5 → 1 → 2 → 3`

**Final Output**: `4 → 5 → 1 → 2 → 3`

---

## 📌 Summary — Quick Revision

* ✅ Count list length
* ✅ Use `%` to reduce `k`
* ✅ Make list circular
* ✅ Find new tail at `n - k` position
* ✅ Break the link and return new head

---

## 🧮 Time & Space Complexity

| Metric   | Value                                          |
| -------- | ---------------------------------------------- |
| ⏱️ Time  | `O(n)` (1 pass to count, 1 to locate new head) |
| 💾 Space | `O(1)` (constant extra space)                  |

---

## 🌍 Real-Life Use Cases

* **Music Playlist Rotation** (shifting start point)
* **Circular Buffers** (log rotation)
* **Task Scheduling** (shifting process queues)

---

## ✅ If You Want to Fix Your Version

You need to:

* Avoid repeated manual re-linking in rotation loop
* Simply connect end → head, then break the link at the newTail

---

Let me know if you want this implemented in a recursive or doubly linked list variant!
