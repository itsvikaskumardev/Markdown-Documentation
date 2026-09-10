# Kth Largest Element in an Array

**LeetCode #215** · [LeetCode](https://leetcode.com/problems/kth-largest-element-in-an-array/) · **Medium**

> **Array · Top K · Min-Heap of Size K**

### Approaches

#### 1. Brute Force (Sort)
Sort the array in ascending order and return `arr[n − K]`.

**Pseudo Code:**

```text id="e4k8pn"
given arr, k

sort arr ascending

return arr[n − k]
```

#### 2. Optimized (Size-K Min-Heap)
Maintain a **min-heap of size `K`**. Add each element and remove the smallest when the heap exceeds `K`. The heap's top is the **Kth largest** element.

**Pseudo Code:**

```text id="m8v3qx"
given arr, k

heap = empty min-heap

for x in arr:
    if heap.size < k: push x
    else if x > heap.top:
        pop the smallest …
        … and push x
    else: skip

return heap.top    // the k-th largest
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort** | **O(n log n)** | **O(1)** |
| **Optimized · Size-K Min-Heap** | **O(n log K)** | **O(K)** |

-------------

# K Closest Points to Origin

**LeetCode #973** · [LeetCode](https://leetcode.com/problems/k-closest-points-to-origin/) · **Medium**

> **Array · Top K · Max-Heap of Size K · Squared Distance**

### Approaches

#### 1. Brute Force (Sort by Distance)
Calculate `dist² = x² + y²` for each point, sort by distance, and return the first `K` points. No square root is needed.

**Pseudo Code:**

```text id="r4k9mt"
given points, k

compute dist² = x² + y² for each

sort points by dist²

return the first k
```

#### 2. Optimized (Size-K Max-Heap)
Maintain a **max-heap of size `K`** using squared distance. If the heap exceeds `K`, remove the farthest point. The heap contains the `K` closest points.

**Pseudo Code:**

```text id="v6p2nx"
given points, k

heap = empty max-heap on dist²

for p in points:
    if heap.size < k: push p
    else if dist(p) < heap.top:
        pop the farthest …
        … and push p
    else: skip

return heap contents
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort by Distance** | **O(n log n)** | **O(n)** |
| **Optimized · Size-K Max-Heap** | **O(n log K)** | **O(K)** |

---

# K Closest Points to Origin

**LeetCode #973** · [LeetCode](https://leetcode.com/problems/k-closest-points-to-origin/) · **Medium**

> **Array · Top K · Max-Heap of Size K · Squared Distance**

### Approaches

#### 1. Brute Force (Sort by Distance)
Calculate `dist² = x² + y²` for each point, sort by distance, and return the first `K` points. No square root is needed.

**Pseudo Code:**

```text id="r4k9mt"
given points, k

compute dist² = x² + y² for each

sort points by dist²

return the first k
```

#### 2. Optimized (Size-K Max-Heap)
Maintain a **max-heap of size `K`** using squared distance. If the heap exceeds `K`, remove the farthest point. The heap contains the `K` closest points.

**Pseudo Code:**

```text id="v6p2nx"
given points, k

heap = empty max-heap on dist²

for p in points:
    if heap.size < k: push p
    else if dist(p) < heap.top:
        pop the farthest …
        … and push p
    else: skip

return heap contents
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort by Distance** | **O(n log n)** | **O(n)** |
| **Optimized · Size-K Max-Heap** | **O(n log K)** | **O(K)** |

---

# Find K Closest Elements

**LeetCode #658** · [LeetCode](https://leetcode.com/problems/find-k-closest-elements/) · **Medium**

> **Array · Top K · Max-Heap of Size K · Distance + Value**

### Approaches

#### 1. Brute Force (Sort by Distance)
Rank each element by `(|a − x|, a)`, take the best `K` elements, then sort them to return the result in ascending order.

**Pseudo Code:**

```text
given arr, x, k

rank each value by (|a − x|, a)

take the best k

return them sorted
```

#### 2. Optimized (Size-K Max-Heap)
Maintain a **max-heap of size `K`** using `(distance, value)` as the key. If the heap exceeds `K`, remove the farthest element; for equal distances, remove the larger value.

**Pseudo Code:**

```text
given arr, x, k

heap = max-heap on (|a − x|, a)

for a in arr:
    if heap.size < k: push a
    else if (dist,a) better than top:
        pop the worst …
        … and push a
    else: skip

return heap sorted
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort by Distance** | **O(n log n)** | **O(n)** |
| **Optimized · Size-K Max-Heap** | **O(n log K)** | **O(K)** |

---

# Merge K Sorted Lists

**LeetCode #23** · [LeetCode](https://leetcode.com/problems/merge-k-sorted-lists/) · **Hard**

> **Linked List · Min-Heap · Merge K Sorted Lists**

### Approaches

#### 1. Brute Force (Scan K Heads Each Round)
Compare the current head of all `k` lists, pick the smallest, add it to the output, and advance that list.

**Pseudo Code:**

```text
given k sorted lists

repeat until all empty:

    scan the ≤ k current heads

    move the minimum to output

    advance that list
```

#### 2. Optimized (Min-Heap of Heads)
Keep the current head of each non-empty list in a min-heap; repeatedly extract the smallest node and push its next node.

**Pseudo Code:**

```text
heap = min-heap; push every list head

while heap not empty:

    (v, list) = pop the root

    append v to output

    if list has a next head:

        push it

    // else list is exhausted

return output
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Scan K Heads Each Round** | **O(N · k)** | **O(1)** |
| **Optimized · Min-Heap of Heads** | **O(N log k)** | **O(k)** |

---

# Median from Data Stream

**LeetCode #295** · [LeetCode](https://leetcode.com/problems/find-median-from-data-stream/) · **Hard**

> **Heap · Two Heaps · Keep the Middle Elements Separated**

### Approaches

#### 1. Brute Force (Keep a Sorted List)
Insert each number in sorted order, then return the middle element(s).

**Pseudo Code:**

```text
sorted = []

on addNum(x):

    insert x keeping sorted order    // O(n)

on findMedian():

    return middle (or avg of two middles)
```

#### 2. Optimized (Max-Heap + Min-Heap)
Use a max-heap for the smaller half and a min-heap for the larger half; keep their sizes balanced so the median is always at the tops.

**Pseudo Code:**

```text
lo = max-heap (lower half)

hi = min-heap (upper half)

on addNum(x):

    if x ≤ lo.top: lo.push(x) else hi.push(x)

    // rebalance sizes (differ by ≤ 1)

    if lo.size > hi.size+1: hi.push(lo.pop)

    elif hi.size > lo.size: lo.push(hi.pop)

on findMedian():

    equal sizes → (lo.top + hi.top)/2  else lo.top
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Keep a Sorted List** | **O(n²)** | **O(n)** |
| **Optimized · Max-Heap + Min-Heap** | **Add: O(log n) · Find: O(1)** | **O(n)** |

---
