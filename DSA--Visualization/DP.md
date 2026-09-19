# Solving a Question with Dynamic Programming

**Concept** · 5-Step DP Framework

> **Dynamic Programming · State → Recurrence → Base Case → Order → Answer**

### Approaches

#### 1. 5-Step Framework (House Robber)

1. **State:** `dp[i]` = maximum loot using houses `0..i`.

2. **Recurrence:** `dp[i] = max(dp[i−1], dp[i−2] + nums[i])`.

3. **Base Case:** `dp[0] = nums[0]`, `dp[1] = max(nums[0], nums[1])`.

4. **Order:** Compute from **left to right**.

5. **Answer:** The final answer is stored in `dp[n−1]`.

**Pseudo Code:**

```text id="m7k2xp"
def rob(nums):

    # 1. state:      dp[i] = best loot using houses 0..i

    # 2. recurrence: dp[i] = max(dp[i−1], dp[i−2] + nums[i])

    # 3. base cases:

    dp[0] = nums[0]

    dp[1] = max(nums[0], nums[1])

    # 4. order: left to right

    for i in 2..n−1:

        # skip house i, or rob it + best two back

        dp[i] = max(dp[i−1], dp[i−2] + nums[i])

    # 5. answer lives in the last cell

    return dp[n−1]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **5-Step Framework** | **O(n)** | **O(n)** |

---

# Pascal's Triangle

**LeetCode #118** · [LeetCode](https://leetcode.com/problems/pascals-triangle/) · **Easy**

> **Array / DP · Build each row from the two values above**

### Approaches

#### 1. Brute Force (Binomial Formula)
Calculate each value using the binomial coefficient `C(r,c) = r! / (c! · (r−c)!)`, but factorial calculations repeat work and can overflow.

**Pseudo Code:**

```text
for r ← 0 to rows − 1:

    for c ← 0 to r: entry ← r! / (c! · (r − c)!)
```

**# Factorials overflow quickly and repeat work**

#### 2. Optimized (Sum the Two Above)
Build the triangle row by row; the first and last values are `1`, while every middle value is the sum of the two values directly above it.

**Pseudo Code:**

```text
triangle ← []

for r ← 0 to rows − 1:

    row ← [1]

    for c ← 1 to r − 1: row.append(prev[c − 1] + prev[c])

    if r > 0: row.append(1)

return triangle
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(rows²)** | **O(rows²)** |
| **Optimized** | **O(rows²)** | **O(rows²)** |

---

# Climbing Stairs

**LeetCode #70** · [LeetCode](https://leetcode.com/problems/climbing-stairs/) · **Easy**

> **Dynamic Programming · Fibonacci recurrence · 1 or 2 steps**

### Approaches

#### 1. Brute Force (Recursion)
From step `i`, choose either 1 step or 2 steps, giving `ways(i) = ways(i−1) + ways(i−2)`, but repeated subproblems cause exponential time.

**Pseudo Code:**

```text
ways(i):

    if i <= 1: return 1

    return ways(i−1) + ways(i−2)   // recomputes!
```

#### 2. Optimized (Tabulation)
Store the number of ways to reach each step so every subproblem is calculated once.

**Pseudo Code:**

```text
dp[0] ← 1; dp[1] ← 1

for i ← 2 to n:

    // ways = from one below + from two below

    dp[i] ← dp[i−1] + dp[i−2]

return dp[n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(2ⁿ)** | **O(n)** |
| **Optimized** | **O(n)** | **O(n)** |


---------


# Dice Combinations

**LeetCode #377** · [LeetCode](https://leetcode.com/problems/combination-sum-iv/) · **Medium**

> **Dynamic Programming · Tabulation · Order matters · 6 choices per state**

### Approaches

#### 1. Brute Force (Recursion)
For each sum `s`, try every die value from `1` to `6` and recursively solve `s−j`, but the same subproblems are recomputed many times.

**Pseudo Code:**

```text
ways(s):

    if s = 0: return 1              // the empty sequence

    total ← 0

    for j ← 1 to 6:

        if j <= s:

            total ← total + ways(s − j)

    return total                    // recomputes the same s repeatedly
```

#### 2. Optimized (Tabulation)
Let `dp[i]` be the number of ways to make sum `i`; for each `i`, add `dp[i−j]` for every valid die value `j` from `1` to `6`.

**Pseudo Code:**

```text
MOD ← 1e9 + 7

dp[0] ← 1                         // the empty sequence totals 0

for i ← 1 to n:

    for j ← 1 to 6:

        if j <= i:                // dp[i−j] must exist

            dp[i] ← (dp[i] + dp[i−j]) mod MOD

