# Table of Contents

- [Word Search](#word-search)
- [Subsets](#subsets)
- [Permutations](#permutations)
- [Letter Combinations of a Phone Number](#letter-combinations-of-a-phone-number)
- [Generate Parentheses](#generate-parentheses)
- [Combination Sum](#combination-sum)
- [Palindrome Partitioning](#palindrome-partitioning)
- [N-Queens](#n-queens)

---

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

- DSA 05: pg no : 165
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
### Dry Run

This pseudocode generates all possible subsets of an array using backtracking.

Input: `nums = [1, 2, 3]`

A subset is any combination of elements, including the empty subset `[]` and the complete subset `[1, 2, 3]`.

For an array of \\(n\\) elements, the total number of subsets is \\(2^n\\). Here, \\(2^3 = 8\\) subsets.

### 1. Explain the pseudocode line by line

```text
subsets(start, path):
    record path
    for i in start..n−1:
        path.push(arr[i])
        subsets(i+1, path)
        path.pop()
```

#### Line 1: `subsets(start, path)`

- `start` tells us the index from which to choose elements.
- `path` stores the subset we are currently building.

Initially:

`start = 0`, `path = []`

#### Line 2: Record the current path

```text
record path
```

Every current `path` is a valid subset, so save it in the answer.

Initially, `path = []`, so the empty subset is recorded first.

#### Line 3: Loop through the remaining elements

```text
for i in start..n−1:
```

Try each element starting at index `start`.

For `[1, 2, 3]`, the indices are:

| Index | Value |
| :---: | :---: |
| **0** | 1 |
| **1** | 2 |
| **2** | 3 |

#### Line 4: Choose an element

```text
path.push(arr[i])
```

Add the selected element to the current subset.

Example: `[]` becomes `[1]`.

#### Line 5: Explore further subsets

```text
subsets(i+1, path)
```

Call the function recursively to choose elements that come after the selected element.

If we choose `1` at index `0`, the next call starts at index `1`, so we can choose `2` or `3`.

This avoids generating duplicate subsets such as `[2, 1]`.

#### Line 6: Backtrack

```text
path.pop()
```

Remove the last selected element after exploring that choice.

For example:

`[1, 2] → [1]`

This lets us try a different choice, such as selecting `3` instead of `2`.

Remember: `push()` means choose, the recursive call means explore, and `pop()` means undo the choice.

### 2. Dry run for `nums = [1, 2, 3]`

The function explores subsets in this order:

Each branch represents a choice. An empty choice is also a subset.

```text
[]
 └─ Choose 1 → [1]
      ├─ Choose 2 → [1, 2]
      │    └─ Choose 3 → [1, 2, 3]
      └─ Choose 3 → [1, 3]
 └─ Choose 2 → [2]
      └─ Choose 3 → [2, 3]
 └─ Choose 3 → [3]
```

After backtracking to the empty path, the function also explores subsets starting with 2 and then 3.

### Step-by-step execution table

| Step | Action | Current `path` | Subset recorded |
| :---: | :--- | :---: | :---: |
| **1** | Start | `[]` | `[]` |
| **2** | Choose 1 | `[1]` | `[1]` |
| **3** | Choose 2 | `[1, 2]` | `[1, 2]` |
| **4** | Choose 3 | `[1, 2, 3]` | `[1, 2, 3]` |
| **5** | Backtrack, remove 3 | `[1, 2]` | — |
| **6** | Backtrack, remove 2; choose 3 | `[1, 3]` | `[1, 3]` |
| **7** | Backtrack, remove 1 | `[]` | — |
| **8** | Choose 2 | `[2]` | `[2]` |
| **9** | Choose 3 | `[2, 3]` | `[2, 3]` |
| **10** | Backtrack; choose 3 alone | `[3]` | `[3]` |

The empty subset is recorded at the beginning, and the remaining subsets are recorded during recursive calls.

### ✅ Final Output

### All subsets of `[1, 2, 3]`

1. `[]`
2. `[1]`
3. `[1, 2]`
4. `[1, 2, 3]`
5. `[1, 3]`
6. `[2]`
7. `[2, 3]`
8. `[3]`

Total subsets: 8

The exact order depends on when `record path` occurs. With this pseudocode, the output order is:

# `[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]`

### 🧠 Key idea

Complexity:
- Time: \\(O(n \cdot 2^n)\\), because there are \\(2^n\\) subsets and copying each subset can take up to \\(O(n)\\).
- Auxiliary space: \\(O(n)\\) for the current path and recursion stack, excluding the stored output.

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
