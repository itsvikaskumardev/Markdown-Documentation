# Table of Contents

- [Two Sum](#two-sum)
- [Contains Duplicate](#contains-duplicate)
- [Valid Anagram](#valid-anagram)
- [Group Anagrams](#group-anagrams)
- [Top K Frequent Elements](#top-k-frequent-elements)
- [Product of Array Except Self](#product-of-array-except-self)
- [Longest Consecutive Sequence](#longest-consecutive-sequence)
- [Encode and Decode Strings](#encode-and-decode-strings)

---

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
### Dry Run
Sure. This is the **brute-force approach for Two Sum**. Let's dry-run your exact input.

```text
arr = [2,7,11,15]
target = 9
```

Pseudocode:

```text
for i ← 0 to n − 2:
    for j ← i + 1 to n − 1:
        if arr[i] + arr[j] == target:
            return (i, j)
return none
```

---

### 1. `for i ← 0 to n − 2`

`n = 4`, so:

```text
n - 2 = 2
```

Therefore `i` will be:

```text
i = 0
i = 1
i = 2
```

Why don't we go to `i = 3`?

Because we need another element after `i` to form a pair.

---

#### i = 0

```text
arr[0] = 2
```

Now the inner loop:

```text
for j ← i + 1 to n − 1
```

Since `i = 0`:

```text
j = 1 to 3
```

##### j = 1

```text
arr[0] + arr[1]
= 2 + 7
= 9
```

Check:

```text
9 == target
9 == 9
```

✅ True!

Therefore:

```text
return (i, j)
```

becomes:

```text
return (0,1)
```

And the algorithm **stops immediately**.

---

### Why don't we check the remaining elements?

Because we already found the answer.

We don't need to check:

```text
2 + 11
2 + 15
7 + 11
...
```

The `return` exits the entire algorithm.

---

### Complete dry run

| `i` | `j` | `arr[i]` | `arr[j]` |   Sum | Result  |
| --: | --: | -------: | -------: | ----: | ------- |
|   0 |   1 |        2 |        7 | **9** | ✅ Found |

So:

```text
arr = [2,7,11,15]
        ↑ ↑
        0 1
```

```text
2 + 7 = 9
```

### ✅ Final output

```text
(0, 1)
```

If the question expects an array instead of a tuple, the answer is usually:

```text
[0,1]
```

### 🧠 Why `j = i + 1`?

This is very important.

We don't want to compare an element with itself:

```text
arr[0] + arr[0]
```

And we don't want duplicate pairs:

```text
(0,1)
(1,0)
```

So:

```text
j = i + 1
```

ensures that we only check **each pair once**.

For example:

```text
i = 0 → j = 1,2,3
i = 1 → j = 2,3
i = 2 → j = 3
```

That's why the total number of comparisons is `O(n²)`.


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

### Dry Run
Sure. This is the **optimized Two Sum approach using a hash map**.

Input:

```text id="u2c7va"
arr = [3,8,2,5]
target = 10
```

Pseudocode:

```text id="x1j5qp"
seen ← {}   // value → index

for i ← 0 to n − 1:
    need ← target − arr[i]

    if need in seen:
        return (seen[need], i)

    seen[arr[i]] ← i

return none
```

The main idea is:

> For every number, calculate what number you **need** to reach the target, then check whether you've already seen that number.

---

### 1. `seen ← {}`

Create an empty hash map.

```text id="y3g6q8"
seen = {}
```

It will store:

```text
value → index
```

For example:

```text
3 → 0
8 → 1
```

---

### 2. `for i ← 0 to n − 1`

Our array has 4 elements:

```text id="j5q1nw"
Index:  0  1  2  3
Value:  3  8  2  5
```

So:

```text id="l8k4sp"
i = 0, 1, 2, 3
```

---

#### 🔹 i = 0

Current value:

```text id="4p0y6x"
arr[0] = 3
```

### Calculate `need`

```text id="z8r2kc"
need = target - arr[i]
     = 10 - 3
     = 7
```

So we're asking:

> "Have I already seen `7`?"

Check:

```text id="n6f1ra"
if 7 in seen
```

Currently:

```text id="2m7k9p"
seen = {}
```

❌ `7` is not there.

So execute:

```text id="k3v8ds"
seen[arr[i]] ← i
```

That means:

```text id="q4x1nm"
seen[3] = 0
```

Now:

```text id="f8z5cw"
seen = {
    3 → 0
}
```

---

#### 🔹 i = 1

Current:

```text id="w6p2qa"
arr[1] = 8
```

Calculate:

```text id="a9c3mv"
need = 10 - 8
     = 2
```

Ask:

> Have we already seen `2`?

```text id="k7v4ps"
2 in seen?
```

Currently:

```text id="e2n6xt"
seen = {3 → 0}
```

❌ No.

So store current value:

```text id="j1r8zb"
seen[8] = 1
```

Now:

```text id="h5q9kd"
seen = {
    3 → 0,
    8 → 1
}
```

---

#### 🔹 i = 2

Current:

```text id="u7m3px"
arr[2] = 2
```

Calculate:

```text id="v4k8sn"
need = 10 - 2
     = 8
```

Now ask:

> Have we already seen `8`?

```text id="b3n7qw"
8 in seen?
```

Yes! ✅

Our map contains:

```text id="c6z2yt"
8 → 1
```

Therefore:

```text id="r5x9mk"
return (seen[need], i)
```

Substitute:

```text id="g2p6vc"
return (seen[8], 2)
```

Since:

```text id="w9s4ja"
seen[8] = 1
```

we get:

```text id="3h7k2q"
return (1,2)
```

And the algorithm **stops here**.

---

### Why does `(1,2)` work?

Look at those indexes:

```text id="v1s8kx"
Index:  0  1  2  3
Value:  3  8  2  5
           ↑  ↑
           1  2
```

Values:

```text id="5q2m8v"
arr[1] + arr[2]
= 8 + 2
= 10
```

Exactly our target. ✅

---

### Complete dry run

| `i` | `arr[i]` | `need = 10-arr[i]` | `seen` before | Found? | Action         |
| --: | -------: | -----------------: | ------------- | ------ | -------------- |
|   0 |        3 |                  7 | `{}`          | ❌      | Store `3 → 0`  |
|   1 |        8 |                  2 | `{3→0}`       | ❌      | Store `8 → 1`  |
|   2 |        2 |                  8 | `{3→0, 8→1}`  | ✅      | Return `(1,2)` |

### ✅ Final output

```text id="x8m2qa"
(1,2)
```

or, if the problem expects an array:

```text id="v5k9rw"
[1,2]
```

### 🧠 The important trick

Instead of checking every pair like the brute-force approach:

```text
3 + 8
3 + 2
3 + 5
8 + 2
...
```

we ask:

```text
current = 2
target = 10

need = 10 - 2
     = 8
```

Then:

> **"Have I seen 8 before?"**

Yes → index `1`.

Therefore:

```text
8 + 2 = 10
```

That's why the hash-map approach takes **O(n) average time** instead of **O(n²)** brute force.


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

### Dry Run
Sure. This is a very simple way to check whether two strings are **anagrams**.

### Input

```text
s = "anagram"
t = "nagaram"
```

Code:

```text
// t anagram of s?
return sorted(s) == sorted(t)
```

Let's understand it step by step.

---

### 1. `sorted(s)`

We have:

```text
s = "anagram"
```

The letters are:

```text
a n a g r a m
```

`sorted(s)` puts the characters in alphabetical order:

```text
sorted("anagram")
```

Result:

```text
['a', 'a', 'a', 'g', 'm', 'n', 'r']
```

So:

```text
sorted(s) = ['a','a','a','g','m','n','r']
```

---

### 2. `sorted(t)`

Now:

```text
t = "nagaram"
```

Characters:

```text
n a g a r a m
```

Sort them:

```text
sorted("nagaram")
```

Result:

```text
['a', 'a', 'a', 'g', 'm', 'n', 'r']
```

So:

```text
sorted(t) = ['a','a','a','g','m','n','r']
```

---

### 3. Compare them

The code says:

```text
sorted(s) == sorted(t)
```

Substitute the values:

```text
['a','a','a','g','m','n','r']
==
['a','a','a','g','m','n','r']
```

They are exactly the same.

Therefore:

```text
True
```

### ✅ Final output

```text
True
```

### Why does this prove they are anagrams?

An **anagram** means both strings contain the **same characters with the same frequency**, just possibly in a different order.

Here:

```text
anagram → a a a g m n r
nagaram → a a a g m n r
```

Same characters ✅
Same frequency ✅
Different original order ✅

Therefore:

```text
s = "anagram"
t = "nagaram"

Output → True
```

### 🧠 Easy trick to remember

```text
sorted(s) == sorted(t)
```

means:

> **"If I arrange both strings in the same order, are they exactly equal?"**

If yes → **anagram** → `True`
If no → **not anagram** → `False`

For example:

```text
s = "rat"
t = "car"

sorted(s) = ['a','r','t']
sorted(t) = ['a','c','r']

False
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

### Dry Run
Sure. This is the **frequency-count / hash-map approach** to check whether two strings are anagrams.

### Input

```text
s = "anagram"
t = "nagaram"
```

Pseudocode:

```text
if len(s) != len(t): return false

count ← {}

for c in s:
    count[c]++

for c in t:
    count[c]--
    if count[c] < 0:
        return false

return true
```

---

### 1. Check lengths

```text
if len(s) != len(t):
    return false
```

Length of `s`:

```text
"anagram"
```

has **7 characters**.

Length of `t`:

```text
"nagaram"
```

also has **7 characters**.

So:

```text
len(s) != len(t)
7 != 7
```

❌ False.

Therefore, we **don't return false** and continue.

---

### 2. `count ← {}`

Create an empty hash map:

```text
count = {}
```

It will store:

```text
character → frequency
```

For example:

```text
a → 3
g → 1
```

---

### 3. `for c in s: count[c]++`

Now go through every character of:

```text
s = "anagram"
```

Characters:

```text
a n a g r a m
```

#### First `a`

```text
count[a]++
```

Since `a` doesn't exist yet, think of it as `0 + 1`:

```text
a → 1
```

---

#### `n`

```text
count[n]++
```

```text
a → 1
n → 1
```

---

#### Second `a`

```text
count[a]++
```

`a` was `1`, so:

```text
a → 2
```

Map:

```text
a → 2
n → 1
```

---

#### `g`

```text
g → 1
```

---

#### `r`

```text
r → 1
```

---

#### Third `a`

```text
a → 3
```

---

#### `m`

```text
m → 1
```

So after processing all of `s`:

```text
count = {
    a → 3,
    n → 1,
    g → 1,
    r → 1,
    m → 1
}
```

This means:

```text
a appears 3 times
n appears 1 time
g appears 1 time
r appears 1 time
m appears 1 time
```

---

### 4. `for c in t: count[c]--`

Now process:

```text
t = "nagaram"
```

Characters:

```text
n a g a r a m
```

We **decrease** the count for each character.

---

#### First `n`

Before:

```text
n → 1
```

Decrease:

```text
n → 0
```

---

#### First `a`

Before:

```text
a → 3
```

Decrease:

```text
a → 2
```

---

#### `g`

```text
g: 1 → 0
```

---

#### Second `a`

```text
a: 2 → 1
```

---

#### `r`

```text
r: 1 → 0
```

---

#### Third `a`

```text
a: 1 → 0
```

---

#### `m`

```text
m: 1 → 0
```

Final map:

```text
count = {
    a → 0,
    n → 0,
    g → 0,
    r → 0,
    m → 0
}
```

---

### 5. Check `count[c] < 0`

After every character in `t`, the code checks:

```text
if count[c] < 0:
    return false
```

But every count stayed at `0` or above.

So we **never return false**.

---

### 6. `return true`

We reach:

```text
return true
```

Therefore:

### ✅ Final Output

```text
true
```

### Complete dry run

| Character from `s` | Count after adding | Character from `t` | Count after subtracting |
| ------------------ | ------------------ | ------------------ | ----------------------- |
| `a`                | `a = 1`            | `n`                | `n = 0`                 |
| `n`                | `n = 1`            | `a`                | `a = 2`                 |
| `a`                | `a = 2`            | `g`                | `g = 0`                 |
| `g`                | `g = 1`            | `a`                | `a = 1`                 |
| `r`                | `r = 1`            | `r`                | `r = 0`                 |
| `a`                | `a = 3`            | `a`                | `a = 0`                 |
| `m`                | `m = 1`            | `m`                | `m = 0`                 |

At the end, all frequencies balance to `0`.

```text
"anagram"
   ↓ count
"a": 3, "n": 1, "g": 1, "r": 1, "m": 1

"nagaram"
   ↓ subtract
"a": 0, "n": 0, "g": 0, "r": 0, "m": 0
```

So:

```text
s = "anagram"
t = "nagaram"

Output = true ✅
```

**Core idea:** `s` **adds** character counts, `t` **subtracts** them. If `t` tries to use a character more times than `s` has, a count becomes negative → `false`.

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
