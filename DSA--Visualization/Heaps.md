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
### Dry Run
This pseudocode finds the **`k` points closest to the origin `(0,0)`**.

The important thing is that we use:

```text
distance² = x² + y²
```

We don't need the actual square root because comparing squared distances gives the same order.

Given:

```text
points = [[1,3],[-2,2]]
k = 1
```

---

### Pseudocode

```text
compute dist² = x² + y² for each
sort points by dist²
return the first k
```

### 1. Calculate distance² for each point

#### Point 1: `[1,3]`

Here:

```text
x = 1
y = 3
```

Calculate:

```text
dist² = x² + y²
      = 1² + 3²
      = 1 + 9
      = 10
```

So:

```text
[1,3] → distance² = 10
```

---

#### Point 2: `[-2,2]`

Here:

```text
x = -2
y = 2
```

Calculate:

```text
dist² = (-2)² + 2²
      = 4 + 4
      = 8
```

So:

```text
[-2,2] → distance² = 8
```

---

### 2. `sort points by dist²`

We have:

```text
[1,3]  → 10
[-2,2] → 8
```

Ascending order:

```text
[-2,2] → 8
[1,3]  → 10
```

So sorted points:

```text
[[-2,2], [1,3]]
```

---

### 3. `return the first k`

We have:

```text
k = 1
```

Therefore, return the first **1 point**:

```text
[-2,2]
```

### ✅ Final Output

```text
[[-2,2]]
```

### 🧠 Quick comparison

```text
Point       dist²
[1,3]         10
[-2,2]         8   ← closer
```

So `[-2,2]` is the closest point to `(0,0)`.

**Time Complexity:** `O(n log n)` because of sorting.
**Space Complexity:** `O(n)` if distances are stored separately.


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
### Dry Run
This pseudocode finds the **`k` closest points to the origin `(0,0)`** using a **max-heap**.

Given:

```text id="x8g6q2"
points = [[3,3],[5,-1],[-2,4]]
k = 2
```

We need the **2 closest points**.

---

### Pseudocode

```text id="0qf5vz"
heap = empty max-heap on dist²

for p in points:
    if heap.size < k: push p
    else if dist(p) < heap.top:
        pop the farthest
        and push p
    else: skip

return heap contents
```

The important idea is:

> A **max-heap** keeps the **farthest point among our current `k` points at the top**.

So if a new point is closer, we can remove the farthest one.

---

### 1. `heap = empty max-heap on dist²`

Initially:

```text id="a2p7m9"
heap = []
```

We compare points using:

```text id="m7n4x1"
dist² = x² + y²
```

No square root is needed.

---

### 2. `p = [3,3]`

Calculate its squared distance:

```text id="v9q3w5"
dist² = 3² + 3²
      = 9 + 9
      = 18
```

Heap size:

```text id="c8k1z4"
0 < k
0 < 2
```

So push `[3,3]`.

```text id="2h7xq6"
heap = [[3,3]]
```

Its distance is `18`.

---

### 3. `p = [5,-1]`

Calculate:

```text id="j4p8s2"
dist² = 5² + (-1)²
      = 25 + 1
      = 26
```

Current heap size:

```text id="r5m2v8"
1 < 2
```

So push `[5,-1]`.

```text id="n6c3y7"
heap = [[3,3], [5,-1]]
```

Distances:

```text id="x2k9p1"
[3,3]  → 18
[5,-1] → 26
```

Because this is a **max-heap**, the largest distance is at the top:

```text id="e7w4q0"
heap.top = 26
```

So `[5,-1]` is currently the **farthest** of our 2 selected points.

---

### 4. `p = [-2,4]`

Calculate its squared distance:

```text id="q1m6z8"
dist² = (-2)² + 4²
      = 4 + 16
      = 20
```

Now the heap is already full:

```text id="b5r9t2"
heap.size = 2
k = 2
```

So check:

```text id="k8v3n6"
dist(p) < heap.top
20 < 26
```

This is **true** ✅

That means `[-2,4]` is closer than the current farthest point `[5,-1]`.

---

### 5. `pop the farthest`

Remove the heap top:

```text id="q6d2x9"
[5,-1] → distance² = 26
```

Heap now contains:

```text id="f3m7a1"
[[3,3]]
```

---

### 6. `and push p`

Push `[-2,4]`:

```text id="r8c5k3"
heap = [[3,3],[-2,4]]
```

Distances:

```text id="z4n2v7"
[3,3]   → 18
[-2,4]  → 20
```

Because it's a max-heap:

```text id="d9p1x6"
heap.top = [-2,4]  // distance² = 20
```

---

### Complete Dry Run

