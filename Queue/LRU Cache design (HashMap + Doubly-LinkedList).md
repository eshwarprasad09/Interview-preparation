Here’s a complete, **interview-grade** explanation and implementation of the **LRU Cache** using **HashMap + Doubly Linked List**.

---

# 🔁 LRU Cache Design using HashMap + Doubly Linked List

---

## ✅ Problem Statement

Design a data structure that:

* Supports **get(key)** and **put(key, value)** in **O(1)** time.
* Removes the **Least Recently Used (LRU)** entry when capacity is exceeded.

---

## 🧠 Intuition

We want:

* **Fast access** to cache entries ⇒ use **HashMap**.
* **Fast order update** (on access/put) ⇒ use **Doubly Linked List** to track usage order.

### Key Idea:

* Use a **HashMap** for O(1) lookups.
* Use a **Doubly Linked List** to maintain order of usage:

    * Most recently used → move to front
    * Least recently used → remove from end

---

## 🧱 Design Overview

| Component            | Responsibility                            |
| -------------------- | ----------------------------------------- |
| `Map<K, Node>`       | Stores key → Node mapping for O(1) access |
| `Doubly Linked List` | Tracks usage order (head: most recent)    |
| `head` & `tail`      | Dummy nodes to simplify insertion/removal |

---

## 🛠️ Java Code with Full Inline Comments

```java
class LRUCache {

    // Node class for doubly linked list
    class Node {
        int key, value;
        Node prev, next;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int capacity;
    private final Map<Integer, Node> map;
    private final Node head, tail; // Dummy nodes

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();

        // Initialize dummy head and tail nodes
        head = new Node(0, 0);
        tail = new Node(0, 0);

        head.next = tail;
        tail.prev = head;
    }

    // Remove a node from DLL
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Insert node right after head (most recently used position)
    private void insertAtFront(Node node) {
        node.next = head.next;
        node.prev = head;

        head.next.prev = node;
        head.next = node;
    }

    // Get operation
    public int get(int key) {
        if (!map.containsKey(key)) return -1;

        Node node = map.get(key);

        // Move accessed node to front
        remove(node);
        insertAtFront(node);

        return node.value;
    }

    // Put operation
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            // Update existing node
            Node node = map.get(key);
            node.value = value;

            // Move to front
            remove(node);
            insertAtFront(node);
        } else {
            // Add new node
            if (map.size() == capacity) {
                // Evict least recently used from tail
                Node lru = tail.prev;
                remove(lru);
                map.remove(lru.key);
            }

            Node node = new Node(key, value);
            insertAtFront(node);
            map.put(key, node);
        }
    }
}
```

---

## 🔂 Step-by-Step Dry Run

### Input:

```java
LRUCache cache = new LRUCache(2);
cache.put(1, 1);   // cache: {1}
cache.put(2, 2);   // cache: {2, 1}
cache.get(1);      // returns 1, cache: {1, 2}
cache.put(3, 3);   // evicts 2, cache: {3, 1}
cache.get(2);      // returns -1 (not found)
```

---

## 📚 Concepts & Patterns Used

| Concept            | Why Used                           |
| ------------------ | ---------------------------------- |
| HashMap            | O(1) access to key-value           |
| Doubly Linked List | O(1) update of LRU order           |
| Dummy Nodes        | Simplifies edge-case insert/delete |

---

## 🧠 How to Choose This Approach

* ✅ When **fast access & eviction** are both required
* ✅ Cache problems involving **Least Recently Used** strategy
* ✅ When **O(1)** operations are required for **get/put**

---

## ⏱️ Time and Space Complexity

| Operation | Time | Space       |
| --------- | ---- | ----------- |
| get/put   | O(1) | O(capacity) |
| DLL       | O(1) | O(capacity) |

---

## 💡 Real-Life Use Cases

* CPU/memory caches
* Browser history / tab management
* Mobile image loading frameworks (e.g., Glide, Picasso)
* Session/token management in web servers

---

## 📌 Summary (Quick Revision)

* 🧠 Use `HashMap<K, Node>` + `Doubly Linked List` (MRU ↔ LRU)
* 🔁 On `get/put`, move node to front (most recently used)
* 🧹 On overflow, remove node before `tail` (least recently used)
* ⏱️ Time: `O(1)` for both operations
* 🧠 Use dummy `head/tail` for simpler pointer logic

---

Let me know if you want:

* 🔁 Python version with `OrderedDict`
* 🧪 Unit test cases
* ✏️ LRU with custom eviction strategies (LFU, MRU, TTL)
