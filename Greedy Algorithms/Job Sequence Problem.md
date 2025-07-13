Here is a complete explanation of the **Job Sequencing Problem** in clean **Markdown format**, including:

* Intuition
* Concepts
* Code (Java)
* Time & Space Complexity
* Real-life examples
* Step-by-step dry run

---

# 🧩 Job Sequencing Problem

---

## 💬 Problem Statement

Given `N` jobs where every job has:

* a **deadline**
* a **profit** (if job is finished before or on deadline)
* a **duration of 1 unit time**

Each job **can be done only once** and **only one job can be scheduled at a time**. Find the **maximum profit** and the number of jobs done.

---

## 🧠 Intuition

* To maximize profit, we should always choose **jobs with higher profits** first.
* But we can't violate the **deadline constraint**, so we need to **schedule profitable jobs as late as possible** before their deadline (to leave room for other earlier jobs).

---

## 💡 Concepts & Patterns Used

* **Greedy Algorithm**
* **Sorting**
* **Disjoint Set (optional for optimization)**

---

## 🔍 How to Identify the Pattern

| Problem Clue                     | Pattern                |
| -------------------------------- | ---------------------- |
| Maximize profit under a deadline | Greedy + Sorting       |
| Only 1 unit of time per job      | Use a time slot array  |
| Schedule jobs with constraints   | Job Scheduling Problem |

---

## ✅ Approach (Greedy + Time Slot)

1. **Sort jobs** in **descending order** of profit.
2. Use a **boolean array `slots[]`** to mark free time slots (size = max deadline).
3. For each job:

    * Try to find the **latest available slot before or on its deadline**
    * If found → schedule it and add to profit
4. Count total jobs done and total profit

---

## ✅ Java Code

```java
class Job {
    int id, deadline, profit;

    Job(int id, int deadline, int profit) {
        this.id = id;
        this.deadline = deadline;
        this.profit = profit;
    }
}

class Solution {
    public int[] jobScheduling(Job[] jobs, int n) {
        // Sort jobs in decreasing order of profit
        Arrays.sort(jobs, (a, b) -> b.profit - a.profit);

        // Find max deadline to create time slots
        int maxDeadline = 0;
        for (Job job : jobs) {
            maxDeadline = Math.max(maxDeadline, job.deadline);
        }

        boolean[] slots = new boolean[maxDeadline + 1];
        int jobCount = 0, totalProfit = 0;

        for (Job job : jobs) {
            // Try to find a free slot from deadline to 1
            for (int j = job.deadline; j > 0; j--) {
                if (!slots[j]) {
                    slots[j] = true;
                    jobCount++;
                    totalProfit += job.profit;
                    break;
                }
            }
        }

        return new int[]{jobCount, totalProfit};
    }
}
```

---

## 🔁 Dry Run Example

### Input:

```java
jobs = [
    Job(id=1, deadline=4, profit=20),
    Job(id=2, deadline=1, profit=10),
    Job(id=3, deadline=1, profit=40),
    Job(id=4, deadline=1, profit=30)
]
```

### Step-by-Step:

* Sort by profit: Job 3 (40), Job 4 (30), Job 1 (20), Job 2 (10)
* Job 3 → Deadline 1 → Slot 1 free → ✅
* Job 4 → Deadline 1 → Slot 1 occupied → ❌
* Job 1 → Deadline 4 → Slot 4 free → ✅
* Job 2 → Deadline 1 → Slot 1 occupied → ❌

### Output:

```java
Jobs done = 2
Total profit = 60
```

---

## 📌 Summary (Quick Revision)

* Sort jobs by **profit descending**
* Try to schedule each job in the **latest available time slot before its deadline**
* Greedy strategy works best

---

## ⏱️ Time & Space Complexity

| Metric                 | Value              |
| ---------------------- | ------------------ |
| ⏳ Time Complexity      | O(N log N + N × D) |
| 💾 Space Complexity    | O(D) (for slots)   |
| Where D = max deadline |                    |

---

## 📅 Real-Life Applications

* Task scheduling in OS
* Job allocation in distributed systems
* Deadline-based bidding systems
* Advertisement scheduling for max revenue

---

Would you like the **Disjoint Set version** for further optimization from `O(N*D)` to `O(N log N)`?