return dp[n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(6ⁿ)** | **O(n)** |
| **Optimized** | **O(n)** | **O(n)** |

---

# Maximum Subarray

**LeetCode #53** · [LeetCode](https://leetcode.com/problems/maximum-subarray/) · **Medium**

> **Array / DP · Kadane's Algorithm · Maximum contiguous sum**

### Approaches

#### 1. Brute Force
Try every possible contiguous subarray and calculate its sum, keeping track of the maximum.

**Pseudo Code:**

```text
best ← −∞

for i ← 0 to n − 1:

    for j ← i to n − 1:

        best ← max(best, sum(nums[i..j]))

return best
```

#### 2. Optimized (Kadane)
At each element, decide whether to **start a new subarray** or **extend the current one** using `cur = max(nums[i], cur + nums[i])`.

**Pseudo Code:**

```text
cur ← nums[0];  best ← nums[0]

for i ← 1 to n − 1:

    // extend the run, or restart at nums[i]

    cur  ← max(nums[i], cur + nums[i])

    best ← max(best, cur)

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(1)** |
| **Optimized** | **O(n)** | **O(1)** |


---

# House Robber

**LeetCode #198** · [LeetCode](https://leetcode.com/problems/house-robber/) · **Medium**

> **Dynamic Programming · 1D DP · Rob or skip each house**

### Approaches

#### 1. Brute Force (Recursion)
For each house, choose between skipping it or robbing it and skipping the adjacent house; repeated subproblems cause exponential time.

**Pseudo Code:**

```text
rob(i):

    if i < 0: return 0

    return max(rob(i−1), rob(i−2) + nums[i])   // recomputes!
```

#### 2. Optimized (Tabulation)
Store the maximum money obtainable up to each house using `dp[i] = max(dp[i−1], dp[i−2] + nums[i])`.

**Pseudo Code:**

```text
dp[0] ← nums[0]; dp[1] ← max(nums[0], nums[1])

for i ← 2 to n−1:

    // skip house i vs rob house i

    dp[i] ← max(dp[i−1], dp[i−2] + nums[i])

return dp[n−1]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(2ⁿ)** | **O(n)** |
| **Optimized** | **O(n)** | **O(1)** |

---

# Coin Change

**LeetCode #322** · [LeetCode](https://leetcode.com/problems/coin-change/) · **Medium**

> **Dynamic Programming · 1D DP · Minimum coins for each amount**

### Approaches

#### 1. Why Greedy Fails
Always choosing the largest coin can miss the optimal combination, e.g. `[1, 3, 4]`, amount `6`: greedy gives `4 + 1 + 1 = 3`, while `3 + 3 = 2`.

**Pseudo Code:**

```text id="q7k0v4"
// greedy: always take the largest coin that fits

// coins [1, 3, 4], amount 6 → 4 + 1 + 1 = 3 coins

// but optimal is 3 + 3 = 2 coins → greedy is wrong
```

#### 2. Optimized (Bottom-Up DP)
For each amount, try every coin and take the minimum of `dp[amount − coin] + 1`.

**Pseudo Code:**

```text id="2x3n8h"
dp[0] ← 0;  dp[1..amount] ← ∞

for a ← 1 to amount:

    for each coin c with a − c ≥ 0:

        dp[a] ← min(dp[a], dp[a − c] + 1)

return dp[amount] == ∞ ? −1 : dp[amount]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(amount × #coins)** | **O(amount)** |

---

# Longest Common Subsequence

**LeetCode #1143** · [LeetCode](https://leetcode.com/problems/longest-common-subsequence/) · **Medium**

> **Dynamic Programming · 2D DP · Match → diagonal + 1, else max(up, left)**

### Approaches

#### 1. Optimized (2-D Tabulation)
Let `dp[i][j]` be the LCS length of the first `i` characters of `s1` and first `j` characters of `s2`; if characters match, use diagonal + 1, otherwise take `max(up, left)`.

**Pseudo Code:**

```text id="6t6i0q"
dp = (m+1) × (n+1) grid of 0   // row 0 / col 0 = empty prefix

for i in 1..m: for j in 1..n:

    if s1[i−1] == s2[j−1]:

        // match → extend diagonal

        dp[i][j] = dp[i−1][j−1] + 1

    else:

        dp[i][j] = max(dp[i−1][j], dp[i][j−1])

return dp[m][n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(m × n)** | **O(m × n)** |

---

# Edit Distance

**LeetCode #72** · [LeetCode](https://leetcode.com/problems/edit-distance/) · **Hard**

> **Dynamic Programming · 2D DP · Match → diagonal, else 1 + min(insert, delete, replace)**

### Approaches

#### 1. Optimized (2-D Tabulation)
Let `dp[i][j]` be the minimum edits to convert the first `i` characters of `word1` into the first `j` characters of `word2`; matching characters use the diagonal value, otherwise take `1 + min(insert, delete, replace)`.

**Pseudo Code:**

```text id="v4s7e1"
dp = (m+1) × (n+1) grid

dp[i][0] = i (delete all) ; dp[0][j] = j (insert all)

for i in 1..m: for j in 1..n:

    if word1[i−1] == word2[j−1]:

        dp[i][j] = dp[i−1][j−1]

    else:

        dp[i][j] = 1 + min(dp[i−1][j−1], dp[i−1][j], dp[i][j−1])

return dp[m][n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(m × n)** | **O(m × n)** |


---

# Counting Bits

**LeetCode #338** · [LeetCode](https://leetcode.com/problems/counting-bits/) · **Easy**

> **Dynamic Programming · Bit Manipulation · DP on the last bit**

### Approaches

#### 1. Optimized (Last-Bit DP)
Remove the last bit using `i >> 1`, use the already computed result, and add the last bit with `(i & 1)`.

**Pseudo Code:**

```text id="2qv5p8"
def countBits(n):

    dp = [0] * (n + 1)

    for i in 1..n:

        # i>>1 drops the last bit (already counted)

        # i&1 adds that dropped bit back

        dp[i] = dp[i >> 1] + (i & 1)

    return dp
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(n)** |


----

# Decode Ways

**LeetCode #91** · [LeetCode](https://leetcode.com/problems/decode-ways/) · **Medium**

> **Dynamic Programming · 1D DP · 1 or 2 digits at a time**

### Approaches

#### 1. Optimized (DP over Prefixes)
Let `dp[i]` be the number of ways to decode the first `i` digits; add `dp[i−1]` for a valid single digit and `dp[i−2]` for a valid two-digit number from `10` to `26`.

**Pseudo Code:**

```text id="m9n5x2"
def numDecodings(s):

    dp = [0] * (n + 1)

    dp[0] = 1               # empty prefix: one way

    for i in 1..n:

        if s[i−1] != '0':   # one-digit letter

            dp[i] += dp[i−1]

        if i >= 2 and 10 <= int(s[i−2:i]) <= 26:

            dp[i] += dp[i−2]   # two-digit letter

    return dp[n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(n)** |

---
# Unique Paths

**LeetCode #62** · [LeetCode](https://leetcode.com/problems/unique-paths/) · **Medium**

> **Dynamic Programming · Grid DP · Paths from top and left**

### Approaches

#### 1. Optimized (Grid Tabulation)
Let `dp[i][j]` be the number of paths to cell `(i,j)`; initialize the first row and column with `1`, then use `dp[i][j] = dp[i−1][j] + dp[i][j−1]`.

**Pseudo Code:**

```text id="k8r3w1"
dp = R × C grid

fill top row and left column with 1

for i in 1..R−1: for j in 1..C−1:

    dp[i][j] = dp[i−1][j] + dp[i][j−1]

return dp[R−1][C−1]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(m × n)** | **O(m × n)** |

---

# Maximal Square

**LeetCode #221** · [LeetCode](https://leetcode.com/problems/maximal-square/) · **Medium**

> **Dynamic Programming · Grid DP · min(up, left, diagonal) + 1**

### Approaches

#### 1. Optimized (Grid Tabulation)
Let `dp[i][j]` be the side length of the largest all-1 square ending at `(i,j)`; if the cell is `1`, use `min(up, left, diagonal) + 1`, otherwise `0`. Return the largest side squared as the area.

**Pseudo Code:**

```text id="r5m2x8"
dp = R × C grid, best = 0

first row / col: dp = source value

for i in 1..R−1: for j in 1..C−1:

    if grid[i][j] == 0: dp[i][j] = 0

    else dp[i][j] = min(dp[i−1][j], dp[i][j−1], dp[i−1][j−1]) + 1

    best = max(best, dp[i][j])

return best * best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(m × n)** | **O(m × n)** |

---


# Longest Increasing Subsequence

**LeetCode #300** · [LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/) · **Medium**

> **Dynamic Programming · 1D DP · Longest increasing subsequence ending at each index**

### Approaches

#### 1. Optimized (DP on Ending Index)
Let `dp[i]` be the length of the longest increasing subsequence ending at `i`; for every previous `j < i` where `nums[j] < nums[i]`, update `dp[i] = max(dp[i], dp[j] + 1)`.

**Pseudo Code:**

```text id="e3k7q1"
n = len(nums)

dp = [1] * n                       // each element alone

for i in 0..n−1:

    for j in 0..i−1:

        if nums[j] < nums[i]:      // smaller predecessor

            dp[i] = max(dp[i], 1 + dp[j])

return max(dp)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n²)** | **O(n)** |

---


# Word Break

**LeetCode #139** · [LeetCode](https://leetcode.com/problems/word-break/) · **Medium**

> **Dynamic Programming · 1D DP · DP over prefixes · Dictionary lookup**

### Approaches

#### 1. Optimized (DP over Prefixes)
Let `dp[i]` indicate whether the first `i` characters can be segmented; if `dp[j]` is true and `s[j:i]` is a dictionary word, then `dp[i] = true`.

**Pseudo Code:**

```text id="u2n8k5"
def wordBreak(s, words):

    dict = set(words)

    dp = [False] * (n + 1)

    dp[0] = True                  # empty prefix

    for i in 1..n:

        for j in 0..i−1:

            # prefix splittable AND suffix is a word

            if dp[j] and s[j:i] in dict:

                dp[i] = True

                break

    return dp[n]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n³)** | **O(n)** |

---

# Maximum Profit in Job Scheduling

**LeetCode #1235** · [LeetCode](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) · **Hard**

> **Dynamic Programming · Sort by end time · Take or skip · Binary search previous compatible job**

### Approaches

#### 1. Optimized (DP + Binary Search)
Sort jobs by end time; for each job, either skip it or take its profit plus the best profit from the latest job ending before its start time.

**Pseudo Code:**

```text id="7x4m2q"
sort jobs by end time

dp = array of size n

for i in 0..n−1:

    p = latest j with end[j] <= start[i]   // binary search

    skip = dp[i−1]                         // (0 if i==0)

    take = profit[i] + (p>=0 ? dp[p] : 0)

    dp[i] = max(skip, take)

return dp[n−1]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n log n)** | **O(n)** |

