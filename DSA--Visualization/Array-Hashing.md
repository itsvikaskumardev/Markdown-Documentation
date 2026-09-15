# Two Sum

**LeetCode #1** · [LeetCode](https://leetcode.com/problems/two-sum/) · **Easy**

> **Unsorted · Find a pair adding to target**

### Approaches

#### 1. Brute Force
Try every pair using two loops.

**Pseudo Code:**

```text
given arr, target

for i ← 0 to n − 2:
    for j ← i + 1 to n − 1:
        if arr[i] + arr[j] == target:
            return (i, j)

return none
```

#### 2. Optimized (HashMap)
Use a **HashMap** to store values and their indices. For each number, check whether `target - current` already exists.

**Pseudo Code:**

```text
given arr, target
seen ← {}   // value → index

for i ← 0 to n − 1:
    need ← target − arr[i]

    if need in seen:
        return (seen[need], i)

    seen[arr[i]] ← i

return none
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(1)** |
| **Optimized · HashMap** | **O(n)** | **O(n)** |

---

# Contains Duplicate

**LeetCode #217** · [LeetCode](https://leetcode.com/problems/contains-duplicate/) · **Easy**

> **Array · Check if any value appears twice**

### Approaches

#### 1. Brute Force
Compare every element with every other element.

**Pseudo Code:**

```text
for i ← 0 to n − 2:
    for j ← i + 1 to n − 1:
        if arr[i] == arr[j]:
            return true

return false
```

#### 2. Optimized (Set)
Keep track of elements already seen. If an element is already in the Set, a duplicate exists.

**Pseudo Code:**

```text
seen ← {}

for x in arr:
    if x in seen:
        return true

    seen.add(x)

return false
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(1)** |
| **Optimized · Set** | **O(n)** | **O(n)** |

---

# Valid Anagram

**LeetCode #242** · [LeetCode](https://leetcode.com/problems/valid-anagram/) · **Easy**

> **String · Check if two strings have the same characters and frequencies**

### Approaches

#### 1. Brute Force (Sort)
Sort both strings and compare them.

**Pseudo Code:**

```text
// Is t an anagram of s?

return sorted(s) == sorted(t)
```

#### 2. Optimized (Count Map)
Count each character in `s`, then decrease the count for each character in `t`. If any count becomes negative, they are not anagrams.

**Pseudo Code:**

```text
if len(s) != len(t):
    return false

count ← {}

for c in s:
    count[c]++

for c in t:
    count[c]--

    if count[c] < 0:
        return false

return true
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort** | **O(n log n)** | **O(n)** |
| **Optimized · Count Map** | **O(n)** | **O(1)** |

---

# Group Anagrams

**LeetCode #49** · [LeetCode](https://leetcode.com/problems/group-anagrams/) · **Medium**

> **String · Group words with the same character signature**

### Approaches

#### 1. Brute Force (Pairwise)
Compare each word with every other word by sorting their characters.

**Pseudo Code:**

```text
given words

for i ← 0 to n − 1:
    if word i not yet grouped:
        start group with words[i]

        for j ← i + 1 to n − 1:
            if sorted(words[i]) == sorted(words[j]):
                add words[j]

return all groups
```

#### 2. Optimized (Signature Map)
Sort each word to create a **signature**. Use a HashMap where the signature is the key and anagram words are stored in the same group.

**Pseudo Code:**

```text
given words
groups ← {}   // signature → list of words

for i ← 0 to n − 1:
    sig ← sorted(words[i])
    groups[sig].append(words[i])

return groups.values()
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Pairwise** | **O(n² · k log k)** | **O(n · k)** |
| **Optimized · Signature Map** | **O(n · k log k)** | **O(n · k)** |

---

# Top K Frequent Elements

**LeetCode #347** · [LeetCode](https://leetcode.com/problems/top-k-frequent-elements/) · **Medium**

> **Array · Find the k most frequent elements**

### Approaches

#### 1. Count + Sort
Count the frequency of each value, then sort by frequency in descending order and take the first `k`.

**Pseudo Code:**

```text
given arr, k
count ← {}

for v in arr:
    count[v] ← count[v] + 1

entries ← sort count by frequency descending

result ← first k values of entries

return result
```

