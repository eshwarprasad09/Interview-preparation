Sure! Here is a complete explanation of the **Queue data structure** with all its essential methods, types, real-life analogies, and Java implementation — perfect for interviews and concept revision.

---

# 📦 Queue Data Structure – Complete Guide

---

## 🚦 What is a Queue?

A **Queue** is a linear data structure that follows the **FIFO** (First In First Out) principle.

> 🔁 Think of people standing in a line:
>
> * First person to get in line is the first to be served.

---

## 📌 Queue Properties

| Feature       | Description                    |
| ------------- | ------------------------------ |
| **Ordering**  | First-In-First-Out (FIFO)      |
| **Insertion** | At the **rear** (end)          |
| **Deletion**  | From the **front** (start)     |
| **Access**    | Only front element (no random) |

---

## 🧠 Real-Life Examples

* Printer queue
* Call center waiting line
* Task scheduling
* Traffic queues at toll booths

---

## 🛠️ Core Queue Operations

| Operation    | Description                                |
| ------------ | ------------------------------------------ |
| `enqueue(x)` | Add element `x` to the **rear**            |
| `dequeue()`  | Remove element from the **front**          |
| `peek()`     | View the front element without removing it |
| `isEmpty()`  | Check if the queue is empty                |
| `size()`     | Returns the number of elements in queue    |

---

## 🧪 Java Implementation using `Queue` Interface

Java’s `Queue` interface is part of the `java.util` package and is typically implemented by **LinkedList** or **ArrayDeque**.

### ✅ Using LinkedList

```java
Queue<Integer> queue = new LinkedList<>();
```

### ✅ Using ArrayDeque (Recommended – faster than LinkedList)

```java
Queue<Integer> queue = new ArrayDeque<>();
```

---

### 🔧 Example: Java Queue Methods

```java
Queue<Integer> queue = new LinkedList<>();

queue.add(1);       // enqueue
queue.add(2);
queue.add(3);

System.out.println(queue); // [1, 2, 3]

System.out.println(queue.poll()); // dequeue → 1
System.out.println(queue.peek()); // peek → 2
System.out.println(queue.isEmpty()); // false
System.out.println(queue.size()); // 2
```

---

## 🔁 Under the Hood (How Queue Works)

Internally, a queue can be implemented using:

* **Array** (Fixed size)
* **Linked List** (Dynamic size)

---

## 🧱 Custom Implementation of Queue using Array

```java
class MyQueue {
    int front = 0, rear = 0, size = 0;
    int[] arr;

    public MyQueue(int capacity) {
        arr = new int[capacity];
    }

    public void enqueue(int val) {
        if (size == arr.length) throw new RuntimeException("Queue Full");
        arr[rear] = val;
        rear = (rear + 1) % arr.length;
        size++;
    }

    public int dequeue() {
        if (isEmpty()) throw new RuntimeException("Queue Empty");
        int val = arr[front];
        front = (front + 1) % arr.length;
        size--;
        return val;
    }

    public int peek() {
        if (isEmpty()) throw new RuntimeException("Queue Empty");
        return arr[front];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public int size() {
        return size;
    }
}
```

---

## 🌀 Types of Queues

| Type                     | Description                                   |
| ------------------------ | --------------------------------------------- |
| **Simple Queue**         | Normal FIFO queue                             |
| **Circular Queue**       | Rear wraps around to front if space available |
| **Deque (Double Ended)** | Insert/delete at both ends                    |
| **Priority Queue**       | Elements are ordered by priority, not FIFO    |
| **Blocking Queue**       | Used in multithreading (Java Concurrency)     |

---

## 📚 Summary – For Quick Revision

* **Queue = FIFO**
* Methods: `add()`, `poll()`, `peek()`, `isEmpty()`, `size()`
* Implementations: `LinkedList`, `ArrayDeque`, custom array/linked list
* Used in: BFS, Scheduling, Message Queues, Producer-Consumer
* Interview types: Simple, Circular, Deque, PriorityQueue

---

## 🧠 Interview Questions You Might Get

* Implement a queue using two stacks
* Implement a circular queue
* Design a queue that supports `min()` in O(1)
* Use queue to perform BFS
* Implement LRU Cache using Queue + HashMap

---

Would you like the custom implementation of **Circular Queue** or **Queue using Stacks** next?
