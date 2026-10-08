# Table of Contents

- [Kth Largest Element in an Array](#kth-largest-element-in-an-array)
- [K Closest Points to Origin](#k-closest-points-to-origin)
- [K Closest Points to Origin](#k-closest-points-to-origin)
- [Find K Closest Elements](#find-k-closest-elements)
- [Merge K Sorted Lists](#merge-k-sorted-lists)
- [Median from Data Stream](#median-from-data-stream)

---

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

### Dry Run
This pseudocode finds the **k-th largest element** in the array.

Given:

```text id="3x9wq2"
arr = [3,2,1,5,6,4]
k = 2
```

### Pseudocode

```text id="2h0p8a"
given arr, k

sort arr ascending

return arr[n − k]
```

### Line-by-line explanation

#### 1. `given arr, k`

We are given:

```text id="b8z7y1"
arr = [3,2,1,5,6,4]
k = 2
```

`k = 2` means we want the **2nd largest element**.

---

#### 2. `sort arr ascending`

Arrange the array from **smallest → largest**:

Before:

```text id="r7c1m4"
[3,2,1,5,6,4]
```

After sorting:

```text id="5q2v8k"
[1,2,3,4,5,6]
```

There are:

```text id="n4t6p0"
n = 6
```

The indices are:

```text id="c1y8v3"
Index:  0  1  2  3  4  5
Value:  1  2  3  4  5  6
```

---

#### 3. `return arr[n − k]`

Now calculate:

```text id="j5r2w9"
n - k
= 6 - 2
= 4
```

So we need:

```text id="x3m7q1"
arr[4]
```

From the sorted array:

```text id="z9k4p6"
arr[4] = 5
```

Therefore:

### ✅ Final Output

```text id="a6v2r8"
5
```

### 🧠 Why `n - k`?

For ascending array:

```text
[1, 2, 3, 4, 5, 6]
 ↑           ↑  ↑
smallest     2nd largest
```

* Largest = index `n-1` = `5` → `6`
* 2nd largest = index `n-2` = `4` → `5`
* 3rd largest = index `n-3` = `3` → `4`

So generally:

```text
k-th largest → arr[n - k]
```

**Time Complexity:** `O(n log n)` because of sorting.
**Extra Space:** depends on the sorting algorithm, typically `O(log n)` for in-place comparison sort.


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
### Dry Run
This pseudocode finds the **k-th largest element** using a **min-heap**.

Given:

```text
arr = [3,2,3,1,2,4,5,5,6]
k = 4
```

We need to find the **4th largest element**.

### Pseudocode

```text
heap = empty min-heap

for x in arr:
    if heap.size < k:
        push x
    else if x > heap.top:
        pop the smallest
        push x
    else:
        skip

return heap.top
```

### 1. `heap = empty min-heap`

Initially:

```text
heap = []
```

A **min-heap** always keeps the **smallest element at the top**.

So:

```text
heap.top = smallest element in heap
```

We only want to keep the **largest 4 elements**, so the heap size will never exceed `k = 4`.

---

### 2. Process each element

#### `x = 3`

Heap size is `0`, which is less than `k = 4`.

```text
push 3
```

```text
heap = [3]
```

---

#### `x = 2`

Size `1 < 4`.

Push:

```text
heap = [2,3]
```

Because it is a min-heap:

```text
heap.top = 2
```

---

#### `x = 3`

Size `2 < 4`.

Push:

```text
heap = [2,3,3]
```

---

#### `x = 1`

Size `3 < 4`.

Push:

```text
heap = [1,2,3,3]
```

Now heap size is `4`.

The smallest element is:

```text
heap.top = 1
```

---

#### `x = 2`

Now:

```text
heap.size = 4
k = 4
```

So `heap.size < k` is false.

Check:

```text
x > heap.top
2 > 1
```

True ✅

So:

```text
pop the smallest → remove 1
push 2
```

Heap now contains:

```text
[2,2,3,3]
```

---

#### `x = 4`

Check:

```text
4 > heap.top
4 > 2
```

True.

Remove smallest `2`:

```text
[2,3,3]
```

Push `4`:

```text
[2,3,3,4]
```

Now the smallest of these 4 largest candidates is `2`.

---

#### `x = 5`

Check:

```text
5 > 2
```

True.

Pop `2`:

```text
[3,3,4]
```

Push `5`:

```text
[3,3,4,5]
```

---

#### `x = 5`

Check:

```text
5 > 3
```

True.

Pop smallest `3`:

```text
[3,4,5]
```

Push `5`:

```text
[3,4,5,5]
```

---

#### `x = 6`

Check:

```text
6 > 3
```

True.

Pop smallest `3`:

```text
[4,5,5]
```

Push `6`:

```text
[4,5,5,6]
```

---

### Complete Dry Run

| `x` | Action        | Heap after action | `heap.top` |
| --: | ------------- | ----------------- | ---------: |
|   3 | push          | `[3]`             |          3 |
|   2 | push          | `[2,3]`           |          2 |
|   3 | push          | `[2,3,3]`         |          2 |
|   1 | push          | `[1,2,3,3]`       |          1 |
|   2 | pop 1, push 2 | `[2,2,3,3]`       |          2 |
|   4 | pop 2, push 4 | `[2,3,3,4]`       |          2 |
|   5 | pop 2, push 5 | `[3,3,4,5]`       |          3 |
|   5 | pop 3, push 5 | `[3,4,5,5]`       |          3 |
|   6 | pop 3, push 6 | `[4,5,5,6]`       |          4 |

At the end:

```text
heap = [4,5,5,6]
```

These are the **4 largest elements**:

```text
6 → largest
5 → 2nd largest
5 → 3rd largest
4 → 4th largest
```

Therefore:

```text
heap.top = 4
```

### ✅ Final Output

```text
4
```

### 🧠 Main idea

The trick is:

> Keep only the **k largest elements** in a min-heap.

For `k = 4`, the heap always contains the best 4 candidates, and the **smallest among those 4** is exactly the **4th largest element**.

**Time Complexity:** `O(n log k)`
**Space Complexity:** `O(k)`


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
