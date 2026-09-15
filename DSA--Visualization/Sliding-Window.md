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
