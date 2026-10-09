# Table of Contents

- [Binary Search](#binary-search)
- [Search Insert Position](#search-insert-position)
- [Peak Index in a Mountain Array](#peak-index-in-a-mountain-array)
- [Maximum Candies Allocated to K Children](#maximum-candies-allocated-to-k-children)
- [Koko Eating Bananas](#koko-eating-bananas)
- [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
- [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
- [Search a 2D Matrix](#search-a-2d-matrix)
- [Split Array Largest Sum](#split-array-largest-sum)
- [Kth Smallest in a Sorted Matrix](#kth-smallest-in-a-sorted-matrix)
- [Capacity to Ship Packages Within D Days](#capacity-to-ship-packages-within-d-days)

---

# Binary Search

**LeetCode #704** · [LeetCode](https://leetcode.com/problems/binary-search/) · **Easy**

> **Binary Search · Sorted Array · Halve `[lo, hi]`**

### Approaches

#### 1. Iterative (Binary Search)
Keep `lo` and `hi`; check the middle element and discard the half that cannot contain the target.

**Pseudo Code:**

```text
lo ← 0;  hi ← n − 1

while lo <= hi:

    mid ← lo + (hi − lo) / 2     // avoid overflow

    if arr[mid] == target: return mid

    else if arr[mid] < target: lo ← mid + 1

    else:                      hi ← mid − 1

return −1
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Iterative · Binary Search** | **O(log n)** | **O(1)** |

---

# Search Insert Position

**LeetCode #35** · [LeetCode](https://leetcode.com/problems/search-insert-position/) · **Easy**

> **Binary Search · Sorted Array · Find the first index where `arr[i] ≥ target`**

### Approaches

#### 1. Brute force (linear scan)
Scan from left to right; return the first index where `arr[i] ≥ target`. If none exists, return `n`.

**Pseudo Code:**

```text
for i ← 0 to n − 1:

    if arr[i] ≥ target: return i

return n
```
### Dry Run
This pseudocode is used to find the insert position of a target in a sorted array. If the target already exists, it returns its current index.

### 1. Pseudocode

```
for i ← 0 to n − 1:
    if arr[i] ≥ target: return i
return n
```

### 2. Given input

```
nums = [1, 3, 5, 6]
target = 5
```

Here, `n = 4` because the array contains 4 elements.

### 3. Explain line by line

Line 1: `for i ← 0 to n − 1`

The loop checks each element from index `0` to index `3`.

Line 2: `if arr[i] ≥ target: return i`

At each index, check whether the current element is greater than or equal to the target (`5`).

| Iteration | `i` | `arr[i]` | Is `arr[i] ≥ 5`? | Action     |
| --------- | --- | -------- | ---------------- | ---------- |
| 1         | 0   | 1        | `1 ≥ 5` → False  | Continue   |
| 2         | 1   | 3        | `3 ≥ 5` → False  | Continue   |
| 3         | 2   | 5        | `5 ≥ 5` → True   | Return `2` |

As soon as the condition becomes true, the function returns `i = 2`. The loop stops immediately.

Line 3: `return n`

This line runs only if the loop finishes without finding an element greater than or equal to the target. Here, it does not execute.

### ✅ Final Output

```
2
```

The target `5` is already present at index `2` (using zero-based indexing).

### 🧠 Remember
If the target is greater than every element, the algorithm returns `n`, which is `4` in this example.



#### 2. Optimized (binary search)
Use binary search to find the **leftmost position** where `arr[i] ≥ target`; when the target is missing, `lo` lands exactly at its insertion position.

**Pseudo Code:**

```text
lo ← 0;  hi ← n − 1

while lo ≤ hi:

    mid ← (lo + hi) / 2

    if arr[mid] == target: return mid

    if arr[mid] < target: lo ← mid + 1

    else: hi ← mid − 1

return lo
```
### Dry Run
This pseudocode uses Binary Search to find the target in a sorted array. If the target is not found, it returns the index where the target should be inserted.

### 1. Given input

```
arr = [1, 3, 5, 6]
target = 7
```

Array length: `n = 4`

```
lo ← 0
hi ← n − 1 = 3
```

* `lo` = starting index.

* `hi` = ending index.

* `mid` = middle index of the current search range.

### 2. Explain the pseudocode line by line

```
lo ← 0; hi ← n − 1

while lo ≤ hi:
    mid ← (lo + hi) / 2

    if arr[mid] == target:
        return mid

    if arr[mid] < target:
        lo ← mid + 1
    else:
        hi ← mid − 1

return lo
```

#### Iteration 1

Initially:

```
lo = 0
hi = 3
```

Calculate the middle index:

mid=⌊(0+3)/2⌋=1mid = \lfloor(0+3)/2\rfloor = 1mid=⌊(0+3)/2⌋=1

`arr[1] = 3`

Compare with target `7`:

```
3 == 7 → False
3 < 7  → True
```

Because `3` is smaller than `7`, search the right half.

```
lo = mid + 1
lo = 1 + 1 = 2
```

Updated values:

```
lo = 2
hi = 3
```

#### Iteration 2

Calculate the middle index again:

mid=⌊(2+3)/2⌋=2mid = \lfloor(2+3)/2\rfloor = 2mid=⌊(2+3)/2⌋=2

`arr[2] = 5`

Compare:

```
5 == 7 → False
5 < 7  → True
```

Move `lo` to the right:

```
lo = mid + 1
lo = 2 + 1 = 3
```

Updated values:

```
lo = 3
hi = 3
```

#### Iteration 3

Calculate the middle index:

mid=⌊(3+3)/2⌋=3mid = \lfloor(3+3)/2\rfloor = 3mid=⌊(3+3)/2⌋=3

`arr[3] = 6`

Compare:

```
6 == 7 → False
6 < 7  → True
```

Move `lo` again:

```
lo = mid + 1
lo = 3 + 1 = 4
```

Updated values:

```
lo = 4
hi = 3
```

### 3. When does the loop stop?

The loop condition is:

```
while lo ≤ hi
```

Now:

```
4 ≤ 3 → False
```

So the loop stops.

The final line executes:

```
return lo
```

Therefore, it returns `4`.

### ✅ Final Output

```
4
```

The array becomes:

```
[1, 3, 5, 6, 7]
```

The target `7` should be inserted at index 4, at the end of the array.

### Complete dry run

| Iteration | `lo` | `hi` | `mid` | `arr[mid]` | Action     |
| --------- | ---- | ---- | ----- | ---------- | ---------- |
| 1         | 0    | 3    | 1     | 3          | `lo = 2`   |
| 2         | 2    | 3    | 2     | 5          | `lo = 3`   |
| 3         | 3    | 3    | 3     | 6          | `lo = 4`   |
| Stop      | 4    | 3    | —     | —          | Return `4` |

### 🧠 Key point
When the target is greater than every element, `lo` moves one position beyond the last index. That final `lo` is the correct insertion index.

Time complexity: O(log⁡n)O(\log n)O(logn) because binary search halves the search range in each iteration.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force · linear scan** | **O(n)** | **O(1)** |
| **Optimized · binary search** | **O(log n)** | **O(1)** |

---

# Peak Index in a Mountain Array

**LeetCode #852** · [LeetCode](https://leetcode.com/problems/peak-index-in-a-mountain-array/) · **Medium**

> **Binary Search · Compare `arr[mid]` with `arr[mid + 1]` · Find the peak**

### Approaches

#### 1. Brute force (walk the slope)
Traverse left to right; the first index where `arr[i] > arr[i + 1]` is the peak.

**Pseudo Code:**

```text
for i ← 0 to n − 2:

    if arr[i] > arr[i + 1]: return i

return n − 1
```

#### 2. Optimized (binary search)
If `arr[mid] < arr[mid + 1]`, move right; otherwise, move left including `mid`. When `lo == hi`, that index is the peak.

**Pseudo Code:**

```text
lo ← 0;  hi ← n − 1

while lo < hi:

    mid ← (lo + hi) / 2

    if arr[mid] < arr[mid + 1]: lo ← mid + 1     # rising

    else: hi ← mid                               # falling

return lo
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n)** | **O(1)** |
| **Optimized** | **O(log n)** | **O(1)** |

---

# Maximum Candies Allocated to K Children

**LeetCode #2226** · [LeetCode](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/) · **Medium**

> **Binary Search · Search the Answer · Feasibility Check**

### Approaches

#### 1. Brute force (try every size)
Try every candidate size from `1` to `max(piles)` and check whether `Σ ⌊pile / size⌋ ≥ k`.

**Pseudocode:**

```text
best ← 0

for size ← 1 to max(piles):

    if Σ ⌊pile / size⌋ ≥ k: best ← size

return best
```

#### 2. Optimized (binary search the answer)
Binary search the candy size; if a size can serve at least `k` children, search larger, otherwise search smaller.

**Pseudocode:**

```text
lo ← 1;  hi ← max(piles);  best ← 0

while lo ≤ hi:

    mid ← (lo + hi) / 2

    if Σ ⌊pile / mid⌋ ≥ k: best ← mid;  lo ← mid + 1

    else: hi ← mid − 1

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n · max(piles))** | **O(1)** |
| **Optimized** | **O(n · log(max(piles)))** | **O(1)** |

---

# Koko Eating Bananas

**LeetCode #875** · [LeetCode](https://leetcode.com/problems/koko-eating-bananas/) · **Medium**

> **Binary Search · Search the Answer · Feasibility Check**

### Approaches

#### 1. Brute force (try every speed)
Try every eating speed from `1` to `max(piles)` and calculate the total hours using `Σ ⌈pile / speed⌉`. The first speed that finishes within `H` hours is the minimum.

**Pseudo Code:**

```text
given piles, H

for k = 1, 2, 3, …:

    hours = Σ ceil(pile / k)

    if hours ≤ H: return k

// first fit is the minimum
```

#### 2. Optimized (binary search the speed)
Binary search between `1` and `max(piles)`; if the current speed finishes within `H` hours, search smaller, otherwise search larger.

**Pseudo Code:**

```text
given piles, H

lo = 1, hi = max(piles)

while lo ≤ hi:

    mid = (lo + hi) / 2

    if Σ ceil(pile / mid) ≤ H:

        ans = mid; hi = mid − 1   // try slower

    else: lo = mid + 1            // need faster

return ans
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n · max(piles))** | **O(1)** |
| **Optimized** | **O(n · log(max(piles)))** | **O(1)** |

---

# Search in Rotated Sorted Array

**LeetCode #33** · [LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/) · **Medium**

> **Binary Search · One half is always sorted · Use the sorted half to decide direction**

### Approaches

#### 1. Brute force (linear scan)
Traverse the array and return the index when `arr[i] == target`.

**Pseudo Code:**

```text id="q8k3vp"
given arr, target

for i ← 0 to n − 1:

    if arr[i] == target: return i

return −1
```

#### 2. Optimized (steer by the sorted half)
Check which half is sorted; determine whether the target lies inside that sorted range, then discard the other half.

**Pseudo Code:**

```text id="m4z7tx"
lo = 0, hi = n − 1

while lo ≤ hi:

    mid = (lo + hi) / 2

    if arr[mid] == target: return mid

    if arr[lo] ≤ arr[mid]:        // left sorted

        target in [arr[lo], arr[mid]) ? hi = mid−1 : lo = mid+1

    else:                         // right sorted

        target in (arr[mid], arr[hi]] ? lo = mid+1 : hi = mid−1
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n)** | **O(1)** |
| **Optimized** | **O(log n)** | **O(1)** |

---

# Find Minimum in Rotated Sorted Array

**LeetCode #153** · [LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) · **Medium**

> **Binary Search · Compare `arr[mid]` with `arr[hi]` · Find the rotation point**

### Approaches

#### 1. Optimized (binary search on the rotation)
If `arr[mid] > arr[hi]`, the minimum is to the right of `mid`; otherwise, the minimum is at `mid` or to its left. When `lo == hi`, `arr[lo]` is the minimum.

**Pseudo Code:**

```text
lo = 0, hi = n − 1

while lo < hi:

    mid = lo + (hi − lo) / 2

    if arr[mid] > arr[hi]:    // cliff is to the right

        lo = mid + 1

    else:                     // min at mid or left

        hi = mid

return arr[lo]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(log n)** | **O(1)** |

---

# Search a 2D Matrix

**LeetCode #74** · [LeetCode](https://leetcode.com/problems/search-a-2d-matrix/) · **Medium**

> **Binary Search · Treat the matrix as one sorted array · Map index → row/column**

### Approaches

#### 1. Optimized (binary search the flattened grid)
Treat the `m × n` matrix as a sorted array of length `m·n`. For a 1D index `mid`, map it to `row = mid / n` and `col = mid % n`, then perform normal binary search.

**Pseudo Code:**

```text id="x7m2kp"
lo ← 0;  hi ← m·n − 1

while lo <= hi:

    mid ← lo + (hi − lo) / 2;  v ← grid[mid / n][mid % n]

    if v == target: return true

    else if v < target: lo ← mid + 1

    else:               hi ← mid − 1

return false
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(log(m·n))** | **O(1)** |

---

# Split Array Largest Sum

**LeetCode #410** · [LeetCode](https://leetcode.com/problems/split-array-largest-sum/) · **Hard**

> **Binary Search · Search the Answer · Greedy Feasibility Check**

### Approaches

#### 1. Brute force (try every cut)
Try all possible ways to split the array into `k` contiguous subarrays and choose the split with the minimum possible largest sum.

**Pseudo Code:**

```text id="n4q8vz"
given arr, m = 2

for each cut position:

    largest = max(sum left, sum right)

return the minimum largest
```

#### 2. Optimized (binary search the answer)
Binary search the maximum allowed subarray sum between `max(arr)` and `Σarr`; greedily create a new subarray whenever adding the next element would exceed the current limit.

**Pseudo Code:**

```text id="w6t2cx"
lo = max(arr), hi = sum(arr)

while lo ≤ hi:

    cap = (lo + hi) / 2

    greedy: count pieces if no piece > cap

    if pieces ≤ m:

        ans = cap; hi = cap − 1    // tighter

    else: lo = cap + 1             // looser

return ans
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n^(k−1))** | **O(1)** |
| **Optimized** | **O(n · log(Σarr))** | **O(1)** |

---

# Kth Smallest in a Sorted Matrix

**LeetCode #378** · [LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) · **Medium**

> **Binary Search · Search the Value Range · Count Elements `≤ mid`**

### Approaches

#### 1. Brute force (flatten and sort)
Flatten the matrix into a list, sort it, and return the `(k − 1)`th element.

**Pseudo Code:**

```text id="r8c4mx"
given matrix, k

flatten into a list; sort it

return list[k − 1]
```

#### 2. Optimized (binary search the value range)
Binary search between the smallest and largest matrix values; for each `mid`, count how many elements are `≤ mid`. If the count is at least `k`, search smaller; otherwise, search larger.

**Pseudo Code:**

```text id="v2n7qp"
lo = matrix min, hi = matrix max

while lo ≤ hi:

    x = (lo + hi) / 2

    cnt = # cells ≤ x   (per sorted row)

    if cnt ≥ k:

        ans = x; hi = x − 1

    else: lo = x + 1

return ans
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n² log(n²))** | **O(n²)** |
| **Optimized** | **O(n · log(max − min))** | **O(1)** |

---

# Capacity to Ship Packages Within D Days

**LeetCode #1011** · [LeetCode](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) · **Medium**

> **Binary Search · Search the Answer · Greedy Feasibility Check**

### Approaches

#### 1. Brute force (try every capacity)
Try capacities from `max(weights)` upward and greedily load consecutive packages; return the first capacity that can ship all packages within `D` days.

**Pseudo Code:**

```text id="p5v8kn"
given weights, D

for cap = max(w), max(w)+1, …:

    days = greedy-load at cap

    if days ≤ D: return cap

// first fit is minimal
```

#### 2. Optimized (binary search the capacity)
Binary search between `max(weights)` and `Σweights`; for each capacity, greedily count the required days. If `days ≤ D`, search smaller; otherwise, search larger.

**Pseudo Code:**

```text id="t3x6qm"
lo = max(w), hi = sum(w)

while lo ≤ hi:

    cap = (lo + hi) / 2

    days = greedy-load at cap

    if days ≤ D:

        ans = cap; hi = cap − 1

    else: lo = cap + 1

return ans
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n · Σweights)** | **O(1)** |
| **Optimized** | **O(n · log(Σweights))** | **O(1)** |
