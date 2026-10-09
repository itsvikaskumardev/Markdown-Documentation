# Table of Contents

- [Max Sum Subarray of Size K](#max-sum-subarray-of-size-k)
- [Max Sum of Distinct Subarrays, Size K](#max-sum-of-distinct-subarrays-size-k)
- [Max Points From Cards](#max-points-from-cards)
- [Variable-Size Window](#variable-size-window)
- [Longest Substring Without Repeats](#longest-substring-without-repeats)
- [Longest Repeating Character Replacement](#longest-repeating-character-replacement)
- [Minimum Window Substring](#minimum-window-substring)
- [Permutation in String](#permutation-in-string)
- [Sliding Window Maximum](#sliding-window-maximum)

---

# Max Sum Subarray of Size K

**LeetCode #643** · [LeetCode](https://leetcode.com/problems/maximum-average-subarray-i/) · **Easy**

> **Array · Fixed-Size Sliding Window · Maximum Sum**

### Approaches

#### 1. Brute Force (Recompute Each Window)
For every starting index, calculate the sum of the next `K` elements and keep the maximum.

**Pseudo Code:**

```text
given arr, K

max_sum ← −∞

for i ← 0 to n − K:
    sum ← 0

    for j ← i to i + K − 1:
        sum += arr[j]

    max_sum = max(max_sum, sum)

return max_sum
```

#### 2. Optimized (Sliding Window)
Calculate the first window's sum, then slide one position at a time by **adding the new element and removing the outgoing element**.

**Pseudo Code:**

```text
given arr, K

sum ← sum of arr[0..K − 1]

max_sum ← sum

for right ← K to n − 1:
    sum += arr[right] − arr[right − K]
    max_sum = max(max_sum, sum)

return max_sum
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Recompute Each Window** | **O(n · K)** | **O(1)** |
| **Optimized · Sliding Window** | **O(n)** | **O(1)** |

---

# Max Sum of Distinct Subarrays, Size K

**LeetCode #2461** · [LeetCode](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/) · **Medium**

> **Array · Fixed-Size Sliding Window · Frequency Map**

### Approaches

#### 1. Brute Force (Re-check Every Window)
For every window of size `K`, check whether all elements are distinct using a set, then calculate its sum.

**Pseudo Code:**

```text id="u5t0bq"
given arr, K

best ← 0

for i ← 0 to n − K:
    win ← arr[i .. i + K − 1]

    if win has a duplicate: skip

    best ← max(best, sum(win))

return best
```

#### 2. Optimized (Sliding Window + Frequency Map)
Maintain the window sum and a frequency map while sliding. If the window contains exactly `K` distinct elements, update the maximum sum.

**Pseudo Code:**

```text id="j6l4qk"
given arr, K

freq ← {}; sum ← 0; best ← 0

for r ← 0 to n − 1:
    absorb arr[r] into sum, freq

    if r ≥ K:
        drop arr[r − K]

    if window full and |freq| < K: skip
    else: best ← max(best, sum)

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Re-check Every Window** | **O(n · K)** | **O(K)** |
| **Optimized · Sliding Window + Frequency Map** | **O(n)** | **O(K)** |

---

# Max Points From Cards

**LeetCode #1423** · [LeetCode](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/) · **Medium**

> **Array · Sliding Window · Complement Window**

### Approaches

#### 1. Brute Force (Try Every Front/Back Split)
Try every possible way to take `K` cards from the front and back, calculate the total score, and keep the maximum.

**Pseudo Code:**

```text id="p3x8nm"
given cards, K

best ← 0

for t ← 0 to K:
    score ← sum(front t) + sum(back K − t)
    best ← max(best, score)

return best
```

#### 2. Optimized (Minimize the Window You Leave)
If you take exactly `K` cards, you leave a contiguous window of `n - K` cards. Find the **minimum-sum window** of size `n - K`, then subtract it from the total sum.

**Pseudo Code:**

```text id="8kq2vd"
total ← sum(cards)

W ← n − K             // the middle you LEAVE

winSum ← sum(cards[0..W − 1])
minSum ← winSum

for r ← W to n − 1:
    winSum += cards[r] − cards[r − W]
    minSum ← min(minSum, winSum)

return total − minSum
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Try Every Front/Back Split** | **O(K)** | **O(1)** |
| **Optimized · Complement Sliding Window** | **O(n)** | **O(1)** |

---

# Variable-Size Window

**Concept · Variable-Size Sliding Window**

> **Array · Expand → Repair → Record**

### Approaches

#### 1. Variable-Size Window
Use two pointers, `l` and `r`. Expand the window by moving `r`, and whenever the window violates the required condition, move `l` forward until the condition is valid again. Then record the best valid window.

**Pseudo Code:**

```text id="x7m2kp"
// the variable-window template

l ← 0; best ← 0

for r ← 0 to n − 1:

    window absorbs arr[r]          // expand

    while invariant broken:       // here: sum ≤ 8
        window drops arr[l]; l++  // contract

    best ← max(best, r − l + 1)   // record

return best
```

**Template:**
`Expand → Repair/Contract → Record`

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Variable-Size Window** | **O(n)** | **O(1)** |

---

# Longest Substring Without Repeats

**LeetCode #3** · [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/) · **Medium**

> **String · Variable-Size Sliding Window · Last-Seen Map**

### Approaches

#### 1. Brute Force (Restart on Duplicate)
Start from every character and expand until a duplicate is found, using a set to track characters.

**Pseudo Code:**

```text
given s

max_len ← 0

for i ← 0 to n − 1:
    seen ← ∅

    for j ← i to n − 1:
        if s[j] in seen: break

        seen.add(s[j])

    max_len ← max(max_len, j − i)

return max_len
```
### Dry Run
We need to find the length of the longest substring without repeating characters.

### 1. Given input

```
s = "abcabcbb"
```

Characters with their indices:

| Index     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --------- | - | - | - | - | - | - | - | - |
| Character | a | b | c | a | b | c | b | b |

### 2. Explain the pseudocode line by line

Line 1: `max_len ← 0`

Initialize `max_len` to `0`. It stores the maximum length of a substring without repeating characters found so far.

Line 2: `for i ← 0 to n − 1:`

The outer loop chooses each character as the starting point of a substring.

Here, `n = 8`, so `i` goes from `0` to `7`.

Line 3: `seen ← ∅`

Create an empty set called `seen` for each new starting index `i`.

The set stores characters already visited in the current substring.

Line 4: `for j ← i to n − 1:`

The inner loop extends the substring from starting index `i`, moving forward using `j`.

Line 5: `if s[j] in seen: break`

Check whether the current character has already appeared in `seen`.

* If yes, stop the inner loop because the substring would contain a duplicate.

* If no, continue to the next line.

Line 6: `seen.add(s[j])`

Add the current character to the set.

Line 7: `max_len = max(max_len, j − i)`

Update the maximum length if the current substring is longer.

Important correction: As written, `j − i` is off by one for a substring with no duplicates. Its length should be `j − i + 1` when `j` is the last successfully included character. Also, if the loop ends because of a duplicate, `j` points at that duplicate, so the original expression can accidentally give a length that seems correct in some cases but is not reliable.

We'll use the corrected expression in the dry run.

Line 8: `return max_len`

Return the longest substring length found.

### 3. Dry run step by step

#### Iteration 1: `i = 0`

Starting character: `a`

## a

0

## b

1

## c

2

## a

3

* `j = 0`: `a` not in set → add `a`. Set = `{a}`. Length = 1.

* `j = 1`: `b` not in set → add `b`. Set = `{a,b}`. Length = 2.

* `j = 2`: `c` not in set → add `c`. Set = `{a,b,c}`. Length = 3.

* `j = 3`: `a` is already in the set → `break`.

Longest substring from this start: `"abc"` (length 3).

`max_len = 3`

#### Iteration 2: `i = 1`

Starting character: `b`

* `j = 1`: `b` → add. Length = 1.

* `j = 2`: `c` → add. Length = 2.

* `j = 3`: `a` → add. Length = 3.

* `j = 4`: `b` repeats → `break`.

Substring: `"bca"` (length 3).

`max_len = max(3, 3) = 3`

#### Iteration 3: `i = 2`

Starting character: `c`

* `j = 2`: `c` → add. Length = 1.

* `j = 3`: `a` → add. Length = 2.

* `j = 4`: `b` → add. Length = 3.

* `j = 5`: `c` repeats → `break`.

Substring: `"cab"` (length 3).

`max_len = 3`

#### Iteration 4: `i = 3`

Starting character: `a`

* `j = 3`: `a` → add. Length = 1.

* `j = 4`: `b` → add. Length = 2.

* `j = 5`: `c` → add. Length = 3.

* `j = 6`: `b` repeats → `break`.

Substring: `"abc"` (length 3).

`max_len = 3`

#### Iteration 5: `i = 4`

Starting character: `b`

* `j = 4`: `b` → add. Length = 1.

* `j = 5`: `c` → add. Length = 2.

* `j = 6`: `b` repeats → `break`.

Substring: `"bc"` (length 2).

`max_len = 3`

#### Iteration 6: `i = 5`

* `j = 5`: `c` → add. Length = 1.

* `j = 6`: `b` → add. Length = 2.

* `j = 7`: `b` repeats → `break`.

Substring: `"cb"` (length 2).

`max_len = 3`

#### Iteration 7: `i = 6`

* `j = 6`: `b` → add. Length = 1.

* `j = 7`: `b` repeats → `break`.

`max_len = 3`

#### Iteration 8: `i = 7`

- `j = 7`: `b` → add. Length = 1.

`max_len = 3`

### ✅ Final Output

Longest substring without repeating characters

# `"abc"`

Length

# 3

### 🧠 Note
To make the pseudocode fully correct, update the length calculation to `max_len = max(max_len, j − i + 1)` after adding a new character, or track the last valid index separately. The correct output for `"abcabcbb"` is 3.


#### 2. Better (Sliding Window + Set)
Maintain a window with unique characters. When a duplicate appears, move `L` forward and remove characters until the window is valid again.

**Pseudo Code:**

```text
given s

L ← 0; seen ← ∅; max_len ← 0

for R ← 0 to n − 1:
    while s[R] in seen:
        seen.remove(s[L])
        L++

    seen.add(s[R])

    max_len ← max(max_len, R − L + 1)

return max_len
```
### Dry Run
We need to find the length of the longest substring without repeating characters using the sliding window technique.

### 1. Given input

```
s = "pwwkew"
```

Characters with indices:

| Index     | 0 | 1 | 2 | 3 | 4 | 5 |
| --------- | - | - | - | - | - | - |
| Character | p | w | w | k | e | w |

### 2. Explain the pseudocode line by line

Line 1: `L ← 0; seen ← ∅; max_len ← 0`

* `L = 0`: Left boundary of the current substring (window).

* `seen = ∅`: An empty set to store characters in the current window.

* `max_len = 0`: Stores the maximum valid substring length found so far.

Line 2: `for R ← 0 to n − 1:`

Move `R` (the right boundary) from index `0` to `n − 1`.

Each step adds a new character to the window.

Line 3: `while s[R] in seen:`

Check whether the new character already exists in the current window.

If it does, the window has a duplicate, so we must shrink the window from the left.

Line 4: `seen.remove(s[L])`

Remove the character at index `L` from the set.

Line 5: `L++`

Move the left boundary one position to the right.

Repeat these steps until `s[R]` is no longer in `seen`.

Line 6: `seen.add(s[R])`

Add the current right-side character to the set. Now the window contains no duplicate characters.

Line 7: `max_len = max(max_len, R − L + 1)`

Calculate the current window length:

length=R−L+1\text{length}=R-L+1length=R−L+1

Update `max_len` if the current window is longer.

Line 8: `return max_len`

Return the longest substring length found.

### 3. Dry run step by step

#### Iteration 1: `R = 0`

## p

0

## w

1

## w

2

## k

3

## e

4

## w

5

* `s[R] = 'p'`

* `'p'` is not in `seen`, so the `while` loop does not run.

* Add `'p'` → `seen = {p}`.

* Length = `R − L + 1 = 0 − 0 + 1 = 1`.

`max_len = 1`

#### Iteration 2: `R = 1`

* `s[R] = 'w'`

* `'w'` is not in `seen`.

* Add `'w'` → `seen = {p, w}`.

* Length = `1 − 0 + 1 = 2`.

`max_len = 2`

Current window: `"pw"`

#### Iteration 3: `R = 2`

## p

0

## w

1

## w

2

## k

3

## e

4

## w

5

* `s[R] = 'w'`

* `'w'` already exists in `seen = {p, w}`, so enter the `while` loop.

* Remove `s[L] = 'p'` → `seen = {w}`; `L = 1`.

* `'w'` is still in `seen`, so continue the loop.

* Remove `s[L] = 'w'` → `seen = {}`; `L = 2`.

* Now `'w'` is not in `seen`. Exit the loop.

* Add `'w'` → `seen = {w}`.

* Length = `2 − 2 + 1 = 1`.

`max_len = 2`

Current window: `"w"`

#### Iteration 4: `R = 3`

* `s[R] = 'k'`

* `'k'` is not in `seen`.

* Add `'k'` → `seen = {w, k}`.

* Length = `3 − 2 + 1 = 2`.

`max_len = 2`

Current window: `"wk"`

#### Iteration 5: `R = 4`

* `s[R] = 'e'`

* `'e'` is not in `seen`.

* Add `'e'` → `seen = {w, k, e}`.

* Length = `4 − 2 + 1 = 3`.

`max_len = 3`

Current window: `"wke"`

#### Iteration 6: `R = 5`

* `s[R] = 'w'`

* `'w'` already exists in `seen = {w, k, e}`.

* Remove `s[L] = 'w'` → `seen = {k, e}`; `L = 3`.

* Now `'w'` is not in `seen`, so exit the loop.

* Add `'w'` → `seen = {k, e, w}`.

* Length = `5 − 3 + 1 = 3`.

`max_len = 3`

Current window: `"kew"`

### ✅ Final Output

Longest substring without repeating characters

# `"wke"` or `"kew"`

Length

# 3

Final answer: `3`

The longest valid substrings include `"wke"` and `"kew"`, each with length `3`.

### 🧠 Key idea
When a duplicate appears, move `L` forward and remove characters until the duplicate is gone. This keeps the sliding window free of repeated characters.


#### 3. Optimized (Last-Seen Map)
Store the last index of each character. When a duplicate appears, jump `L` directly to `max(L, lastSeen[c] + 1)` instead of moving one step at a time.

**Pseudo Code:**

```text
given s

L ← 0; last ← {}; max_len ← 0

for R ← 0 to n − 1:
    if s[R] in last and last[s[R]] ≥ L:
        L ← last[s[R]] + 1

    last[s[R]] = R

    max_len ← max(max_len, R − L + 1)

return max_len
```
### Dry Run
We need to find the length of the longest substring without repeating characters using a sliding window and a hashmap (`last`).

### 1. Given input

```
s = "abcabcbb"
```

| Index     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --------- | - | - | - | - | - | - | - | - |
| Character | a | b | c | a | b | c | b | b |

### 2. Explain the pseudocode line by line

Line 1: `L ← 0; last ← {}; max_len ← 0`

* `L = 0`: Left boundary of the current substring.

* `last = {}`: An empty hashmap that stores each character's most recent index.

* `max_len = 0`: Stores the longest valid substring length found so far.

Line 2: `for R ← 0 to n − 1:`

Move `R` from index `0` to index `7`. It represents the right boundary of the current substring.

Line 3: `if s[R] in last and last[s[R]] ≥ L:`

Check two conditions:

1. Has the current character appeared before?

2. Is its previous occurrence inside the current window (at or after `L`)?

If both are true, the current character is repeated in the window.

Line 4: `L ← last[s[R]] + 1`

Move `L` to one position after the character's previous occurrence.

For example, if `'a'` last appeared at index `0`, then `L = 0 + 1 = 1`.

Line 5: `last[s[R]] = R`

Update the hashmap with the current character's index.

For example, if the current character is `'a'` at index `3`, store `last['a'] = 3`.

Line 6: `max_len = max(max_len, R − L + 1)`

Calculate the current window length:

length=R−L+1\text{length}=R-L+1length=R−L+1

Keep whichever is larger: the previous `max_len` or the current window length.

Line 7: `return max_len`

Return the maximum length found.

### 3. Dry run step by step

#### Iteration 1: `R = 0`, character = `a`

* `'a'` is not in `last`, so no duplicate.

* Store `last = {a: 0}`.

* Length = `0 − 0 + 1 = 1`.

`max_len = 1`

Current window: `"a"`

#### Iteration 2: `R = 1`, character = `b`

* `'b'` is not in `last`.

* Store `last = {a: 0, b: 1}`.

* Length = `1 − 0 + 1 = 2`.

`max_len = 2`

Current window: `"ab"`

#### Iteration 3: `R = 2`, character = `c`

* `'c'` is not in `last`.

* Store `last = {a: 0, b: 1, c: 2}`.

* Length = `2 − 0 + 1 = 3`.

`max_len = 3`

Current window: `"abc"`

#### Iteration 4: `R = 3`, character = `a`

* `'a'` exists in `last`, with `last['a'] = 0`.

* Since `0 ≥ L` (`L = 0`), it is a duplicate in the current window.

* Update `L = 0 + 1 = 1`.

* Update `last['a'] = 3`.

* Length = `3 − 1 + 1 = 3`.

`max_len = 3`

Current window: `"bca"`

#### Iteration 5: `R = 4`, character = `b`

* `'b'` exists at index `1`.

* Since `1 ≥ L` (`L = 1`), it is a duplicate.

* Update `L = 1 + 1 = 2`.

* Update `last['b'] = 4`.

* Length = `4 − 2 + 1 = 3`.

`max_len = 3`

Current window: `"cab"`

#### Iteration 6: `R = 5`, character = `c`

* `'c'` exists at index `2`.

* Since `2 ≥ L` (`L = 2`), it is a duplicate.

* Update `L = 2 + 1 = 3`.

* Update `last['c'] = 5`.

* Length = `5 − 3 + 1 = 3`.

`max_len = 3`

Current window: `"abc"`

#### Iteration 7: `R = 6`, character = `b`

* `'b'` exists at index `4`.

* Since `4 ≥ L` (`L = 3`), it is a duplicate.

* Update `L = 4 + 1 = 5`.

* Update `last['b'] = 6`.

* Length = `6 − 5 + 1 = 2`.

`max_len = 3`

Current window: `"cb"`

#### Iteration 8: `R = 7`, character = `b`

* `'b'` exists at index `6`.

* Since `6 ≥ L` (`L = 5`), it is a duplicate.

* Update `L = 6 + 1 = 7`.

* Update `last['b'] = 7`.

* Length = `7 − 7 + 1 = 1`.

`max_len = 3`

Current window: `"b"`

### 4. Dry-run summary table

| `R` | `s[R]` | Updated `L` | Current window | Length | `max_len` |
| --- | ------ | ----------- | -------------- | ------ | --------- |
| 0   | a      | 0           | `a`            | 1      | 1         |
| 1   | b      | 0           | `ab`           | 2      | 2         |
| 2   | c      | 0           | `abc`          | 3      | 3         |
| 3   | a      | 1           | `bca`          | 3      | 3         |
| 4   | b      | 2           | `cab`          | 3      | 3         |
| 5   | c      | 3           | `abc`          | 3      | 3         |
| 6   | b      | 5           | `cb`           | 2      | 3         |
| 7   | b      | 7           | `b`            | 1      | 3         |

### ✅ Final Output

Longest substring without repeating characters

# `"abc"`

Returned length

# 3

Final answer: `3`

### 🧠 Key difference
Instead of removing characters one by one from a set, this method uses `last` to jump `L` directly past the previous occurrence of a repeated character. The time complexity is O(n)O(n)O(n) on average.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Restart on Duplicate** | **O(n²)** | **O(charset)** |
| **Better · Sliding Window + Set** | **O(n)** | **O(charset)** |
| **Optimized · Last-Seen Map** | **O(n)** | **O(charset)** |

---

# Longest Repeating Character Replacement

**LeetCode #424** · [LeetCode](https://leetcode.com/problems/longest-repeating-character-replacement/) · **Medium**

> **String · Variable-Size Sliding Window · `window length − maxFreq ≤ k`**

### Approaches

#### 1. Brute Force (Extend From Every Start)
Start from each index, maintain character frequencies and `maxFreq`, and extend the window while `window length − maxFreq ≤ k`.

**Pseudo Code:**

```text id="m2h4qk"
given s, k

best ← 0

for i ← 0 to n − 1:
    freq ← {}; maxFreq ← 0

    for j ← i to n − 1:
        count s[j]
        if len − maxFreq ≤ k:
            best ← max(best, len)
        else:
            break

return best
```

#### 2. Optimized (Variable Window)
Expand the window with `R` and maintain character frequencies. If `window length − maxFreq > k`, move `L` forward until the window becomes valid again. Track the maximum valid window length.

**Pseudo Code:**

```text id="7c4v1p"
given s, k

l ← 0; freq ← {}; maxFreq ← 0; best ← 0

for r ← 0 to n − 1:
    count s[r]
    maxFreq ← max(maxFreq, freq[s[r]])

    if (r − l + 1) − maxFreq > k:
        uncount s[l]
        l++

    best ← max(best, r − l + 1)

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Extend From Every Start** | **O(n²)** | **O(26)** |
| **Optimized · Variable Window** | **O(n)** | **O(26)** |

---

# Minimum Window Substring

**LeetCode #76** · [LeetCode](https://leetcode.com/problems/minimum-window-substring/) · **Hard**

> **String · Variable-Size Sliding Window · Frequency Map**

### Approaches

#### 1. Optimized (Expand + Contract Window)
Build a frequency map for `t`. Expand the window with `R` until it contains all required characters with the correct frequencies. Then move `L` forward while the window remains valid to find the **smallest valid window**.

**Pseudo Code:**

```text
build need from t; required ← distinct chars in t

have ← 0; window counts ← {}

left ← 0; best ← none

for right ← 0 to n − 1:
    add s[right] to window; if it now meets need: have++

    while have == required:          // window is valid

        if width < best: best ← [left, right]

        remove s[left]; if it drops below need: have--
        left++

return best window
```

**Key Idea:**
Track `need` (required frequencies) and `have` (how many distinct characters currently meet their required frequency). When `have == required`, the window is valid.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Expand + Contract Window** | **O(\|s\| + \|t\|)** | **O(\|t\|)** |

---

# Permutation in String

**LeetCode #567** · [LeetCode](https://leetcode.com/problems/permutation-in-string/) · **Medium**

> **String · Fixed-Size Sliding Window · Frequency Match**

### Approaches

#### 1. Optimized (Fixed-Size Window + Frequency Match)
The required window size is `|s1|`. Build the frequency map of `s1`, then maintain a sliding window of the same size in `s2`. If both frequency maps match, a permutation exists.

**Pseudo Code:**

```text id="n7q3kx"
given s1, s2; k ← |s1|

target ← char counts of s1

build first window s2[0..k − 1]

    count each char into window

if window counts == target: return true

for right ← k to |s2| − 1:

    remove s2[right − k]; add s2[right]

return false
```

**Key Idea:**
When the window slides, **remove the outgoing character and add the incoming character**, then check whether the character frequencies match.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Fixed-Size Window · Frequency Match** | **O(\|s1\| + \|s2\|)** | **O(1)** |

---

# Sliding Window Maximum

**LeetCode #239** · [LeetCode](https://leetcode.com/problems/sliding-window-maximum/) · **Hard**

> **Array · Fixed-Size Sliding Window · Monotonic Deque**

### Approaches

#### 1. Brute Force (Re-scan Every Window)
For every window of size `K`, scan all `K` elements and find the maximum.

**Pseudo Code:**

```text id="g2v6pm"
given nums, k

for i ← 0 to n − k:
    scan nums[i..i+k−1] for its max
    append max to answer

return answer
```

#### 2. Optimized (Monotonic Deque)
Maintain a deque of **indices** where values are in decreasing order. Remove indices outside the window from the front, and remove smaller values from the back. The **front always contains the current window maximum**.

**Pseudo Code:**

```text id="d5n8qa"
given nums, k

dq ← []                              // indices, nums decreasing

for i ← 0 to n − 1:

    while dq and nums[dq.back] ≤ nums[i]:  pop back

    push i

    if dq.front ≤ i − k:  pop front

    if i ≥ k − 1:  answer.append(nums[dq.front])

    // else: first window still filling

return answer
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Re-scan Every Window** | **O(n · K)** | **O(1)** |
| **Optimized · Monotonic Deque** | **O(n)** | **O(K)** |
