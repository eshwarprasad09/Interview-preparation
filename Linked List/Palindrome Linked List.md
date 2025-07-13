Here's a complete explanation in **Markdown format** for the problem **"Check if a Linked List is a Palindrome"**, including step-by-step intuition, algorithm breakdown, concepts, and dry run.

---

# ✅ Check if a Singly Linked List is a Palindrome

---

## 📘 Problem Statement

Given the `head` of a singly linked list, return `true` if the list is a palindrome or `false` otherwise.

---

## 💡 Intuition

A palindrome is a sequence that reads the same backward as forward.
For example, `1 → 2 → 2 → 1` is a palindrome.

Since singly linked lists can only be traversed **in one direction**, we can’t compare from both ends directly.

**Key Insight**:

* Find the **middle** of the list using **fast and slow pointers**
* **Reverse the second half**
* Compare the **first half** and the **reversed second half**

This approach is **efficient in time and space**.

---

## 🔧 Concepts & Patterns Used

* 🐢🐇 **Fast & Slow Pointer** (to find the midpoint)
* 🔁 **Reverse a Linked List**
* 🔍 **Two-pointer comparison**
* 🧠 **In-place algorithm** — No need for extra space (like stack or array)

---

## 🔍 How to Identify the Pattern / Algorithm

| Clue                                  | Pattern                                     |
| ------------------------------------- | ------------------------------------------- |
| Need to find the middle node          | Use fast and slow pointers                  |
| Need to check symmetry in a list      | Compare first half and reversed second half |
| Singly linked list, can't go backward | Reverse part of the list                    |

---

## 🔄 Step-by-Step Code Walkthrough

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null){
            return true; // 0 or 1 node is always a palindrome
        }

        // Step 1: Find middle using fast & slow
        ListNode slow = head;
        ListNode fast = head;
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        // Step 2: Reverse the second half
        slow.next = reverseList(slow.next);
        slow = slow.next; // move to start of second half

        // Step 3: Compare both halves
        while(slow != null){
            if(head.val != slow.val) return false;
            head = head.next;
            slow = slow.next;
        }

        return true;
    }

    // Helper to reverse a linked list
    ListNode reverseList(ListNode head){
        ListNode pre = null;
        ListNode next = null;
        while(head != null){
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
}
```

---

## 🧮 Step-by-Step Dry Run

### Input: `1 → 2 → 2 → 1`

#### Step 1: Fast & Slow to Middle

* After loop:

    * `slow` at 1st `2` (middle)
    * `fast` at last node

#### Step 2: Reverse Second Half

* Second Half: `2 → 1` becomes `1 → 2`

#### Step 3: Compare

```
head = 1, slow = 1 → match
head = 2, slow = 2 → match
```

Return `true`

---

## ⚡ Time & Space Complexity

| Complexity | Value                                                         |
| ---------- | ------------------------------------------------------------- |
| 🕒 Time    | `O(n)` — Full traversal to midpoint, reversal, and comparison |
| 💾 Space   | `O(1)` — In-place reversal, no extra storage                  |

---

## 📌 Summary — Quick Revision Bullet Points

* Use **fast and slow pointer** to find midpoint
* **Reverse second half** of list
* **Compare** values in first and second half
* Return `false` if mismatch found, else return `true`
* Time: `O(n)`, Space: `O(1)`

---

## 🧠 Real-Life Use Case

* **Palindrome Detection in Streams**: Check if incoming data (like logs or keystrokes) forms a palindrome.
* **DNA/RNA Matching**: Some genetic sequences need to be checked for symmetry.
* **Text Editors**: Detecting palindromic words while typing (e.g., `madam`, `racecar`).

---

Let me know if you want the version where the list should be restored after checking (for read-only scenarios).
