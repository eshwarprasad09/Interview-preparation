Here's a complete explanation of the **Quick Sort** algorithm with code, dry run, intuition, pattern breakdown, real-world use cases, and complexity analysis.

---

# ⚡ Quick Sort Algorithm

---

## ✅ Problem Statement

Sort a given array using the **Quick Sort** algorithm.

Example:

```java
Input:  [5, 3, 7, 6, 2, 9]
Output: [2, 3, 5, 6, 7, 9]
```

---

## 🧠 Intuition (Layman Explanation)

* Think of QuickSort as **divide-and-conquer** sorting.
* Choose a number as a **pivot**.
* Move all **smaller elements to the left**, larger to the right.
* Recursively repeat this for left and right halves.

Imagine you're organizing books on a shelf. Pick one book (pivot). Move all books shorter than it to the left, taller ones to the right. Repeat this process on each half.

---

## 🧩 Pattern Identification

| Feature                         | Concept / Pattern           |
| ------------------------------- | --------------------------- |
| Divide problem into subproblems | Divide and Conquer          |
| Recursive sorting of partitions | Recursion                   |
| Sort in-place                   | In-place Array manipulation |
| Choose pivot and partition      | Partitioning                |

---

## 🔍 When to Use Quick Sort

* When in-place sorting is needed (no extra memory).
* When average-case performance matters more than worst-case.
* When the dataset is large and random.

---

## ✅ Java Code (with Inline Comments)

```java
class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            // Partition the array and get the pivot index
            int pivotIndex = partition(arr, low, high);
            
            // Recursively sort elements before and after partition
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    // Partition function - Lomuto partition scheme
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];  // Choose the last element as pivot
        int i = low - 1;        // Place for smaller element
        
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                // Swap arr[i] and arr[j]
                int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
            }
        }
        // Swap pivot into correct position
        int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;
        return i + 1; // Return pivot index
    }
}
```

---

## 🔁 Dry Run: `[5, 3, 7, 6, 2, 9]`

**First call:**
Pivot = 9
→ All elements ≤ 9, so no swaps needed till end
→ Put 9 in correct position (last)
→ Pivot index = 5

Recurse on:

* Left = `[5, 3, 7, 6, 2]`
* Right = none (already sorted)

**Next pivot = 2**
→ Move 2 to front
→ Pivot index = 0

Continue recursively on smaller partitions until all are sorted.

---

## 📚 Concepts & Patterns Used

| Concept / Pattern  | Purpose                         |
| ------------------ | ------------------------------- |
| Divide and Conquer | Break problem into subarrays    |
| Partitioning       | Place pivot at correct location |
| Recursion          | Solve left and right parts      |
| In-place sorting   | No need for extra memory        |

---

## 🧾 Step-by-Step Algorithm (Layman's Terms)

1. Pick a pivot (usually last element).
2. Rearrange array so:

    * All elements < pivot are left.
    * All elements > pivot are right.
3. Recursively repeat this for left and right halves.
4. Eventually, all elements are in correct position.

---

## 🧪 Time & Space Complexity

| Case    | Time Complexity  | Explanation                         |
| ------- | ---------------- | ----------------------------------- |
| Best    | `O(n log n)`     | Balanced partitioning               |
| Average | `O(n log n)`     | On average, good splits             |
| Worst   | `O(n^2)`         | If pivot always smallest/largest    |
| Space   | `O(log n)` (avg) | For recursive stack (in-place sort) |

> Worst-case occurs on sorted/reverse arrays if pivot is poorly chosen.

---

## 💼 Real-Life Use Cases

* Language libraries often use QuickSort (C++ STL `std::sort`).
* Efficient sorting in databases or logs.
* Sorting in systems where extra memory is limited.

---

## 📌 Summary for Quick Revision

* 🧠 Core idea: partition the array using a pivot.
* ⛓ Partition: left of pivot < pivot < right of pivot.
* 🔁 Recurse on subarrays.
* ✅ In-place, average-case efficient.
* ⚠️ Worst-case = `O(n^2)`, but can be mitigated by random pivoting.

---

## 🏷 Category

| Belongs To       | Why                               |
| ---------------- | --------------------------------- |
| ✅ Sorting        | Main purpose is to sort array     |
| ✅ Divide-Conquer | Uses recursive splitting of array |
| ✅ Recursion      | Repeats steps on subarrays        |

---

Let me know if you'd like:

* 🔀 QuickSort with random pivoting (to avoid worst-case)
* 🧠 Comparison with Merge Sort or Heap Sort
* 📈 Visual pivot-based step-by-step animation