| Point    | dist² | Action             | Heap after action |
| -------- | ----: | ------------------ | ----------------- |
| `[3,3]`  |    18 | Push               | `[[3,3]]`         |
| `[5,-1]` |    26 | Push               | `[[3,3],[5,-1]]`  |
| `[-2,4]` |    20 | Remove 26, push 20 | `[[3,3],[-2,4]]`  |

The final two closest points are:

```text id="b7m4q9"
[3,3]    → 18
[-2,4]   → 20
```

The point `[5,-1]` has:

```text id="v2k8s5"
26
```

so it is removed.

### ✅ Final Output

```text id="g6x1p4"
[[3,3],[-2,4]]
```

The **order of points in the heap is not guaranteed**, so:

```text
[[-2,4],[3,3]]
```

would also be a valid output.

### 🧠 Main idea

For **k closest points**:

```text id="h4z9c2"
Use a MAX-HEAP
        ↓
Keep k points
        ↓
Top = farthest among those k
        ↓
New point closer?
        ↓
Remove farthest + add new point
```

**Time Complexity:** `O(n log k)`
**Space Complexity:** `O(k)`


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
### Dry Run
Sure. This pseudocode is for finding the **k elements closest to `x`**, with a tie-breaker: if two values have the same distance from `x`, the **smaller value gets higher rank**.

### Pseudocode

```text
given arr, x, k
rank each value by (|a − x|, a)
take the best k
return them sorted
```

### 1. Given input

```text
arr = [1,1,2,3,4,5]
x = -1
k = 4
```

We calculate:

```text
|a - x| = |a - (-1)| = |a + 1|
```

### 2. Calculate distance and rank

| Value `a` | Distance `|a - (-1)|` | Rank `(distance, a)` |
|---:|---:|---|
| 1 | `|1+1| = 2` | `(2,1)` |
| 1 | `2` | `(2,1)` |
| 2 | `|2+1| = 3` | `(3,2)` |
| 3 | `4` | `(4,3)` |
| 4 | `5` | `(5,4)` |
| 5 | `6` | `(6,5)` |

So the ranking from **best → worst** is:

```text
1 → (2,1)
1 → (2,1)
2 → (3,2)
3 → (4,3)
4 → (5,4)
5 → (6,5)
```

### 3. `take the best k`

`k = 4`, so take the first 4:

```text
[1, 1, 2, 3]
```

### 4. `return them sorted`

They are already sorted:

```text
[1, 1, 2, 3]
```

### ✅ Final Output

```text
[1, 1, 2, 3]
```

### 🧠 Main idea

Because `x = -1` is **to the left of every element**, the closest elements are simply the **smallest values** in the array.

```text
x = -1
     ↓
[-1] 1  1  2  3  4  5
      ↑  ↑  ↑  ↑
      2  2  3  4   ← distances
```

So the **4 closest elements are `[1,1,2,3]`**.


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
### Dry Run
Sure. This is the **max-heap approach for finding the `k` closest elements to `x`**.

The important point is:

> Since it is a **max-heap**, the `top` contains the **worst/farthest element among the current k elements**.

### Pseudocode

```text
given arr, x, k
heap = max-heap on (|a − x|, a)

for a in arr:
    if heap.size < k:
        push a
    else if (dist,a) better than top:
        pop the worst
        and push a
    else:
        skip

return heap sorted
```

### Input

```text
arr = [1,2,3,4,5]
x = 3
k = 4
```

---

### 1. Calculate distances

Distance formula:

```text
|a - x| = |a - 3|
```

| Value | Distance |     |      |
| ----: | -------: | --- | ---- |
|     1 |        ` | 1-3 | = 2` |
|     2 |        ` | 2-3 | = 1` |
|     3 |        ` | 3-3 | = 0` |
|     4 |        ` | 4-3 | = 1` |
|     5 |        ` | 5-3 | = 2` |

The ranking `(distance, value)` is:

```text
3 → (0,3)   ← best
2 → (1,2)
4 → (1,4)
1 → (2,1)
5 → (2,5)   ← worst
```

For equal distances, the **smaller value is better**.

---

### 2. Process each element

#### `a = 1`

Heap size is `0`, and `0 < k`.

So push `1`.

```text
heap = [1]
```

Distance:

```text
1 → (2,1)
```

---

#### `a = 2`

Heap size is `1`, and `1 < 4`.

Push `2`.

```text
heap = [1,2]
```

---

#### `a = 3`

Heap size is `2`, and `2 < 4`.

Push `3`.

```text
heap = [1,2,3]
```

---

#### `a = 4`

Heap size is `3`, and `3 < 4`.

Push `4`.

