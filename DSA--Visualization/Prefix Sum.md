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

**Brute Force**

Time: **O(n²)** · Space: **O(1)**

**Optimized**

Time: **O(n)** · Space: **O(n)**

---
