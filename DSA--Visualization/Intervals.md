# Merge Intervals

**LeetCode #56** · [LeetCode](https://leetcode.com/problems/merge-intervals/) · **Medium**

> **Intervals · Sort by Start · Merge Overlapping Intervals**

### Approaches

#### 1. Optimized (Sort + One-Pass Sweep)
Sort intervals by start time, then compare each interval with the current interval. If they overlap, extend `cur.end = max(cur.end, b.end)`; otherwise, add `cur` to the result and start a new interval.

**Pseudo Code:**

```text id="q8x3vn"
sort intervals by start

out ← []

cur ← intervals[0]

for b in intervals[1:]:

    if b.start <= cur.end:
        cur.end = max(cur.end, b.end)

    else:
        out.append(cur)
        cur ← b

out.append(cur)

return out
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Sort + One-Pass Sweep** | **O(n log n)** | **O(n)** |

### Dry Run

### Input

```text
[[1,3], [2,6], [8,10], [15,18], [8,9], [9,11], [2,4], [16,17]]
```

## Step 1: Sort by starting value

Sort using the **first number** of each interval:

```text
[[1,3], [2,6], [2,4], [8,10], [8,9], [9,11], [15,18], [16,17]]
```

Why?

```text
1, 2, 2, 8, 8, 9, 15, 16
```

are in increasing order.

---

## Step 2: `out ← []`

Create an empty array for our final answer.

```text
out = []
```

---

## Step 3: `cur ← intervals[0]`

Take the first interval:

```text
cur = [1,3]
```

`cur` means:

> The interval we're currently trying to merge.

---

## Step 4: Loop

```text
for b in intervals[1:]:
```

Means:

> Start checking every interval after `[1,3]`.

---

### 🔹 Iteration 1

```text
b = [2,6]
cur = [1,3]
```

Check:

```text
b.start <= cur.end
2 <= 3
```

✅ True → intervals overlap.

So:

```text
cur.end = max(cur.end, b.end)
```

```text
cur.end = max(3,6)
         = 6
```

Now:

```text
cur = [1,6]
out = []
```

---

### 🔹 Iteration 2

```text
b = [2,4]
cur = [1,6]
```

Check:

```text
2 <= 6
```

✅ Overlap.

```text
cur.end = max(6,4)
         = 6
```

Still:

```text
cur = [1,6]
```

---

### 🔹 Iteration 3

```text
b = [8,10]
cur = [1,6]
```

Check:

```text
8 <= 6
```

❌ No overlap.

So execute:

```text
out.append(cur)
```

Now:

```text
out = [[1,6]]
```

Then:

```text
cur = b
```

Therefore:

```text
cur = [8,10]
```

---

### 🔹 Iteration 4

```text
b = [8,9]
cur = [8,10]
```

Check:

```text
8 <= 10
```

✅ Overlap.

```text
cur.end = max(10,9)
         = 10
```

So:

```text
cur = [8,10]
```

---

### 🔹 Iteration 5

```text
b = [9,11]
cur = [8,10]
```

Check:

```text
9 <= 10
```

✅ Overlap.

```text
cur.end = max(10,11)
         = 11
```

Now:

```text
cur = [8,11]
```

---

### 🔹 Iteration 6

```text
b = [15,18]
cur = [8,11]
```

Check:

```text
15 <= 11
```

❌ No overlap.

So:

```text
out.append(cur)
```

```text
out = [[1,6], [8,11]]
```

Then:

```text
cur = b
```

```text
cur = [15,18]
```

---

### 🔹 Iteration 7

```text
b = [16,17]
cur = [15,18]
```

Check:

```text
16 <= 18
```

✅ Overlap.

Update:

```text
cur.end = max(18,17)
         = 18
```

So:

```text
cur = [15,18]
```

---

## Step 5: Loop finished

We've checked everything.

But notice:

```text
cur = [15,18]
```

has **not yet been added to `out`**.

That's why we need:

```text
out.append(cur)
```

Now:

```text
out = [[1,6], [8,11], [15,18]]
```

Finally:

```text
return out
```

### ✅ Final Answer

```text
[[1,6], [8,11], [15,18]]
```

### Full dry run in one table

| `b`       | `cur` before | Check        | Action     | `cur` after | `out`                    |
| --------- | ------------ | ------------ | ---------- | ----------- | ------------------------ |
| `[2,6]`   | `[1,3]`      | `2 <= 3` ✅   | Merge      | `[1,6]`     | `[]`                     |
| `[2,4]`   | `[1,6]`      | `2 <= 6` ✅   | Merge      | `[1,6]`     | `[]`                     |
| `[8,10]`  | `[1,6]`      | `8 <= 6` ❌   | Save + new | `[8,10]`    | `[[1,6]]`                |
| `[8,9]`   | `[8,10]`     | `8 <= 10` ✅  | Merge      | `[8,10]`    | `[[1,6]]`                |
| `[9,11]`  | `[8,10]`     | `9 <= 10` ✅  | Merge      | `[8,11]`    | `[[1,6]]`                |
| `[15,18]` | `[8,11]`     | `15 <= 11` ❌ | Save + new | `[15,18]`   | `[[1,6],[8,11]]`         |
| `[16,17]` | `[15,18]`    | `16 <= 18` ✅ | Merge      | `[15,18]`   | `[[1,6],[8,11]]`         |
| **end**   | `[15,18]`    | —            | Save       | —           | `[[1,6],[8,11],[15,18]]` |

**The main trick:** after sorting, you only need to compare `b.start` with `cur.end`. If `b.start <= cur.end`, they overlap; otherwise, the current interval is finished.


---

# Insert Interval

**LeetCode #57** · [LeetCode](https://leetcode.com/problems/insert-interval/) · **Medium**

> **Intervals · Sorted & Non-overlapping · Three-Phase Sweep**

### Approaches

#### 1. Optimized (Three-Phase Sweep)
1. Add intervals completely **before** `newInterval`.
2. Merge all intervals **overlapping** `newInterval` by updating its start and end.
3. Add all remaining intervals **after** the merged interval.

No sorting is needed because the input is already sorted and non-overlapping.

**Pseudo Code:**

```text id="7h3k2p"
out ← []
i ← 0

// phase 1: intervals entirely before new

while i < n and intervals[i].end < new.start:
    out.append(intervals[i])
    i++

// phase 2: merge every overlapping interval into new

while i < n and intervals[i].start <= new.end:
    new = [min(starts), max(ends)]
    i++

out.append(new)

// phase 3: copy the rest

while i < n:
    out.append(intervals[i])
    i++

return out
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Three-Phase Sweep** | **O(n)** | **O(n)** |

---

# Non-overlapping Intervals

**LeetCode #435** · [LeetCode](https://leetcode.com/problems/non-overlapping-intervals/) · **Medium**

> **Intervals · Sort by End · Greedy · Keep Early Finishers**

### Approaches

#### 1. Optimized (Greedy by End Time)
Sort intervals by their **end time**. Keep the interval that finishes earliest; if the next interval overlaps, remove it. This leaves maximum room for future intervals.

**Pseudo Code:**

```text id="m8v2kx"
sort intervals by end

prevEnd ← −∞
removed ← 0

for iv in intervals:

    if iv.start >= prevEnd:
        keep
        prevEnd ← iv.end

    else:
        removed += 1    # iv ends later — drop it

return removed
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Greedy by End Time** | **O(n log n)** | **O(1)** |

---