```text
heap = [1,2,3,4]
```

Now we have exactly `k = 4` elements.

Their distances are:

```text
1 → distance 2
2 → distance 1
3 → distance 0
4 → distance 1
```

The **worst** is `1`, because its distance is `2`.

---

#### `a = 5`

Now:

```text
heap.size = 4
k = 4
```

So we cannot directly push.

We compare `5` with the heap's worst element.

```text
5 → distance |5-3| = 2
top/worst = 1 → distance |1-3| = 2
```

Both have the same distance.

Tie-breaker is the value:

```text
1 < 5
```

So `1` is better than `5`.

Therefore, `5` is **worse** than the current top.

```text
else: skip
```

So `5` is skipped.

Heap remains:

```text
[1,2,3,4]
```

---

### 3. Return heap sorted

Sort the selected elements:

```text
[1,2,3,4]
```

### ✅ Final Output

```text
[1,2,3,4]
```

### 🧠 Key point to remember

For a **max-heap**:

```text
heap top = worst among current k
```

So whenever a new element comes:

```text
new element better than top
        ↓
remove top (worst)
        ↓
insert new element
```

Here `5` was not better than `1`, because:

```text
distance(5,3) = 2
distance(1,3) = 2
```

and when distances tie, **smaller value wins**. Hence `1` stays and `5` is skipped.


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
### Dry Run
Sure. This pseudocode is the **brute-force approach to merge `k` sorted lists**. We look at the first/current element (head) of every list, choose the smallest one, put it into the output, and then move that list's pointer forward.

### Pseudocode

```text id="k7m3px"
given k sorted lists

repeat until all empty:
    scan the ≤ k current heads
    move the minimum to output
    advance that list
```

### Input

```text id="z9q4hd"
lists = [
    [1,4,5],
    [1,3,4],
    [2,6]
]
```

There are **3 sorted lists**, so at most 3 heads are checked at a time.

---

### Step 1

Current heads:

```text id="d4y1qk"
List 1 → 1
List 2 → 1
List 3 → 2
```

Minimum = `1`.

There are two `1`s. We can take either one.

Take List 1's `1`:

```text id="r8k2vz"
output = [1]
```

Advance List 1:

```text id="p3n7wa"
[1,4,5]
 ↓
[4,5]
```

---

### Step 2

Current heads:

```text id="w2f8mc"
List 1 → 4
List 2 → 1
List 3 → 2
```

Minimum = `1`.

```text id="g5x1ra"
output = [1,1]
```

Advance List 2:

```text id="u6m9pt"
[1,3,4]
   ↓
[3,4]
```

---

### Step 3

Current heads:

```text id="v3k7ds"
List 1 → 4
List 2 → 3
List 3 → 2
```

Minimum = `2`.

```text id="j8q4lm"
output = [1,1,2]
```

Advance List 3:

```text id="a2w6nc"
[2,6]
 ↓
[6]
```

---

### Step 4

Current heads:

```text id="h7r2kp"
List 1 → 4
List 2 → 3
List 3 → 6
```

Minimum = `3`.

```text id="c5n8bx"
output = [1,1,2,3]
```

Advance List 2:

```text id="m4q9zs"
[3,4]
   ↓
[4]
```

---

### Step 5

Current heads:

```text id="e1v6ty"
List 1 → 4
List 2 → 4
List 3 → 6
```

Minimum = `4`.

Take either `4`.

```text id="x9p3ka"
output = [1,1,2,3,4]
```

Suppose we take List 1's `4`.

List 1 becomes:

```text id="n6b2rw"
[4,5]
   ↓
[5]
```

---

### Step 6

Current heads:

```text id="q2c7mf"
List 1 → 5
List 2 → 4
List 3 → 6
```

Minimum = `4`.

```text id="y8d1hs"
output = [1,1,2,3,4,4]
```

Advance List 2:

```text id="s5k9je"
[4]
 ↓
[]
```

List 2 is now empty.

---

### Step 7

Current heads:

```text id="b3m7qx"
List 1 → 5
List 3 → 6
```

Minimum = `5`.

```text id="t4w8nc"
output = [1,1,2,3,4,4,5]
```

Advance List 1:

```text id="r1p6vz"
[5]
 ↓
[]
```

---

### Step 8

Only List 3 remains:

```text id="f7k2md"
List 3 → 6
```

Take `6`:

```text id="u9c4xa"
output = [1,1,2,3,4,4,5,6]
```

List 3 becomes empty.

Now **all lists are empty**, so the loop stops.

### ✅ Final Output

```text id="n2h6qs"
[1,1,2,3,4,4,5,6]
```

### 🧠 Main idea

At every step:

