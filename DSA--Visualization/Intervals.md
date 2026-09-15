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