#### 2. Optimized (Bucket Sort)
Count frequencies and place values into buckets based on their frequency. Traverse buckets from highest to lowest frequency and collect `k` elements.

**Pseudo Code:**

```text
given arr, k
count ← {}

for v in arr:
    count[v] ← count[v] + 1

buckets[f] ← values with frequency f   // f from 1..n

result ← []

for f ← n down to 1:
    add buckets[f] to result until |result| = k

return result
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Count + Sort** | **O(n log n)** | **O(n)** |
| **Optimized · Bucket Sort** | **O(n)** | **O(n)** |

---

# Product of Array Except Self

**LeetCode #238** · [LeetCode](https://leetcode.com/problems/product-of-array-except-self/) · **Medium**

> **Array · Prefix × Suffix · No Division**

### Approaches

#### 1. Brute Force
For each index, multiply every element except the current one.

**Pseudo Code:**

```text
given arr
output ← new array[n]

for i ← 0 to n − 1:
    p ← 1

    for j ← 0 to n − 1 where j ≠ i:
        p ← p × arr[j]

    output[i] ← p

return output
```

#### 2. Optimized (Prefix × Suffix)
Use two passes:
* Left → right: Store the product of all elements before `i`.
* Right → left: Multiply by the product of all elements after `i`.

**Pseudo Code:**

```text
given arr
output ← new array[n]

prefix ← 1    // Pass 1: left → right

for i ← 0 to n − 1:
    output[i] ← prefix
    prefix ← prefix × arr[i]

suffix ← 1    // Pass 2: right → left

for i ← n − 1 down to 0:
    output[i] ← output[i] × suffix
    suffix ← suffix × arr[i]

return output
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(1)** |
| **Optimized · Prefix × Suffix** | **O(n)** | **O(1)** |

---

# Longest Consecutive Sequence

**LeetCode #128** · [LeetCode](https://leetcode.com/problems/longest-consecutive-sequence/) · **Medium**

> **Array · Find the longest consecutive sequence in O(n)**

### Approaches

#### 1. Brute Force (Sort)
Sort the array, then track the longest consecutive streak while ignoring duplicates.

**Pseudo Code:**

```text
given arr

sort arr ascending

streak ← 1
best ← 1

for i ← 1 to n − 1:
    if arr[i] == arr[i−1] + 1:
        streak++
    else if arr[i] != arr[i−1]:
        streak ← 1

    best ← max(best, streak)

return best
```

#### 2. Optimized (Set + Sequence Starts)
Put all numbers in a Set. Only start a sequence when `n − 1` is not in the Set, then keep checking `n + 1`, `n + 2`, etc.

**Pseudo Code:**

```text
set ← all numbers    // O(1) membership

best ← 0

for each n in set:
    if (n − 1) in set:
        continue    // not a sequence start

    length ← 1

    while (n + length) in set:
        length++

    best ← max(best, length)

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sort** | **O(n log n)** | **O(n)** |
| **Optimized · Set + Sequence Starts** | **O(n)** | **O(n)** |

---

# Encode and Decode Strings

**LeetCode #271** · [LeetCode](https://leetcode.com/problems/encode-and-decode-strings/) · **Medium**

> **String · Encode/decode a list of strings without losing any characters**

### Approaches

#### 1. Naive (Delimiter)
Join strings using a delimiter like `#`, but it breaks if a string itself contains `#`.

**Pseudo Code:**

```text
encode(words):
    return words.join("#")

decode(s):
    return s.split("#")

// FAILS: a word containing "#" is split apart
```

#### 2. Optimized (Length Prefix)
Store each string as `length + delimiter + string`. During decoding, read the length first, then extract exactly that many characters.

**Pseudo Code:**

```text
encode(words):
    s ← ""

    for w in words:
        s += len(w) + "#" + w

    return s

decode(s):
    res ← []
    ptr ← 0

    while ptr < len(s):
        j ← index of next "#" from ptr
        L ← int(s[ptr..j])

        res.push(s[j + 1 .. j + 1 + L])
        ptr ← j + 1 + L

    return res
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Naive · Delimiter** | **O(N)** | **O(N)** |
| **Optimized · Length Prefix** | **O(N)** | **O(N)** |
