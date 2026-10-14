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

- DSA 5 page no: 180
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
### Dry Run

This pseudocode solves the Letter Combinations of a Phone Number problem using recursion and backtracking.

Given a string of digits, we generate all possible letter combinations using the letters mapped to each digit on a phone keypad.

Input: `digits = "23"`

On a phone keypad:

| Digit | Letters |
| :---: | :---: |
| **2** | ABC |
| **3** | DEF |
| **4** | GHI |
| **5** | JKL |
| **6** | MNO |
| **7** | PQRS |
| **8** | TUV |
| **9** | WXYZ |

Digit `2` gives `A, B, C`, and digit `3` gives `D, E, F`.

### 1. Explain the pseudocode line by line

```text
combine(index, path):
    if index == len(digits):
        record path
        return

    for letter in keypad[digits[index]]:
        path.push(letter)
        combine(index+1, path)
        path.pop()
```

#### Line 1: `combine(index, path)`

- `index` tells us which digit we are currently processing.
- `path` stores the letters selected so far.

Initially:

```text
index = 0
path = ""
digits = "23"
```

#### Line 2: Check whether all digits are processed

```text
if index == len(digits):
```

The input `"23"` has length `2`.

When `index == 2`, both digits have been processed, so we have a complete combination.

#### Line 3: Record the combination

```text
record path
return
```

Save the current `path` as one answer and return from that recursive call.

For example, if `path = "ad"`, record `"ad"`.

#### Line 4: Loop through the current digit's letters

```text
for letter in keypad[digits[index]]:
```

Get all letters mapped to the current digit.

- At `index = 0`, digit `2` gives `A, B, C`.
- At `index = 1`, digit `3` gives `D, E, F`.

#### Line 5: Choose a letter

```text
path.push(letter)
```

Add a letter to the current combination.

Example:

`"" → "a"`

#### Line 6: Process the next digit

```text
combine(index+1, path)
```

Move to the next digit and choose one of its letters.

Example:

`combine(1, "a")`

Now the algorithm chooses `d`, `e`, and `f`, creating `"ad"`, `"ae"`, and `"af"`.

#### Line 7: Backtrack

```text
path.pop()
```

Remove the last letter after exploring that choice, so we can try another letter.

Example:

`"ad" → "a"`

Now we can try `"e"` instead of `"d"`.

### 2. Dry run for `digits = "23"`

The recursion explores each letter for digit `2`, and then every letter for digit `3`.

Search tree:

```text
path = ""
 └─ Choose 'a' (digit 2)
      ├─ Choose 'd' (digit 3) → "ad" ✅
      ├─ Choose 'e' (digit 3) → "ae" ✅
      └─ Choose 'f' (digit 3) → "af" ✅
 └─ Choose 'b' (digit 2)
      ├─ Choose 'd' (digit 3) → "bd" ✅
      ├─ Choose 'e' (digit 3) → "be" ✅
      └─ Choose 'f' (digit 3) → "bf" ✅
 └─ Choose 'c' (digit 2)
      ├─ Choose 'd' (digit 3) → "cd" ✅
      ├─ Choose 'e' (digit 3) → "ce" ✅
      └─ Choose 'f' (digit 3) → "cf" ✅
```

### Step-by-step execution

| Step | Action | `path` | Result |
| :---: | :--- | :---: | :--- |
| **1** | Choose `a` for digit 2 | `"a"` | Continue to digit 3 |
| **2** | Choose `d` | `"ad"` | Record `"ad"` |
| **3** | Backtrack; choose `e` | `"ae"` | Record `"ae"` |
| **4** | Backtrack; choose `f` | `"af"` | Record `"af"` |
| **5** | Backtrack; choose `b` | `"b"` | Start next group |
| **6** | Choose `d`, `e`, `f` | `"bd"`, `"be"`, `"bf"` | Record all three |
| **7** | Backtrack; choose `c` | `"c"` | Start final group |
| **8** | Choose `d`, `e`, `f` | `"cd"`, `"ce"`, `"cf"` | Record all three |

Each output contains exactly two letters: one from digit `2` and one from digit `3`.

### ✅ Final Output

### All letter combinations:

* `"ad"`, `"ae"`, `"af"`
* `"bd"`, `"be"`, `"bf"`
* `"cd"`, `"ce"`, `"cf"`

Output:

# `["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]`

Total combinations: \\(3 \times 3 = 9\\)

### 🧠 Key idea

Complexity:
- Time: \\(O(3^m \times 4^n \times (m+n))\\) in the general keypad case, commonly expressed as \\(O(4^k \cdot k)\\) as an upper bound, where \\(k\\) is the number of digits.
- Auxiliary space: \\(O(k)\\) for the recursion stack and current path, excluding the output.

