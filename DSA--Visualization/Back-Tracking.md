# Word Search

**LeetCode #79** · [LeetCode](https://leetcode.com/problems/word-search/) · **Medium**

> **Backtracking · DFS · Mark & unmark · 4-directional grid**

### Approaches

#### 1. Optimized (DFS + Backtracking)
Start DFS from every cell matching the first character. Mark the current cell as used, explore its 4 neighbors, then unmark it when backtracking. Each cell can be used only once in the current path.

**Pseudo Code:**

```text
exist(word):

    for each start cell == word[0]:

        if dfs(r, c, 0): return true

dfs(r, c, i):                  // board[r][c] == word[i]

    mark (r, c) used

    if i == last index: return true

    for each neighbour (nr, nc):

        if used or letter != word[i+1]: skip

        if dfs(nr, nc, i+1): return true

    unmark (r, c)              // backtrack

    return false
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(R · C · 4ᴸ)** | **O(L)** |

---

# Subsets

**LeetCode #78** · [LeetCode](https://leetcode.com/problems/subsets/) · **Medium**

> **Backtracking · Choose / un-choose · Every node is a subset**

### Approaches

#### 1. Optimized (Backtracking)
At every recursive call, add the current `path` to the result because every node in the recursion tree represents a valid subset. Choose an element, recursively explore later elements, then `pop()` to backtrack.

**Pseudo Code:**

```text
subsets(start, path):

    record path                 // every node is a subset

    for i in start..n−1:

        path.push(arr[i])       // choose

        subsets(i+1, path)      // explore

        path.pop()              // un-choose (backtrack)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(2ⁿ · n)** | **O(n)** |

---

# Permutations

**LeetCode #46** · [LeetCode](https://leetcode.com/problems/permutations/) · **Medium**

> **Backtracking · Choose an unused element at each level**

### Approaches

#### 1. Optimized (Backtracking)
At each level, choose any unused element, add it to the current `path`, and continue recursively. When `path` reaches size `n`, record the permutation. Then unmark and remove the element to backtrack.

**Pseudo Code:**

```text
permute(path, used):

    if path.length == n:

        record path             // a complete permutation

    for num in nums where !used[num]:

        used[num] = true; path.push(num)   // choose

        permute(path, used)                // explore

        used[num] = false; path.pop()      // un-choose (backtrack)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n · n!)** | **O(n)** |

---

# Letter Combinations of a Phone Number

**LeetCode #17** · [LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) · **Medium**

> **Backtracking · One digit per tree level · Choose one letter per digit**

### Approaches

#### 1. Optimized (Backtracking)
Process one digit at each recursion level. Try every letter mapped to that digit, add it to the current path, recursively process the next digit, then `pop()` to backtrack.

**Pseudo Code:**

```text id="pc4927"
combine(index, path):

    if index == len(digits):

        record path             // a full combination (leaf)

        return

    for letter in keypad[digits[index]]:

        path.push(letter)       // choose

        combine(index+1, path)  // explore next digit

        path.pop()              // un-choose (backtrack)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(4ⁿ · n)** | **O(n)** |

---

# Generate Parentheses

**LeetCode #22** · [LeetCode](https://leetcode.com/problems/generate-parentheses/) · **Medium**

> **Backtracking · Open/close counters · Validity pruning**

### Approaches

#### 1. Optimized (Backtracking)
Add `(` while `opens < n`; add `)` only while `closes < opens`, so invalid parentheses are pruned early.

**Pseudo Code:**

```text
gen(s, opens, closes):

    if s.length == 2n: record s; return    // complete

    // choose ( while opens < n

    if opens < n:

        gen(s + "(", opens + 1, closes)

    // choose ) while closes < opens, else prune

    if closes < opens:

        gen(s + ")", opens, closes + 1)

    return                                  // un-choose by returning

// answer = all recorded strings
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(4ⁿ / √n)** | **O(n)** |

---

# Combination Sum

**LeetCode #39** · [LeetCode](https://leetcode.com/problems/combination-sum/) · **Medium**

> **Backtracking · Reuse allowed · Overshoot pruning**

### Approaches

#### 1. Optimized (Backtracking)
Pick candidates from the current index onward; reuse the same candidate with `i`, and stop when the candidate exceeds the remaining target.

**Pseudo Code:**

```text id="8x2vkm"
combo(start, rem, path):

    if rem == 0: record path; return       // exact hit

    for i in start..n−1:

        if cand[i] > rem: prune; continue  // overshoot

        path.push(cand[i])                  // choose

        combo(i, rem − cand[i], path)      // explore (reuse: i, not i+1)

        path.pop()                          // un-choose

    return
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n^(T/min))** | **O(T/min)** |

---

# Palindrome Partitioning

**LeetCode #131** · [LeetCode](https://leetcode.com/problems/palindrome-partitioning/) · **Medium**

> **Backtracking · Palindrome prefix · Cut only at valid palindromes**

### Approaches

#### 1. Optimized (Backtracking)
Try every substring from the current position; only choose it if it is a palindrome, then recurse from the next position.

**Pseudo Code:**

```text id="6q3z8p"
partition(pos, parts):

    if pos == len(s): record parts; return  // reached the end

    for end in pos+1..len(s):

        prefix = s[pos..end]

        if not isPalindrome(prefix): prune; continue

        parts.push(prefix)                  // choose

        partition(end, parts)               // explore

        parts.pop()                         // un-choose

    return

// answer = all recorded partitions
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n · 2ⁿ)** | **O(n)** |

---

# N-Queens

**LeetCode #51** · [LeetCode](https://leetcode.com/problems/n-queens/) · **Hard**

> **Backtracking · One queen per row · Check column + diagonals**

### Approaches

#### 1. Optimized (Backtracking)
Place one queen in each row; try every column and reject positions sharing a column or diagonal with an existing queen.

**Pseudo Code:**

```text id="v4m2sx"
solve(N):

    return place(row = 0)

place(row):

    if row == N: return true        // all queens placed

    for col in 0..N-1:

        if attacked(row, col):

            continue                // reject, try next column

        board[row][col] = Q         // choose

        if place(row + 1): return true

        board[row][col] = .         // un-choose (backtrack)

    return false                   // no safe column in this row

attacked(row, col):

    any earlier queen in same column or diagonal
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(N!)** | **O(N)** |

---
