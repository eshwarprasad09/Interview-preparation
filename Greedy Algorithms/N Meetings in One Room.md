Here’s the complete explanation of the **“N Meetings in One Room”** problem in a structured **Markdown format** to help you understand and revise effectively.

---

# 🏢 N Meetings in One Room

---

## 💬 Problem Statement

You are given `N` meetings with their start and end times. All meetings take place in a **single room**. You need to find the **maximum number of meetings** that can be accommodated in the room **without any overlap**.

### Input:

* `start[]`: array of start times of `N` meetings
* `end[]`: array of end times of `N` meetings

### Output:

* Maximum number of non-overlapping meetings

---

## 🧠 Intuition

To **maximize the number of meetings**, we should always **pick the meeting that ends earliest** — this way, we leave the room free for future meetings.

This is a classic **Greedy Algorithm** problem.

---

## 💡 Concepts & Patterns Used

* **Greedy Algorithm**
* **Activity Selection Problem**
* **Sorting by end time**

---

## 🧩 How to Identify the Pattern

| Clue in Problem Statement      | Pattern to Use     |
| ------------------------------ | ------------------ |
| “Maximum number of activities” | Greedy             |
| “No two meetings overlap”      | Sort by end time   |
| Only **one room available**    | Activity Selection |

---

## ✅ Step-by-Step Approach

### Step 1: Create a data structure to store meetings

Each meeting should have:

* Start time
* End time
* (Optional) Index for tracking

### Step 2: Sort all meetings by their **end time**

### Step 3: Greedily select meetings

* Select the meeting if its start time > last selected meeting’s end time.

---

## ✅ Java Code

```java
class Meeting {
    int start, end;

    Meeting(int start, int end) {
        this.start = start;
        this.end = end;
    }
}

class Solution {
    public static int maxMeetings(int[] start, int[] end, int n) {
        List<Meeting> meetings = new ArrayList<>();

        // Step 1: Store start and end time in Meeting object
        for (int i = 0; i < n; i++) {
            meetings.add(new Meeting(start[i], end[i]));
        }

        // Step 2: Sort by end time
        meetings.sort((a, b) -> a.end - b.end);

        int count = 1;
        int endTime = meetings.get(0).end;

        // Step 3: Select meetings greedily
        for (int i = 1; i < n; i++) {
            if (meetings.get(i).start > endTime) {
                count++;
                endTime = meetings.get(i).end;
            }
        }

        return count;
    }
}
```

---

## 🧪 Step-by-Step Dry Run

### Input:

```text
start = [1, 3, 0, 5, 8, 5]
end   = [2, 4, 6, 7, 9, 9]
```

### Sorted Meetings by End Time:

```
[1,2], [3,4], [0,6], [5,7], [8,9], [5,9]
```

### Selected Meetings:

* Pick \[1,2] ✅
* Pick \[3,4] ✅
* Skip \[0,6] ❌ (0 < 4)
* Pick \[5,7] ✅
* Pick \[8,9] ✅
* Skip \[5,9] ❌

✅ **Total = 4 meetings**

---

## 📌 Summary (Quick Revision)

* Pair start and end into objects
* Sort by end time (Greedy Strategy)
* Count meetings with no overlap
* Time Complexity: **O(N log N)** due to sorting
* Space Complexity: **O(N)**

---

## ⏱️ Time & Space Complexity

| Metric              | Value                               |
| ------------------- | ----------------------------------- |
| 🕒 Time Complexity  | `O(N log N)`                        |
| 💾 Space Complexity | `O(N)` (or `O(1)` if done in-place) |

---

## 📅 Real-Life Use Case

* Scheduling conference rooms in companies
* CPU job scheduling with non-overlapping time slots
* Interview room slot planning

---

Would you like to also see a **variation where meetings can have priorities** or where **rooms are more than one** (like *Meeting Rooms II*)?