```text
Current heads
     ↓
[4, 3, 2]
     ↓
minimum = 2
     ↓
put 2 in output
     ↓
advance the list containing 2
```

So we repeatedly pick the **smallest current head** until all lists are empty.

**Complexity:** If there are `k` lists and `N` total elements, each selection scans up to `k` heads, so **O(N × k)** time.


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
### Dry Run
Sure. This is the **min-heap optimized approach to merge `k` sorted lists**.

The main idea is:

> Keep only the **current smallest candidate from each list** inside the min-heap. The heap root always gives us the smallest value.

### Pseudocode

```text id="m4k7qp"
heap = min-heap
push every list head

while heap not empty:
    (v, list) = pop the root
    append v to output

    if list has a next head:
        push it

    // else list is exhausted

return output
```

### Input

```text id="z8n2wc"
lists = [
    [1,4,7],
    [2,5],
    [3,6,8]
]
```

We have **3 sorted lists**, so initially we push their first elements.

---

### Step 1: Push every list head

List 1:

```text
[1,4,7]
 ↑
```

Head = `1`

List 2:

```text
[2,5]
 ↑
```

Head = `2`

List 3:

```text
[3,6,8]
 ↑
```

Head = `3`

So heap contains:

```text id="v3q8ma"
heap = [1,2,3]
```

The root is:

```text
1
```

---

### Step 2

```text
(v, list) = pop the root
```

Root = `1`.

So:

```text
v = 1
list = [1,4,7]
```

Remove `1`:

```text
heap = [2,3]
```

Append `1`:

```text
output = [1]
```

Now check:

```text
if list has a next head
```

List 1 has next value `4`.

Push `4`:

```text
heap = [2,3,4]
```

---

### Step 3

Heap:

```text
[2,3,4]
 ↑
```

Root = `2`.

Pop `2`:

```text
output = [1,2]
```

List 2:

```text
[2,5]
   ↑
```

Next head = `5`.

Push `5`:

```text
heap = [3,4,5]
```

---

### Step 4

Heap:

```text
[3,4,5]
 ↑
```

Root = `3`.

Pop `3`:

```text
output = [1,2,3]
```

List 3 has next value `6`.

Push `6`:

```text
heap = [4,5,6]
```

---

### Step 5

Root = `4`.

Pop `4`:

```text
output = [1,2,3,4]
```

List 1 has next value `7`.

Push `7`:

```text
heap = [5,6,7]
```

---

### Step 6

Root = `5`.

Pop `5`:

```text
output = [1,2,3,4,5]
```

List 2 has no next element.

```text
[2,5]
    ↓
  exhausted
```

So we **don't push anything**.

Heap:

```text
[6,7]
```

---

### Step 7

Root = `6`.

Pop `6`:

```text
output = [1,2,3,4,5,6]
```

List 3 has next value `8`.

Push `8`:

```text
heap = [7,8]
```

---

### Step 8

Root = `7`.

Pop `7`:

```text
output = [1,2,3,4,5,6,7]
```

List 1 is exhausted:

```text
[1,4,7]
      ↓
   exhausted
```

Nothing is pushed.

Heap:

```text
[8]
```

---

### Step 9

Root = `8`.

Pop `8`:

```text
output = [1,2,3,4,5,6,7,8]
```

List 3 is exhausted.

Heap becomes:

```text
[]
```

Now:

```text
while heap not empty
```

is false, so we stop.

### ✅ Final Output

```text id="r7p3kx"
[1,2,3,4,5,6,7,8]
```

### Complete dry run

| Step | Popped | Output              | New element pushed |
| ---: | -----: | ------------------- | -----------------: |
|    1 |      1 | `[1]`               |                  4 |
|    2 |      2 | `[1,2]`             |                  5 |
|    3 |      3 | `[1,2,3]`           |                  6 |
|    4 |      4 | `[1,2,3,4]`         |                  7 |
|    5 |      5 | `[1,2,3,4,5]`       |                  — |
|    6 |      6 | `[1,2,3,4,5,6]`     |                  8 |
|    7 |      7 | `[1,2,3,4,5,6,7]`   |                  — |
|    8 |      8 | `[1,2,3,4,5,6,7,8]` |                  — |

### 🧠 Key idea

Unlike the previous approach where we **scan all list heads**, here the heap automatically gives us the smallest head:

```text
       MIN-HEAP
          ↓
          1   ← smallest
        /   \
       2     3
```

After removing `1`, we insert **only its next element `4`**.

So the heap always contains at most **one current element from each list**.

**Complexity:** `O(N log k)` time and `O(k)` heap space, where `N` is the total number of elements and `k` is the number of lists.


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