Interview tip: `path.push(letter)` chooses a letter, the recursive call processes the next digit, and `path.pop()` undoes the choice so another letter can be tried.

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
### Dry Run

This pseudocode generates all valid combinations of `n` pairs of parentheses using recursion and backtracking.

Input: `n = 3`

We need to generate strings containing 3 opening parentheses `(` and 3 closing parentheses `)`, while ensuring that every closing parenthesis has a matching opening parenthesis before it.

### 1. Explain the pseudocode line by line

```text
gen(s, opens, closes):
    if s.length == 2n:
        record s
        return

    if opens < n:
        gen(s + "(", opens + 1, closes)

    if closes < opens:
        gen(s + ")", opens, closes + 1)

    return
```

#### Line 1: `gen(s, opens, closes)`

The function has three parameters:

- `s`: the string being constructed.
- `opens`: the number of opening parentheses used.
- `closes`: the number of closing parentheses used.

Initially:

```text
s = ""
opens = 0
closes = 0
n = 3
```

#### Line 2: Check whether the string is complete

```text
if s.length == 2n:
    record s
    return
```

Since `n = 3`, the string must contain \\(2 \times 3 = 6\\) characters.

When `s.length == 6`, record the string and return.

For example, `"((()))"` has 6 characters, so it is complete.

#### Line 3: Choose an opening parenthesis

```text
if opens < n:
    gen(s + "(", opens + 1, closes)
```

We can add `(` only while fewer than 3 opening parentheses have been used.

For example:

```text
s = ""
```

Adding `(` gives:

```text
s = "("
opens = 1
closes = 0
```

The recursive call explores this choice further.

#### Line 4: Choose a closing parenthesis

```text
if closes < opens:
    gen(s + ")", opens, closes + 1)
```

We can add `)` only when `closes < opens`.

This prevents invalid strings such as `")("`, because we cannot close a parenthesis before opening one.

For example, if:

```text
opens = 2
closes = 1
```

We can add `)` because \\(1 < 2\\).

#### Line 5: Return and backtrack

```text
return
```

Each recursive call returns to its previous call after exploring a choice. The algorithm then tries the other valid choice.

Unlike an array-based backtracking algorithm, this pseudocode creates a new string with `s + "("` or `s + ")"`, so it doesn't need an explicit `pop()`.

### 2. Dry run for `n = 3`

Let's explore the valid strings.

Start with an empty string. Add only parentheses that satisfy the rules:

```text
""
 └─ Add '(' → "("
      ├─ Add '(' → "(("
      │    ├─ Add '(' → "(((" → Add ')' → "((()" → Add ')' → "((())" → Add ')' → "((()))" ✅
      │    └─ Add ')' → "(()"
      │         ├─ Add '(' → "(()(" → Add ')' → "(()()" → Add ')' → "(()())" ✅
      │         └─ Add ')' → "(())" → Add '(' → "(())(" → Add ')' → "(())()" ✅
      └─ Add ')' → "()"
           └─ Add '(' → "()("
                ├─ Add '(' → "()((" → Add ')' → "()(()" → Add ')' → "()(())" ✅
                └─ Add ')' → "()()" → Add '(' → "()()(" → Add ')' → "()()()" ✅
```

### Example: How `"((()))"` is generated

| Step | Action | `s` | `opens` | `closes` |
| :---: | :--- | :---: | :---: | :---: |
| **1** | Start | `""` | 0 | 0 |
| **2** | Add `(` | `"("` | 1 | 0 |
| **3** | Add `(` | `"(("` | 2 | 0 |
| **4** | Add `(` | `"((("` | 3 | 0 |
| **5** | Add `)` | `"((()"` | 3 | 1 |
| **6** | Add `)` | `"((())"` | 3 | 2 |
| **7** | Add `)` | `"((()))"` | 3 | 3 |
| **8** | Length is 6 | `"((()))"` | 3 | 3 |

The string is recorded because its length is 6 and all parentheses are matched.

The algorithm explores other branches as well, producing four more valid strings.

### ✅ Final Output

### Valid parentheses combinations for `n = 3`:

1. `"((()))"`
2. `"(()())"`
3. `"(())()"`
4. `"()(())"`
5. `"()()()"`

Output:

# `["((()))", "(()())", "(())()", "()(())", "()()()"]`

Total valid combinations: 5

### 🧠 Key idea

Remember the two rules:
- `opens < n` — you can add an opening parenthesis if fewer than `n` have been used.
- `closes < opens` — you can add a closing parenthesis only if there is an unmatched opening parenthesis.

Complexity: The number of valid strings is the \\(n\\)-th Catalan number. For `n = 3`, there are 5 strings. The algorithm uses \\(O(n)\\) recursion depth, excluding the output; generating and storing all strings requires output space proportional to the total length of all results.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(4ⁿ / √n)** | **O(n)** |

