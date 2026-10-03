# Table of Contents

- [Two Sum](#two-sum)
- [Contains Duplicate](#contains-duplicate)
- [Valid Anagram](#valid-anagram)
- [Group Anagrams](#group-anagrams)
- [Top K Frequent Elements](#top-k-frequent-elements)
- [Product of Array Except Self](#product-of-array-except-self)
- [Longest Consecutive Sequence](#longest-consecutive-sequence)
- [Encode and Decode Strings](#encode-and-decode-strings)
- [Majority Element](#majority-element)

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
Sure. This is the **Group Anagrams** brute-force approach.

### Input

```text
words = ["eat","tea","tan","ate","nat","bat"]
```

Pseudocode:

```text
for i ← 0 to n − 1 (if word i not yet grouped):
    start group with words[i]

    for j ← i + 1 to n − 1:
        if sorted(words[i]) == sorted(words[j]):
            add words[j]

return all groups
```

The main idea is:

> Sort each word. If two words produce the same sorted characters, they are anagrams and go into the same group.

---

### Step 1: `i = 0`

Current word:

```text
words[0] = "eat"
```

Start a group:

```text
group = ["eat"]
```

Now compare `"eat"` with every word after it.

#### Compare `"eat"` and `"tea"`

```text
sorted("eat") = ['a','e','t']
sorted("tea") = ['a','e','t']
```

They are equal ✅

So add `"tea"`:

```text
group = ["eat","tea"]
```

---

#### Compare `"eat"` and `"tan"`

```text
sorted("eat") = ['a','e','t']
sorted("tan") = ['a','n','t']
```

❌ Different → don't add.

---

#### Compare `"eat"` and `"ate"`

```text
sorted("eat") = ['a','e','t']
sorted("ate") = ['a','e','t']
```

✅ Same.

```text
group = ["eat","tea","ate"]
```

---

#### Compare `"eat"` and `"nat"`

```text
sorted("eat") = ['a','e','t']
sorted("nat") = ['a','n','t']
```

❌ Different.

---

#### Compare `"eat"` and `"bat"`

```text
sorted("eat") = ['a','e','t']
sorted("bat") = ['a','b','t']
```

❌ Different.

So our first group is:

```text
["eat","tea","ate"]
```

---

### Step 2: `i = 1`

Normally we would reach `"tea"`.

But `"tea"` is **already grouped** with `"eat"`.

That's why the pseudocode says:

```text
if word i not yet grouped
```

So we skip `"tea"`.

---

### Step 3: `i = 2`

Current:

```text
words[2] = "tan"
```

Start a new group:

```text
group = ["tan"]
```

Now compare with words after it.

#### `"tan"` vs `"ate"`

```text
sorted("tan") = ['a','n','t']
sorted("ate") = ['a','e','t']
```

❌ Different.

#### `"tan"` vs `"nat"`

```text
sorted("tan") = ['a','n','t']
sorted("nat") = ['a','n','t']
```

✅ Same.

Add `"nat"`:

```text
group = ["tan","nat"]
```

#### `"tan"` vs `"bat"`

```text
sorted("tan") = ['a','n','t']
sorted("bat") = ['a','b','t']
```

❌ Different.

So second group:

```text
["tan","nat"]
```

---

### Step 4: `i = 3`

```text
words[3] = "ate"
```

Already grouped with:

```text
["eat","tea","ate"]
```

So skip it.

---

### Step 5: `i = 4`

```text
words[4] = "nat"
```

Already grouped with:

```text
["tan","nat"]
```

So skip it.

---

### Step 6: `i = 5`

Current:

```text
words[5] = "bat"
```

It hasn't been grouped yet.

Start:

```text
group = ["bat"]
```

There are no words after `"bat"`.

So this group is:

```text
["bat"]
```

---

### Final groups

We have:

```text
Group 1 = ["eat","tea","ate"]

Group 2 = ["tan","nat"]

Group 3 = ["bat"]
```

### ✅ Final output

```text
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

### Why are they grouped?

Because their sorted versions are the same:

```text
"eat" → "aet"
"tea" → "aet"
"ate" → "aet"
```

Therefore:

```text
["eat","tea","ate"]
```

And:

```text
"tan" → "ant"
"nat" → "ant"
```

Therefore:

```text
["tan","nat"]
```

While:

```text
"bat" → "abt"
```

has no matching word.

### 🧠 Core logic

```text
word
 ↓
sort characters
 ↓
same sorted result?
 ↓
YES → same anagram group
NO  → different group
```

For example:

```text
eat → aet
tea → aet    ← same → group together
ate → aet    ← same → group together

tan → ant
nat → ant    ← same → group together

bat → abt    ← unique
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
Sure. This is the **optimized Hash Map approach for Group Anagrams**. Instead of comparing every word with every other word, we create a **signature** using the sorted characters.

### Input

```text
words = ["eat","tea","tan","ate","nat","bat"]
```

Pseudocode:

```text
groups ← {}   // signature → list of words

for i ← 0 to n − 1:
    sig ← sorted(words[i])
    groups[sig].append(words[i])

return groups.values()
```

---

### 1. `groups ← {}`

Create an empty hash map.

```text
groups = {}
```

The comment tells us what it stores:

```text
signature → list of words
```

For example:

```text
"aet" → ["eat","tea","ate"]
```

Here `"aet"` is the **signature**.

---

### 2. Loop through every word

```text
for i ← 0 to n − 1:
```

There are 6 words, so:

```text
i = 0,1,2,3,4,5
```

---

#### 🔹 i = 0

```text
words[0] = "eat"
```

##### `sig ← sorted(words[i])`

Sort `"eat"`:

```text
sorted("eat") = "aet"
```

So:

```text
sig = "aet"
```

Now:

```text
groups[sig].append(words[i])
```

means:

> Go to the `"aet"` group and add `"eat"`.

Since `"aet"` doesn't exist yet, create it:

```text
groups = {
    "aet": ["eat"]
}
```

---

#### 🔹 i = 1

```text
words[1] = "tea"
```

Sort:

```text
sorted("tea") = "aet"
```

So:

```text
sig = "aet"
```

The `"aet"` group already exists.

Add `"tea"`:

```text
groups = {
    "aet": ["eat","tea"]
}
```

---

#### 🔹 i = 2

```text
words[2] = "tan"
```

Sort:

```text
sorted("tan") = "ant"
```

So:

```text
sig = "ant"
```

No `"ant"` group exists, so create it:

```text
groups = {
    "aet": ["eat","tea"],
    "ant": ["tan"]
}
```

---

#### 🔹 i = 3

```text
words[3] = "ate"
```

Sort:

```text
sorted("ate") = "aet"
```

So:

```text
sig = "aet"
```

Add `"ate"` to the existing `"aet"` group:

```text
groups = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan"]
}
```

---

#### 🔹 i = 4

```text
words[4] = "nat"
```

Sort:

```text
sorted("nat") = "ant"
```

So:

```text
sig = "ant"
```

Add `"nat"`:

```text
groups = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan","nat"]
}
```

---

#### 🔹 i = 5

```text
words[5] = "bat"
```

Sort:

```text
sorted("bat") = "abt"
```

So:

```text
sig = "abt"
```

No `"abt"` group exists, so create it:

```text
groups = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan","nat"],
    "abt": ["bat"]
}
```

---

### 3. `return groups.values()`

Now the map contains:

```text
"aet" → ["eat","tea","ate"]
"ant" → ["tan","nat"]
"abt" → ["bat"]
```

`groups.values()` means:

> Give me only the values, not the keys/signatures.

So we get:

### ✅ Final output

```text
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

---

### 🧠 Understand the whole algorithm

The important idea is:

```text
word
 ↓
sort it
 ↓
signature
 ↓
use signature as HashMap key
```

For your input:

```text
eat → aet ─┐
tea → aet ─┤
ate → aet ─┘  → ["eat","tea","ate"]

tan → ant ─┐
nat → ant ─┘  → ["tan","nat"]

bat → abt ────→ ["bat"]
```

So instead of comparing every word against every other word, we simply ask:

> **"What is the sorted signature of this word?"**

If two words have the same signature, they belong to the same anagram group.

### Final:

```text
groups = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan","nat"],
    "abt": ["bat"]
}
```

and

```text
groups.values()
```

gives:

```text
[["eat","tea","ate"],["tan","nat"],["bat"]]
```

**Time complexity:** approximately `O(n × k log k)`, where `n` = number of words and `k` = average word length.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Pairwise** | **O(n² · k log k)** | **O(n · k)** |
| **Optimized · Signature Map** | **O(n · k log k)** | **O(n · k)** |

---

# Top K Frequent Elements

**LeetCode #347** · [LeetCode](https://leetcode.com/problems/top-k-frequent-elements/) · **Medium**

// Soln: DSA -3 -> Heaps-> pg.no:141
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

### Dry Run
Sure. This is the **Top K Frequent Elements** approach using a frequency map.

### Input

```text
nums = [1,2,1,2,1,2,3,1,3,2]
k = 2
```


---

### 1. `count ← {}`

Create an empty map:

```text
count = {}
```

It will store:

```text
number → frequency
```

For example:

```text
1 → 4
2 → 5
3 → 2
```

---

### 2. Count each number

```text
for v in arr:
    count[v] ← count[v] + 1
```

We go through:

```text
[1,2,1,2,1,2,3,1,3,2]
```

#### `v = 1`

```text
count[1] = 1
```

#### `v = 2`

```text
count[2] = 1
```

#### `v = 1`

```text
count[1] = 2
```

#### `v = 2`

```text
count[2] = 2
```

#### `v = 1`

```text
count[1] = 3
```

#### `v = 2`

```text
count[2] = 3
```

#### `v = 3`

```text
count[3] = 1
```

#### `v = 1`

```text
count[1] = 4
```

#### `v = 3`

```text
count[3] = 2
```

#### `v = 2`

```text
count[2] = 4
```

So according to the sequence, we get:

```text
count = {
    1 → 4,
    2 → 4,
    3 → 2
}
```

⚠️ **Important:** `1` appears 4 times and `2` appears 4 times — not 5 times.

Let's verify:

```text
1 → positions 1,3,5,8 = 4 times
2 → positions 2,4,6,10 = 4 times
3 → positions 7,9 = 2 times
```

---

### 3. `entries ← sort count by frequency desc`

Now sort the numbers according to their frequency from **highest to lowest**.

Before sorting:

```text
1 → 4
2 → 4
3 → 2
```

After sorting:

```text
[(1,4), (2,4), (3,2)]
```

Since `1` and `2` have the same frequency, their order can depend on the sorting implementation.

The important thing is:

```text
1 and 2 → frequency 4
3       → frequency 2
```

---

### 4. `result ← first k values of entries`

We have:

```text
k = 2
```

So take the first **2 numbers**:

```text
result = [1,2]
```

---

### 5. `return result`

Therefore:

### ✅ Final output

```text
[1,2]
```

or potentially:

```text
[2,1]
```

depending on how ties are ordered.

Both are correct because **1 and 2 are the two most frequent elements**, each appearing 4 times.

### Complete dry run

| Number | Frequency |
| -----: | --------: |
|    `1` |       `4` |
|    `2` |       `4` |
|    `3` |       `2` |

`k = 2`, so:

```text
Top 2 = [1,2]
```

### 🧠 Main idea

```text
Array
  ↓
Count frequencies
  ↓
{1:4, 2:4, 3:2}
  ↓
Sort by frequency ↓
  ↓
Take first k
  ↓
[1,2]
```

**Output: `[1,2]`** (order between `1` and `2` is interchangeable because they are tied).


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

### Dry Run
Sure. This is the **Bucket Sort approach for Top K Frequent Elements**.

Input:

```text
nums = [1,1,1,2,2,3]
k = 2
```

```

---

### 1. `count ← {}`

Create an empty frequency map:

```text
count = {}
```

It will store:

```text
value → frequency
```

---

### 2. Count every value

```text
for v in arr:
    count[v] ← count[v] + 1
```

Go through:

```text
[1,1,1,2,2,3]
```

#### First `1`

```text
count[1] = 1
```

#### Second `1`

```text
count[1] = 2
```

#### Third `1`

```text
count[1] = 3
```

#### First `2`

```text
count[2] = 1
```

#### Second `2`

```text
count[2] = 2
```

#### `3`

```text
count[3] = 1
```

So:

```text
count = {
    1 → 3,
    2 → 2,
    3 → 1
}
```

---

### 3. Create buckets

```text
buckets[f] ← values with frequency f
```

Here `n = 6`, so conceptually we have buckets for frequencies `1` through `6`:

```text
frequency 1 → [3]
frequency 2 → [2]
frequency 3 → [1]
frequency 4 → []
frequency 5 → []
frequency 6 → []
```

Visualize it like:

```text
Bucket
  ↓
6 → []
5 → []
4 → []
3 → [1]
2 → [2]
1 → [3]
```

The important thing is:

```text
1 occurs 3 times → bucket[3] = [1]
2 occurs 2 times → bucket[2] = [2]
3 occurs 1 time  → bucket[1] = [3]
```

---

### 4. `result ← []`

Create an empty result:

```text
result = []
```

We need:

```text
k = 2
```

So we need **2 elements**.

---

### 5. Loop from highest frequency to lowest

```text
for f ← n down to 1:
```

Since `n = 6`, we check:

```text
f = 6
f = 5
f = 4
f = 3
f = 2
f = 1
```

We start from the **highest frequency** because we want the most frequent elements.

---

#### `f = 6`

```text
buckets[6] = []
```

Nothing to add.

```text
result = []
```

---

#### `f = 5`

```text
buckets[5] = []
```

Nothing.

```text
result = []
```

---

#### `f = 4`

```text
buckets[4] = []
```

Nothing.

```text
result = []
```

---

#### `f = 3`

```text
buckets[3] = [1]
```

Add `1`:

```text
result = [1]
```

Current size:

```text
|result| = 1
```

But we need:

```text
k = 2
```

So continue.

---

#### `f = 2`

```text
buckets[2] = [2]
```

Add `2`:

```text
result = [1,2]
```

Now:

```text
|result| = 2
```

And:

```text
k = 2
```

So we have enough elements.

Stop.

---

### 6. `return result`

Therefore:

```text
result = [1,2]
```

### ✅ Final Output

```text
[1,2]
```

---

### Complete dry run

| Frequency `f` | Bucket | Result     |
| ------------: | ------ | ---------- |
|             6 | `[]`   | `[]`       |
|             5 | `[]`   | `[]`       |
|             4 | `[]`   | `[]`       |
|             3 | `[1]`  | `[1]`      |
|             2 | `[2]`  | `[1,2]`    |
|          Stop | —      | Size = `k` |

### Why is `[1,2]` the answer?

Frequencies:

```text
1 → 3 times
2 → 2 times
3 → 1 time
```

Therefore the top 2 frequent elements are:

```text
1 → 3
2 → 2
```

So:

```text
✅ Output = [1,2]
```

### 🧠 The main difference from the previous approach

Instead of:

```text
count → sort frequencies → take k
```

Bucket approach does:

```text
count
  ↓
put values into frequency buckets
  ↓
start from highest frequency
  ↓
take k values
```

That's why we don't explicitly sort the frequencies.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Count + Sort** | **O(n log n)** | **O(n)** |
| **Optimized · Bucket Sort** | **O(n)** | **O(n)** |

---

# Product of Array Except Self

**LeetCode #238** · [LeetCode](https://leetcode.com/problems/product-of-array-except-self/) · **Medium**

> **Array · Prefix × Suffix · No Division**

Soln : DSA 02->Special Algorithms -> Pg.no:24
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
### Dry Run
Sure. This is the **brute-force approach for Product of Array Except Self**.

### Input

```text id="t7l9p3"
nums = [1,2,3,4]
```

---

### 1. `output ← new array[n]`

Our array has 4 elements:

```text id="8bq2wa"
n = 4
```

Create an output array of size 4:

```text id="m1z8kp"
output = [_, _, _, _]
```

`_` means we haven't calculated that position yet.

---

### 2. Outer loop

```text id="c5r7vx"
for i ← 0 to n − 1:
```

Since `n = 4`:

```text id="k9v2ds"
i = 0, 1, 2, 3
```

The important part is:

> For every `i`, calculate the product of **all elements except `arr[i]`**.

---

#### 🔹 i = 0

Current element:

```text id="4s2j7q"
arr[0] = 1
```

Initialize:

```text id="g8n4wc"
p = 1
```

Now:

```text id="b0x6vr"
for j = 0 to 3 where j ≠ 0
```

So we skip index `0`.

##### j = 1

```text id="v3p9la"
p = p × arr[1]
  = 1 × 2
  = 2
```

##### j = 2

```text id="n6c1fx"
p = 2 × 3
  = 6
```

##### j = 3

```text id="q7m4zs"
p = 6 × 4
  = 24
```

Now:

```text id="d1k8vy"
output[0] = 24
```

Output:

```text id="w3f6qp"
[24, _, _, _]
```

Because:

```text id="j5n9ca"
2 × 3 × 4 = 24
```

---

#### 🔹 i = 1

Current:

```text id="y2k7mx"
arr[1] = 2
```

Reset:

```text id="h8p3vw"
p = 1
```

Now skip index `1`.

##### j = 0

```text id="d6r1qs"
p = 1 × 1 = 1
```

##### j = 2

```text id="a9f4kc"
p = 1 × 3 = 3
```

##### j = 3

```text id="s7n2lx"
p = 3 × 4 = 12
```

Therefore:

```text id="c4m8zp"
output[1] = 12
```

Output:

```text id="w5q1rv"
[24,12,_,_]
```

Because:

```text id="g9x3mt"
1 × 3 × 4 = 12
```

---

#### 🔹 i = 2

Current:

```text id="e6v1bn"
arr[2] = 3
```

Reset:

```text id="q8k4ws"
p = 1
```

Skip index `2`.

##### j = 0

```text id="a1c7dz"
p = 1 × 1 = 1
```

##### j = 1

```text id="r5m9xk"
p = 1 × 2 = 2
```

##### j = 3

```text id="u3f6pq"
p = 2 × 4 = 8
```

Therefore:

```text id="k7n2vb"
output[2] = 8
```

Output:

```text id="q4w8mz"
[24,12,8,_]
```

Because:

```text id="x2c9la"
1 × 2 × 4 = 8
```

---

#### 🔹 i = 3

Current:

```text id="b8m4qy"
arr[3] = 4
```

Reset:

```text id="f2k7ns"
p = 1
```

Skip index `3`.

##### j = 0

```text id="e9v1rc"
p = 1 × 1 = 1
```

##### j = 1

```text id="j3x6kp"
p = 1 × 2 = 2
```

##### j = 2

```text id="s8q2wd"
p = 2 × 3 = 6
```

Therefore:

```text id="m5c9za"
output[3] = 6
```

Final output:

```text id="r7v2kx"
[24,12,8,6]
```

---

### Complete dry run

| `i` | Element excluded | Elements multiplied | `p` | `output[i]` |
| --: | ---------------: | ------------------- | --: | ----------: |
|   0 |              `1` | `2 × 3 × 4`         |  24 |          24 |
|   1 |              `2` | `1 × 3 × 4`         |  12 |          12 |
|   2 |              `3` | `1 × 2 × 4`         |   8 |           8 |
|   3 |              `4` | `1 × 2 × 3`         |   6 |           6 |

### ✅ Final output

```text
[24,12,8,6]
```

### 🧠 Easy way to understand it

For every position, simply ask:

```text
output[i] = product of everything EXCEPT nums[i]
```

So:

```text
nums = [1, 2, 3, 4]

index 0 → skip 1 → 2×3×4 = 24
index 1 → skip 2 → 1×3×4 = 12
index 2 → skip 3 → 1×2×4 = 8
index 3 → skip 4 → 1×2×3 = 6
```

Therefore:

```text
✅ [24, 12, 8, 6]
```

This brute-force solution takes **O(n²)** time because for every `i`, we loop through the entire array again.


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

### Dry Run
Sure. This is the **optimized Product of Array Except Self** approach using **prefix and suffix products**.

### Input

```text
nums = [-1, 1, 0, -3, 3]
```


---

### Part 1: Prefix — left → right

The idea is:

> `output[i]` first stores the product of everything **to the left** of `i`.

Initially:

```text
prefix = 1
output = [_, _, _, _, _]
```

Input:

```text
index:  0   1  2   3   4
nums:  -1   1  0  -3   3
```

---

#### i = 0

```text
output[0] = prefix
          = 1
```

Then update:

```text
prefix = prefix × arr[0]
       = 1 × (-1)
       = -1
```

Now:

```text
output = [1, _, _, _, _]
prefix = -1
```

---

#### i = 1

```text
output[1] = prefix
          = -1
```

Update:

```text
prefix = -1 × arr[1]
       = -1 × 1
       = -1
```

Now:

```text
output = [1, -1, _, _, _]
prefix = -1
```

---

#### i = 2

```text
output[2] = prefix
          = -1
```

Update:

```text
prefix = -1 × arr[2]
       = -1 × 0
       = 0
```

Now:

```text
output = [1, -1, -1, _, _]
prefix = 0
```

⚠️ This is important: after encountering `0`, the prefix becomes `0`.

---

#### i = 3

```text
output[3] = prefix
          = 0
```

Update:

```text
prefix = 0 × (-3)
       = 0
```

Now:

```text
output = [1, -1, -1, 0, _]
```

---

#### i = 4

```text
output[4] = prefix
          = 0
```

Update:

```text
prefix = 0 × 3
       = 0
```

So after the first pass:

```text
output = [1, -1, -1, 0, 0]
```

But this is **not the final answer yet**.

It currently contains only the **left-side products**.

---

### Part 2: Suffix — right → left

Now we calculate the product of everything **to the right**.

Initialize:

```text
suffix = 1
```

We go from index `4` down to `0`.

---

#### i = 4

Current:

```text
arr[4] = 3
```

First:

```text
output[4] = output[4] × suffix
          = 0 × 1
          = 0
```

Then update:

```text
suffix = suffix × arr[4]
       = 1 × 3
       = 3
```

Now:

```text
output = [1, -1, -1, 0, 0]
suffix = 3
```

---

#### i = 3

Current:

```text
arr[3] = -3
```

First:

```text
output[3] = output[3] × suffix
          = 0 × 3
          = 0
```

Then:

```text
suffix = 3 × (-3)
       = -9
```

Now:

```text
output = [1, -1, -1, 0, 0]
suffix = -9
```

---

#### i = 2

Current:

```text
arr[2] = 0
```

First:

```text
output[2] = output[2] × suffix
          = -1 × (-9)
          = 9
```

Then:

```text
suffix = -9 × 0
       = 0
```

Now:

```text
output = [1, -1, 9, 0, 0]
suffix = 0
```

This is an important step.

For index `2`, we exclude the `0` itself:

```text
(-1) × 1 × (-3) × 3
```

Calculate:

```text
-1 × 1 × -3 × 3 = 9
```

So `output[2] = 9`.

---

#### i = 1

Current:

```text
arr[1] = 1
```

```text
output[1] = output[1] × suffix
          = -1 × 0
          = 0
```

Then:

```text
suffix = 0 × 1
       = 0
```

Now:

```text
output = [1, 0, 9, 0, 0]
```

---

#### i = 0

Current:

```text
arr[0] = -1
```

```text
output[0] = output[0] × suffix
          = 1 × 0
          = 0
```

Then:

```text
suffix = 0 × (-1)
       = 0
```

Final:

```text
output = [0, 0, 9, 0, 0]
```

### ✅ Final Output

```text
[0, 0, 9, 0, 0]
```

### Verify manually

For every index, multiply everything except that index:

```text
[-1, 1, 0, -3, 3]
```

**Index 0:** exclude `-1`

```text
1 × 0 × -3 × 3 = 0
```

**Index 1:** exclude `1`

```text
-1 × 0 × -3 × 3 = 0
```

**Index 2:** exclude `0`

```text
-1 × 1 × -3 × 3 = 9
```

**Index 3:** exclude `-3`

```text
-1 × 1 × 0 × 3 = 0
```

**Index 4:** exclude `3`

```text
-1 × 1 × 0 × -3 = 0
```

Therefore:

```text
nums   = [-1, 1, 0, -3, 3]
output = [ 0, 0, 9,  0, 0]
```

### 🧠 The key concept

The algorithm splits the answer into two parts:

```text
answer[i] = LEFT PRODUCT × RIGHT PRODUCT
```

First pass calculates:

```text
LEFT PRODUCT
```

Second pass calculates:

```text
RIGHT PRODUCT
```

For index `2`:

```text
[-1, 1]  [0]  [-3, 3]
   ↓            ↓
  -1            -9

-1 × -9 = 9
```

That's how we get the only non-zero answer:

```text
✅ [0, 0, 9, 0, 0]
```

And unlike the previous brute-force solution, this takes **O(n) time** because we make only two passes through the array.


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

---
# Majority Element

**LeetCode #169** · [LeetCode](https://leetcode.com/problems/majority-element/) · **Easy**

> **Array · Boyer-Moore Voting · One candidate + one counter**

### Approaches

#### 1. Brute Force (Hash Map)
Count the frequency of each element and return the element whose count is greater than `n / 2`.

**Pseudo Code:**

```text
count ← {}

for x in arr: count[x] ← count[x] + 1

return the key whose count > n / 2
```
### Dry Run
Sure. This is the **Majority Element** approach using a frequency map.

### Input

```text
nums = [2,2,1,3,2,2,1,2,2]
```

Pseudocode:

```text
count ← {}

for x in arr:
    count[x] ← count[x] + 1

return the key whose count > n / 2
```

---

### 1. `count ← {}`

Create an empty map:

```text
count = {}
```

It will store:

```text
number → frequency
```

For example:

```text
2 → 6
1 → 2
3 → 1
```

---

### 2. `for x in arr`

Go through every element:

```text
[2,2,1,3,2,2,1,2,2]
```

Let's count them one by one.

#### First `2`

```text
count[2] = 1
```

#### Second `2`

```text
count[2] = 2
```

#### `1`

```text
count[1] = 1
```

#### `3`

```text
count[3] = 1
```

#### Third `2`

```text
count[2] = 3
```

#### Fourth `2`

```text
count[2] = 4
```

#### Second `1`

```text
count[1] = 2
```

#### Fifth `2`

```text
count[2] = 5
```

#### Sixth `2`

```text
count[2] = 6
```

So finally:

```text
count = {
    2 → 6,
    1 → 2,
    3 → 1
}
```

---

### 3. `n / 2`

There are 9 elements:

```text
n = 9
```

Calculate:

```text
n / 2 = 9 / 2 = 4.5
```

We need a number whose frequency is **greater than 4.5**.

Check our counts:

```text
2 → 6   ✅ 6 > 4.5
1 → 2   ❌
3 → 1   ❌
```

So the key is:

```text
2
```

---

### 4. Return the key

Therefore:

```text
return 2
```

### ✅ Final Output

```text
2
```

### Quick dry-run table

| Number | Count |
| -----: | ----: |
|    `2` | **6** |
|    `1` |     2 |
|    `3` |     1 |

Since:

```text
6 > 9/2
6 > 4.5
```

`2` is the **majority element**.

### 🧠 Important point

The condition is:

```text
count[x] > n / 2
```

Not:

```text
count[x] >= n / 2
```

For this input:

```text
n = 9
n/2 = 4.5
```

So an element must appear **at least 5 times** to be the majority.

Here `2` appears **6 times**.

```text
✅ Answer = 2
```


#### 2. Optimized (Boyer-Moore)
Maintain a candidate and counter; increase the counter for the same value, decrease it for a different value, and replace the candidate when the counter reaches `0`.

**Pseudo Code:**

```text
candidate ← none;  count ← 0

for x in arr:

    if count == 0: candidate ← x;  count ← 1

    else if x == candidate: count ← count + 1

    else: count ← count − 1

return candidate
```
### Dry Run
Sure. This is **Boyer-Moore Majority Vote Algorithm**. It finds a candidate for the majority element using `O(1)` extra space.

### Input

```text
nums = [2,2,1,3,2,2,1,2,2]
```

Pseudocode:

```text
candidate ← none
count ← 0

for x in arr:
    if count == 0:
        candidate ← x
        count ← 1
    else if x == candidate:
        count ← count + 1
    else:
        count ← count − 1

return candidate
```

---

### Initial state

```text
candidate = none
count = 0
```

Now process each number one by one.

---

#### 🔹 x = 2

Currently:

```text
count = 0
```

So:

```text
candidate ← 2
count ← 1
```

State:

```text
candidate = 2
count = 1
```

---

#### 🔹 x = 2

`count` is not 0.

Check:

```text
x == candidate
2 == 2
```

✅ True.

So:

```text
count = count + 1
      = 1 + 1
      = 2
```

State:

```text
candidate = 2
count = 2
```

---

#### 🔹 x = 1

Check:

```text
count == 0?
2 == 0 → ❌
```

Then:

```text
x == candidate?
1 == 2 → ❌
```

So:

```text
count = count - 1
      = 2 - 1
      = 1
```

State:

```text
candidate = 2
count = 1
```

Think of this as `1` canceling one occurrence of candidate `2`.

---

#### 🔹 x = 3

Again:

```text
3 == 2 → ❌
```

So:

```text
count = 1 - 1
      = 0
```

State:

```text
candidate = 2
count = 0
```

Now the current candidate has been completely cancelled.

---

#### 🔹 x = 2

Now:

```text
count == 0
```

So choose a new candidate:

```text
candidate = 2
count = 1
```

---

#### 🔹 x = 2

Check:

```text
2 == 2 → ✅
```

So:

```text
count = 1 + 1
      = 2
```

State:

```text
candidate = 2
count = 2
```

---

#### 🔹 x = 1

```text
1 == 2 → ❌
```

So:

```text
count = 2 - 1
      = 1
```

State:

```text
candidate = 2
count = 1
```

---

#### 🔹 x = 2

```text
2 == 2 → ✅
```

So:

```text
count = 1 + 1
      = 2
```

---

#### 🔹 x = 2

Again:

```text
2 == 2 → ✅
```

So:

```text
count = 2 + 1
      = 3
```

Final state:

```text
candidate = 2
count = 3
```

---

### Complete dry run

| `x` | Condition                    | Candidate | Count |
| --: | ---------------------------- | --------: | ----: |
|   2 | `count == 0` → new candidate |         2 |     1 |
|   2 | `x == candidate`             |         2 |     2 |
|   1 | different → decrease         |         2 |     1 |
|   3 | different → decrease         |         2 |     0 |
|   2 | `count == 0` → new candidate |         2 |     1 |
|   2 | `x == candidate`             |         2 |     2 |
|   1 | different → decrease         |         2 |     1 |
|   2 | `x == candidate`             |         2 |     2 |
|   2 | `x == candidate`             |         2 |     3 |

Therefore:

### ✅ Final output

```text
2
```

### 🧠 What is `count` actually doing?

It is **not the actual frequency of `2`**.

Actual frequency:

```text
2 appears 6 times
```

But final:

```text
count = 3
```

because every different number cancels one candidate occurrence.

The algorithm works because `2` is actually a majority:

```text
2 appears 6 times
n = 9

6 > 9/2
6 > 4.5
```

So the final candidate is:

```text
✅ 2
```

**Important:** This pseudocode assumes that a majority element is guaranteed to exist. If the problem does **not** guarantee that, you should do a second pass to verify that the candidate actually occurs more than `n/2` times.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n)** | **O(n)** |
| **Optimized** | **O(n)** | **O(1)** |
