# Table of Contents

- [Count Vowels in Substrings](#count-vowels-in-substrings)
- [Subarray Sum Equals K](#subarray-sum-equals-k)

---

# Count Vowels in Substrings

**Prefix Sum · Range Queries · Prefix vowel counts**

### Tip

**Brute Force · Scan each query:** For every query `(l, r)`, scan all characters from `l` to `r` and count the vowels.

**Pseudo Code:**

```text
given s, queries

for each query (l, r):

    count = 0

    for i ← l to r:

        if s[i] is a vowel:

            count++

    answer the query with count
```

**Optimized · Prefix Sum:** Build a prefix array where each position stores the number of vowels seen so far. Then answer every query in **O(1)** using the difference between two prefix values.

**Pseudo Code:**

```text
given s, queries

V = array of size n + 1

V[0] = 0

for i ← 0 to n − 1:

    V[i + 1] = V[i] + (s[i] is a vowel ? 1 : 0)

for each query (l, r):

    answer = V[r + 1] − V[l]

return all answers
```

**Brute Force · Scan each query**

Time: **O(n · q)** · Space: **O(1)**

**Optimized · Prefix Sum**

Time: **O(n + q)** · Space: **O(n)**

---------

# Subarray Sum Equals K

**LeetCode #560** · [LeetCode](https://leetcode.com/problems/subarray-sum-equals-k/) · **Medium**

**Prefix Sum · HashMap · Running prefix + frequency count**

### Tip

**Brute Force · Every subarray:** Start from each index, keep a running sum, and count every subarray whose sum equals `k`.

**Pseudo Code:**

```text id="b6r2m8"
given arr, k

count = 0

for start in 0..n−1:

    sum = 0

    for end in start..n−1:

        sum += arr[end]

        if sum == k: count++

return count
```
### Dry Run
This pseudocode counts **how many contiguous subarrays have sum exactly equal to `k`**.

### Pseudocode

```text
given arr, k
count = 0

for start in 0..n−1:
    sum = 0

    for end in start..n−1:
        sum += arr[end]

        if sum == k:
            count++

return count
```

Given:

```text
arr = [3,4,7,2,-3,1,4,2]
k = 7
```

### Line-by-line explanation

#### 1. `given arr, k`

We have the array and target sum:

```text
arr = [3,4,7,2,-3,1,4,2]
k = 7
```

Array indices:

```text
Index:  0  1  2  3   4  5  6  7
Value:  3  4  7  2  -3  1  4  2
```

---

#### 2. `count = 0`

Initially, we haven't found any subarray whose sum is `7`.

```text
count = 0
```

---

#### 3. `for start in 0..n−1`

This chooses the **starting index** of the subarray.

We will try:

```text
start = 0
start = 1
start = 2
...
start = 7
```

---

#### 4. `sum = 0`

For every new `start`, reset the sum.

For example, when:

```text
start = 0
```

we begin with:

```text
sum = 0
```

---

#### 5. `for end in start..n−1`

This moves the ending index from `start` toward the end of the array.

For `start = 0`:

```text
end = 0
end = 1
end = 2
...
end = 7
```

So we examine all subarrays beginning at index `0`.

---

#### 6. `sum += arr[end]`

Add the current element to the running sum.

For `start = 0`:

| end | arr[end] | sum |
| --: | -------: | --: |
|   0 |        3 |   3 |
|   1 |        4 |   7 |
|   2 |        7 |  14 |
|   3 |        2 |  16 |
|   4 |       -3 |  13 |
|   5 |        1 |  14 |
|   6 |        4 |  18 |
|   7 |        2 |  20 |

At `end = 1`:

```text
sum = 3 + 4
    = 7
```

---

#### 7. `if sum == k: count++`

If the current sum equals `7`, increase `count`.

For example:

```text
sum = 7
k = 7
```

Therefore:

```text
count++
```

---

### Complete Dry Run

Let's find **all** subarrays whose sum is `7`.

#### `start = 0`

Starting from index 0:

```text
[3]                  = 3
[3,4]                = 7  ✅
[3,4,7]              = 14
[3,4,7,2]            = 16
[3,4,7,2,-3]         = 13
[3,4,7,2,-3,1]       = 14
[3,4,7,2,-3,1,4]     = 18
[3,4,7,2,-3,1,4,2]   = 20
```

Found:

```text
[3,4]
```

`count = 1`

---

#### `start = 1`

Starting from index 1:

```text
[4]                 = 4
[4,7]               = 11
[4,7,2]             = 13
[4,7,2,-3]          = 10
[4,7,2,-3,1]        = 11
[4,7,2,-3,1,4]      = 15
[4,7,2,-3,1,4,2]    = 17
```

No sum `7`.

```text
count = 1
```

---

#### `start = 2`

```text
[7]                 = 7  ✅
[7,2]               = 9
[7,2,-3]            = 6
[7,2,-3,1]          = 7  ✅
[7,2,-3,1,4]        = 11
[7,2,-3,1,4,2]      = 13
```

Found:

```text
[7]
[7,2,-3,1]
```

So:

```text
count = 3
```

---

#### `start = 3`

```text
[2]              = 2
[2,-3]           = -1
[2,-3,1]         = 0
[2,-3,1,4]       = 4
[2,-3,1,4,2]     = 6
```

No `7`.

---

#### `start = 4`

```text
[-3]          = -3
[-3,1]        = -2
[-3,1,4]      = 2
[-3,1,4,2]    = 4
```

No `7`.

---

#### `start = 5`

```text
[1]          = 1
[1,4]        = 5
[1,4,2]      = 7  ✅
```

Found:

```text
[1,4,2]
```

```text
count = 4
```

---

#### `start = 6`

```text
[4]       = 4
[4,2]     = 6
```

No `7`.

---

#### `start = 7`

```text
[2] = 2
```

No `7`.

---

### All matching subarrays

There are **4**:

```text
[3,4]              → 7
[7]                → 7
[7,2,-3,1]         → 7
[1,4,2]            → 7
```

Therefore:

```text
return count
```

### ✅ Final Output

```text
4
```

### 🧠 Main idea

`start` chooses **where the subarray starts**, and `end` keeps extending that subarray while `sum` maintains its running total.

Because `end` always moves forward from `start`, only **contiguous subarrays** are counted.

**Time Complexity:** `O(n²)`
**Space Complexity:** `O(1)`


**Optimized · Prefix Sum + HashMap:** For each running sum `prefix`, check how many times `prefix - k` has appeared. Store prefix-sum frequencies in a hashmap to count valid subarrays in one pass.

**Pseudo Code:**

```text id="n8q4v1"
given arr, k

seen = {0: 1}; cur = 0; count = 0

for x in arr:

    cur += x

    count += seen.get(cur − k, 0)

    seen[cur] += 1

return count
```
### Dry Run
This is the **optimized O(n) approach** for counting the number of contiguous subarrays whose sum is exactly `k`.

Given:

```text
arr = [3,4,7,2,-3,1,4,2]
k = 7
```

### Pseudocode

```text
seen = {0: 1}
cur = 0
count = 0

for x in arr:
    cur += x
    count += seen.get(cur − k, 0)
    seen[cur] += 1

return count
```

### 1. `seen = {0: 1}`

`seen` stores:

> **prefix sum → how many times we have seen that prefix sum**

We start with:

```text
seen = {0: 1}
```

Why `0:1`?

It represents a **prefix sum of 0 before we process any element**.

This is important when the subarray starts from index `0`.

---

### 2. `cur = 0`

`cur` means the **current prefix sum**.

```text
cur = 0
```

---

### 3. `count = 0`

This stores the number of subarrays whose sum is `k`.

```text
count = 0
```

---

### 4. `for x in arr`

We process every element one by one:

```text
3 → 4 → 7 → 2 → -3 → 1 → 4 → 2
```

---

### Iterations

The important formula is:

```text
needed = cur - k
```

If we have previously seen this `needed` prefix sum, then the elements between that previous position and the current position have sum `k`.

---

#### x = 3

```text
cur += 3
cur = 3
```

Need:

```text
cur - k
= 3 - 7
= -4
```

`-4` is not in `seen`.

```text
count = 0
```

Now store current prefix sum:

```text
seen[3] += 1
```

So:

```text
seen = {0:1, 3:1}
```

---

#### x = 4

```text
cur = 3 + 4
    = 7
```

Need:

```text
7 - 7 = 0
```

`0` is in `seen` with frequency `1`.

Therefore:

```text
count += 1
count = 1
```

This corresponds to:

```text
[3,4] = 7
```

Now:

```text
seen[7] += 1
```

```text
seen = {0:1, 3:1, 7:1}
```

---

#### x = 7

```text
cur = 7 + 7
    = 14
```

Need:

```text
14 - 7 = 7
```

`7` exists once.

So:

```text
count += 1
count = 2
```

The subarray is:

```text
[7]
```

Update:

```text
seen = {0:1, 3:1, 7:1, 14:1}
```

---

#### x = 2

```text
cur = 14 + 2
    = 16
```

Need:

```text
16 - 7 = 9
```

`9` is not present.

```text
count = 2
```

Update:

```text
seen[16] = 1
```

---

#### x = -3

```text
cur = 16 - 3
    = 13
```

Need:

```text
13 - 7 = 6
```

`6` is not present.

```text
count = 2
```

Update:

```text
seen[13] = 1
```

---

#### x = 1

```text
cur = 13 + 1
    = 14
```

Need:

```text
14 - 7 = 7
```

`7` exists **once** in `seen`.

So:

```text
count += 1
count = 3
```

This gives:

```text
[7,2,-3,1] = 7
```

Now `14` has appeared twice:

```text
seen[14] = 2
```

---

#### x = 4

```text
cur = 14 + 4
    = 18
```

Need:

```text
18 - 7 = 11
```

`11` doesn't exist.

```text
count = 3
```

Update:

```text
seen[18] = 1
```

---

#### x = 2

```text
cur = 18 + 2
    = 20
```

Need:

```text
20 - 7 = 13
```

`13` exists once.

Therefore:

```text
count += 1
count = 4
```

This gives:

```text
[1,4,2] = 7
```

Update:

```text
seen[20] = 1
```

---

### Complete Dry Run Table

|  x | `cur` | `cur-k` | Frequency in `seen` | `count` |
| -: | ----: | ------: | ------------------: | ------: |
|  3 |     3 |      -4 |                   0 |       0 |
|  4 |     7 |       0 |                   1 |       1 |
|  7 |    14 |       7 |                   1 |       2 |
|  2 |    16 |       9 |                   0 |       2 |
| -3 |    13 |       6 |                   0 |       2 |
|  1 |    14 |       7 |                   1 |       3 |
|  4 |    18 |      11 |                   0 |       3 |
|  2 |    20 |      13 |                   1 |       4 |

The matching subarrays are:

```text
[3,4]          = 7
[7]            = 7
[7,2,-3,1]     = 7
[1,4,2]        = 7
```

### ✅ Final Output

```text
4
```

### 🧠 Why this is faster

The previous brute-force solution checks every possible subarray → **O(n²)**.

This solution uses `seen` to remember previous prefix sums, so each element is processed once:

**Time:** `O(n)`
**Space:** `O(n)`

The key idea is:

```text
If current prefix sum = cur
and an earlier prefix sum = cur - k

then:
current subarray sum = cur - (cur-k) = k
```


**Brute Force**

Time: **O(n²)** · Space: **O(1)**

**Optimized**

Time: **O(n)** · Space: **O(n)**

---