---

# Combination Sum

**LeetCode #39** · [LeetCode](https://leetcode.com/problems/combination-sum/) · **Medium**

> **Backtracking · Reuse allowed · Overshoot pruning**
- DSA 5 : pg no : 169

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

### Dry Run

This pseudocode solves the Combination Sum problem using recursion and backtracking.

The goal is to find all combinations of candidates that add up to the target. You can reuse the same candidate multiple times.

Input:

- `candidates = [2, 3, 6, 7]`
- `target = 7`

### 1. Explain the pseudocode line by line

```text
combo(start, rem, path):
    if rem == 0:
        record path
        return

    for i in start..n−1:
        if cand[i] > rem:
            prune
            continue

        path.push(cand[i])
        combo(i, rem − cand[i], path)
        path.pop()

    return
```

#### Line 1: `combo(start, rem, path)`

This function has three parameters:

- `start`: index where we begin checking candidates.
- `rem`: remaining target we need to make.
- `path`: current combination we are building.

Initially:

```text
start = 0
rem = 7
path = []
```

#### Line 2: Check whether the target is reached

```text
if rem == 0:
    record path
    return
```

If `rem` becomes `0`, the combination adds up exactly to `7`.

For example, if `path = [2, 2, 3]`:

\\[ 2+2+3=7 \\]

Record `[2, 2, 3]` as a valid answer and return from that recursive call.

#### Line 3: Loop through candidates

```text
for i in start..n−1:
```

Try candidates starting from index `start`.

The candidates are:

| Index | Candidate |
| :---: | :---: |
| **0** | 2 |
| **1** | 3 |
| **2** | 6 |
| **3** | 7 |

#### Line 4: Prune candidates that are too large

```text
if cand[i] > rem:
    prune
    continue
```

If the candidate is greater than the remaining target, skip it.

For example, if `rem = 1` and the next candidate is `2`, we skip `2` because it would exceed the remaining target.

This works because the candidates are sorted and positive.

#### Line 5: Choose a candidate

```text
path.push(cand[i])
```

Add the candidate to the current combination.

Example:

→ [2]

#### Line 6: Explore recursively

```text
combo(i, rem − cand[i], path)
```

Subtract the chosen candidate from `rem` and search for the remaining sum.

Important: The next call uses `i`, not `i + 1`. This allows the same candidate to be selected again.

Example:

```text
path = [2]
rem = 5
```

Choosing `2` again gives:

```text
path = [2, 2]
rem = 3
```

#### Line 7: Backtrack

```text
path.pop()
```

Remove the last candidate after exploring that choice.

Example:

`[2, 2, 3] → [2, 2]`

This lets the algorithm try another candidate.

### 2. Dry run for `candidates = [2, 3, 6, 7]`, `target = 7`

The algorithm tries each candidate and explores possible combinations.

Search tree (showing the successful paths and key pruned branches):

```text
path = [], rem = 7
 └─ Choose 2 → [2], rem = 5
      └─ Choose 2 again → [2, 2], rem = 3
           ├─ Choose 2 → [2, 2, 2], rem = 1 (pruned: 2 > 1)
           └─ Choose 3 → [2, 2, 3], rem = 0 ✅ (Valid combination!)
 └─ Backtrack to path = [], rem = 7
 └─ Choose 7 directly → [7], rem = 0 ✅ (Valid combination!)
```

### Important recursive steps

| Current `path` | Remaining `rem` | Action |
| :---: | :---: | :--- |
| `[]` | 7 | Choose 2 |
| `[2]` | 5 | Choose 2 again |
| `[2, 2]` | 3 | Choose 2 again; remaining becomes 1 |
| `[2, 2, 2]` | 1 | Skip 2 and 3 because both exceed 1; backtrack |
| `[2, 2]` | 3 | Choose 3 |
| `[2, 2, 3]` | 0 | Record `[2, 2, 3]` |
| `[]` | 7 | Backtrack and try 3, then 6 |
| `[]` | 7 | Choose 7 |
| `[7]` | 0 | Record `[7]` |

The combination `[2, 2, 3]` is found first. After backtracking, the algorithm eventually finds `[7]`.

### ✅ Final Output

### Valid combinations:

* `[2, 2, 3]` (2 + 2 + 3 = 7)
* `[7]` (7 = 7)

Output:

# `[[2, 2, 3], [7]]`

### 🧠 Key idea

Complexity: The search can take exponential time in the target value because candidates can be reused. Auxiliary recursion/path space is proportional to the maximum combination length, approximately \\(O(\text{target}/\min(\text{candidates}))\\), excluding the output.

Interview tip: `combo(i, ...)` allows reuse of the current candidate. If you used `combo(i + 1, ...)`, each candidate could be used at most once.
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
