Here’s a concise and **interview-friendly implementation** of both **Min Heap** and **Max Heap** using Java's `PriorityQueue`, and also with a **custom array-based approach** (if you're asked to implement one from scratch).

---

# 📦 Java Heap Implementation – Interview Style

## ✅ Using Java Built-in PriorityQueue (Quickest in Interviews)

```java
// Min Heap (Default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

### 🔹 Example Usage:

```java
minHeap.add(3);
minHeap.add(1);
minHeap.add(2);
System.out.println(minHeap.poll()); // prints 1 (min element)

maxHeap.add(3);
maxHeap.add(1);
maxHeap.add(2);
System.out.println(maxHeap.poll()); // prints 3 (max element)
```

---

## 🔧 Manual Heap Implementation (For System Design or DSA Interviews)

### 🧱 Min Heap from Scratch

```java
class MinHeap {
    private List<Integer> heap = new ArrayList<>();

    public void add(int val) {
        heap.add(val);
        siftUp(heap.size() - 1);
    }

    public int poll() {
        if (heap.isEmpty()) throw new NoSuchElementException();
        int min = heap.get(0);
        int last = heap.remove(heap.size() - 1);
        if (!heap.isEmpty()) {
            heap.set(0, last);
            siftDown(0);
        }
        return min;
    }

    private void siftUp(int index) {
        while (index > 0) {
            int parent = (index - 1) / 2;
            if (heap.get(index) < heap.get(parent)) {
                swap(index, parent);
                index = parent;
            } else break;
        }
    }

    private void siftDown(int index) {
        int size = heap.size();
        while (index < size) {
            int left = 2 * index + 1;
            int right = 2 * index + 2;
            int smallest = index;

            if (left < size && heap.get(left) < heap.get(smallest)) smallest = left;
            if (right < size && heap.get(right) < heap.get(smallest)) smallest = right;

            if (smallest == index) break;
            swap(index, smallest);
            index = smallest;
        }
    }

    private void swap(int i, int j) {
        int tmp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, tmp);
    }
}
```

---

### 🧱 Max Heap from Scratch

Same as MinHeap, but change `>` to `<` in comparisons.

```java
class MaxHeap {
    private List<Integer> heap = new ArrayList<>();

    public void add(int val) {
        heap.add(val);
        siftUp(heap.size() - 1);
    }

    public int poll() {
        if (heap.isEmpty()) throw new NoSuchElementException();
        int max = heap.get(0);
        int last = heap.remove(heap.size() - 1);
        if (!heap.isEmpty()) {
            heap.set(0, last);
            siftDown(0);
        }
        return max;
    }

    private void siftUp(int index) {
        while (index > 0) {
            int parent = (index - 1) / 2;
            if (heap.get(index) > heap.get(parent)) {
                swap(index, parent);
                index = parent;
            } else break;
        }
    }

    private void siftDown(int index) {
        int size = heap.size();
        while (index < size) {
            int left = 2 * index + 1;
            int right = 2 * index + 2;
            int largest = index;

            if (left < size && heap.get(left) > heap.get(largest)) largest = left;
            if (right < size && heap.get(right) > heap.get(largest)) largest = right;

            if (largest == index) break;
            swap(index, largest);
            index = largest;
        }
    }

    private void swap(int i, int j) {
        int tmp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, tmp);
    }
}
```

---

## 🧠 Summary for Interviews

| Heap Type | Java PriorityQueue                                | Custom Implementation |
| --------- | ------------------------------------------------- | --------------------- |
| Min Heap  | `new PriorityQueue<>()`                           | Compare `<` in sift   |
| Max Heap  | `new PriorityQueue<>(Collections.reverseOrder())` | Compare `>` in sift   |

---

## 💡 When Are Heaps Asked in Interviews?

* Top K Elements
* Median of Data Stream
* Kth Largest/Smallest Element
* Merge K Sorted Lists
* Scheduling problems (Jobs, Events, Meetings)

---

If you'd like templates for those problems or a practice set to master heap-based problems, let me know!