---

# Paint House

**LeetCode #256** · [LeetCode](https://leetcode.com/problems/paint-house/) · **Medium**

> **Dynamic Programming · 2D DP · Choose a different color from the previous house**

### Approaches

#### 1. Optimized (Grid Tabulation)
Let `dp[i][c]` be the minimum cost to paint houses `0..i` with house `i` painted color `c`; add the current cost to the minimum of the other two colors from the previous house.

**Pseudo Code:**

```text
dp = R × 3 grid

dp[0] = costs[0]             // first house, no neighbour

for i in 1..R−1: for c in 0..2:

    dp[i][c] = costs[i][c] + min(dp[i−1][other two colours])

return min(dp[R−1])
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(n)** |

---

# Paint House II

**LeetCode #265** · [LeetCode](https://leetcode.com/problems/paint-house-ii/) · **Hard**

> **Dynamic Programming · 1D DP · Track the two smallest previous costs**

### Approaches

#### 1. Naive (Scan all other colors)
For every house and color, scan all other previous colors to find the minimum cost.

**Pseudo Code:**

```text
dp[0] = costs[0]

for i in 1..n−1:

    for c in 0..k−1:

        m = min(dp[i−1][c'] for c' != c)   // O(k) scan

        dp[i][c] = costs[i][c] + m

return min(dp[n−1])
```

#### 2. Optimized (Two-Smallest Trick)
Track the smallest and second-smallest DP costs from the previous house; use the smallest unless it has the same color, otherwise use the second-smallest.

**Pseudo Code:**

```text
dp[0] = costs[0]

for i in 1..n−1:

    (min1, min1Col, min2) = two smallest of dp[i−1]

    for c in 0..k−1:

        best = (c == min1Col) ? min2 : min1

        dp[i][c] = costs[i][c] + best

return min(dp[n−1])
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Naive** | **O(n × k²)** | **O(k)** |
| **Optimized** | **O(n × k)** | **O(k)** |

---

# Minimum Window Subsequence

**LeetCode #727** · [LeetCode](https://leetcode.com/problems/minimum-window-subsequence/) · **Hard**

> **Dynamic Programming · 2D DP · Carry the start index of the subsequence**

### Approaches

#### 1. Optimized (Grid DP)
Let `dp[i][j]` store the starting index in `S` of a window ending at `i` that contains the first `j` characters of `T` as a subsequence; on a match use the diagonal value, otherwise carry the value from above, then track the shortest valid window.

**Pseudo Code:**

```text
dp[(n+1) × (m+1)], −1 = no window

dp[i][0] = i             // empty T → start = i

for i in 1..n: for j in 1..m:

    if S[i−1] == T[j−1]: dp[i][j] = dp[i−1][j−1]   // diagonal

    else:                 dp[i][j] = dp[i−1][j]     // carry from above

    if dp[i][m] valid: window = S[dp[i][m]..i)

        keep it if shorter than best

return best window
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n × m)** | **O(n × m)** |
