Here’s a **comprehensive Markdown explanation** for the `findDuplicate()` function using **Floyd’s Tortoise and Hare (Cycle Detection)** algorithm — a favorite in interviews at companies like Google, Facebook, Amazon.

---

# 🔍 Find the Duplicate Number — Cycle Detection Approach

---

## ✅ Problem Statement

> Given an array `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive, return the **duplicate number**.

* Must not modify the array.
* Must use **constant extra space**.
* Time complexity < `O(n²)`.

---

## 💡 Intuition

The problem seems unrelated to **cycle detection** at first, but here's the key idea:

* Treat the array as a **linked list**:

    * Each index is a node.
    * `nums[i]` gives the **next node**.
* Because one number is duplicated, **at least two indices will point to the same next value**, forming a **cycle**.

So, we use **Floyd’s Cycle Detection Algorithm** to find that cycle — and the entry point to the cycle is the **duplicate number**.

---

## 🔍 Pattern Recognition & Breakdown

| Clue                                               | Inference                                          |
| -------------------------------------------------- | -------------------------------------------------- |
| Values are in range `1..n` and array size is `n+1` | **Pigeonhole Principle** — duplicate is guaranteed |
| Don’t use extra space                              | Cannot use `HashSet`, must be in-place             |
| No modifications allowed                           | Avoid sorting                                      |
| Need efficient traversal                           | Model it as a **cycle in a linked list**           |

---

## 🛠️ Concepts & Patterns Used

| Concept / Pattern             | Why It’s Used                            |
| ----------------------------- | ---------------------------------------- |
| **Floyd’s Tortoise and Hare** | To detect cycles and find entry point    |
| **Linked List Cycle**         | Array as an implicit graph with pointers |
| **Two-Pointer Technique**     | For space-efficient traversal            |
| **Mathematical Guarantee**    | At least one duplicate exists            |

---

## 🧰 Full Code with Inline Comments

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;

        // Step 1: Detect cycle using Floyd's algorithm
        do {
            slow = nums[slow];             // move 1 step
            fast = nums[nums[fast]];       // move 2 steps
        } while (slow != fast);            // loop until a cycle is found

        // Step 2: Find entry point of the cycle (duplicate number)
        fast = 0;                          // reset fast to start
        while (slow != fast) {
            slow = nums[slow];             // move both by 1 step
            fast = nums[fast];
        }

        return fast; // or return slow; both point to the duplicate
    }
}
```

---

## 🔂 Step-by-Step Dry Run

### Input:

```java
nums = [1, 3, 4, 2, 2]
```

### Linked List View:

Each index `i` points to `nums[i]`:

* 0 → 1
* 1 → 3
* 3 → 2
* 2 → 4
* 4 → 2 ← forms a cycle!

### Execution:

#### Phase 1: Detect intersection

```
slow = nums[0] = 1  
fast = nums[nums[0]] = nums[1] = 3

slow = nums[1] = 3  
fast = nums[nums[3]] = nums[2] = 4

slow = nums[3] = 2  
fast = nums[nums[4]] = nums[2] = 4

slow = nums[2] = 4  
fast = nums[nums[2]] = nums[4] = 2

slow = nums[4] = 2  
fast = nums[nums[4]] = nums[2] = 4

...eventually slow == fast at `2`
```

#### Phase 2: Find entrance

```
Reset fast = 0

slow = nums[2] = 4  
fast = nums[0] = 1

slow = nums[4] = 2  
fast = nums[1] = 3

slow = nums[2] = 4  
fast = nums[3] = 2

slow = nums[4] = 2  
fast = nums[2] = 4

Both meet at 2 → Duplicate found ✅
```

---

## 🧠 Real-Life Analogy

Imagine a **train track loop**:

* Each station (index) has one outbound train (points to next index).
* If there's a duplicate station in the schedule (duplicate number), some passengers will loop endlessly.

We detect this loop using two pointers moving at different speeds.

---

## ⏱️ Time and Space Complexity

| Metric    | Complexity                                                |
| --------- | --------------------------------------------------------- |
| **Time**  | `O(n)` — linear, each pointer traverses at most `n` steps |
| **Space** | `O(1)` — constant space, no extra data structures used    |

---

## 📌 Summary: Quick Revision Points

* 🚀 Use **Floyd’s Cycle Detection Algorithm** (Tortoise and Hare)
* 🧭 View array as a graph: `nums[i]` is the **next pointer**
* 🔁 Cycle exists because of duplicate (pigeonhole principle)
* 🎯 Entry point of cycle is the **duplicate number**
* ⏱️ Time: `O(n)`, 📦 Space: `O(1)`

---

Let me know if you'd like:

* 📊 Visualization of pointer movement
* 🔄 Alternative approaches using sorting or HashSet (space–time tradeoffs)
* 🧠 Interview follow-ups for similar cycle detection problems (e.g., Linked List Cycle II)
