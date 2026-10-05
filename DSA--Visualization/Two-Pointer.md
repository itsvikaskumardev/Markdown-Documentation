# Table of Contents

- [Two Sum II](#two-sum-ii)
- [Valid Palindrome](#valid-palindrome)
- [3Sum](#3sum)
- [Remove Duplicates from Sorted Array](#remove-duplicates-from-sorted-array)
- [Merge Sorted Array](#merge-sorted-array)
- [Move Zeroes](#move-zeroes)
- [Sort Colors](#sort-colors)
- [Rotate Array by K Places](#rotate-array-by-k-places)
- [4Sum](#4sum)

---

# Two Sum II

**LeetCode #167** · [LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) · **Medium**

> **Sorted Array · Find two numbers that add up to target**

### Approaches

#### 1. Brute Force
Use two loops to check every possible pair. Since the answer is **1-based**, return `i + 1` and `j + 1`.

**Pseudo Code:**

```text
given sorted arr, target

for i ← 0 to n − 2:
    fix arr[i]

    for j ← i + 1 to n − 1:
        if arr[i] + arr[j] == target:
            return (i + 1, j + 1)

return none
```

#### 2. Optimized (Two Pointers)
Use `left` at the beginning and `right` at the end.

**Pseudo Code:**

```text
given sorted arr, target

left ← 0
right ← n − 1

while left < right:
    sum ← arr[left] + arr[right]

    if sum == target:
        return (left + 1, right + 1)

    else if sum < target:
        left++

    else:
        right--
```

* If `arr[left] + arr[right] == target` → return indices.
* If `sum < target` → move `left` forward.
* If `sum > target` → move `right` backward.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(1)** |
| **Optimized · Two Pointers** | **O(n)** | **O(1)** |

---

# Valid Palindrome

**LeetCode #125** · [LeetCode](https://leetcode.com/problems/valid-palindrome/) · **Easy**

> **String · Palindrome check · Ignore non-alphanumeric characters and case**

### Approaches

#### 1. Brute Force (Clean + Reverse)
Create a cleaned string containing only lowercase alphanumeric characters, then compare it with its reverse.

**Pseudo Code:**

```text
clean ← lowercase alphanumeric characters of s

return clean == reverse(clean)
```

### Dry Run
Sure. This is checking whether a string is a **palindrome** after removing spaces/special characters and converting uppercase letters to lowercase.

### Input

```text
s = "race a car"
```

Pseudocode:

```text
clean ← lowercased alphanumerics of s
return clean == reverse(clean)
```

---

### 1. `clean ← lowercased alphanumerics of s`

This means:

> Take only letters (`a-z`) and numbers (`0-9`), convert letters to lowercase, and remove spaces/special characters.

Original:

```text
"race a car"
```

Characters:

```text
r a c e   a   c a r
```

There are spaces, so remove them:

```text
"raceacar"
```

All characters are already lowercase.

Therefore:

```text
clean = "raceacar"
```

---

### 2. `reverse(clean)`

Reverse:

```text
clean = "raceacar"
```

From right to left:

```text
r a c a e c a r
```

So:

```text
reverse(clean) = "racaecar"
```

Let's carefully compare:

```text
clean:
r a c e a c a r
```

Reverse:

```text
r a c a e c a r
```

Therefore:

```text
"raceacar" != "racaecar"
```

---

### 3. `return clean == reverse(clean)`

We compare:

```text
"raceacar" == "racaecar"
```

❌ False.

### ✅ Final Output

```text
false
```

### Quick visualization

```text
race a car
    ↓
remove space
    ↓
raceacar
    ↓
reverse
    ↓
racaecar
```

They are **not the same**, so `"race a car"` is **not a palindrome**.

```text
Output = false
```


#### 2. Optimized (Two Pointers)
Use `left` and `right` pointers. Skip non-alphanumeric characters, compare characters ignoring case, then move both pointers toward the center.

**Pseudo Code:**

```text
l ← 0
r ← n − 1

while l < r:
    if not alnum(s[l]):
        l++
        continue

    if not alnum(s[r]):
        r--
        continue

    if lower(s[l]) != lower(s[r]):
        return false

    l++
    r--

return true
```

### Dry Run
Sure. This is the **two-pointer approach** for checking whether a string is a palindrome.

### Input

```text id="0rj8hx"
s = "A man, a plan, a canal: Panama"
```

Pseudocode:

```text id="p9v7ku"
l ← 0
r ← n − 1

while l < r:
    if not alnum(s[l]): l++; continue
    if not alnum(s[r]): r−−; continue
    if lower(s[l]) != lower(s[r]): return false
    l++; r−−

return true
```

---

### 1. `l ← 0`

`l` is the **left pointer**.

It starts at the first character:

```text id="8o9y5k"
A
↑
l = 0
```

---

### 2. `r ← n − 1`

`r` is the **right pointer**.

It starts at the last character:

```text id="0w2c7f"
A man, a plan, a canal: Panama
                              ↑
                              r
```

So:

```text id="p9t6wq"
l = 0
r = last index
```

---

### Now enter the `while` loop

```text id="gq3x4j"
while l < r
```

We continue while the two pointers haven't crossed.

---

#### 🔹 Comparison 1

Left:

```text id="9t4q2x"
s[l] = 'A'
```

Right:

```text id="j1m8zv"
s[r] = 'a'
```

Both are alphanumeric, so we don't skip either.

Compare lowercase:

```text id="e7v5n3"
lower('A') = 'a'
lower('a') = 'a'
```

They match ✅

Move both pointers:

```text id="x4p8kd"
l++
r--
```

---

#### 🔹 Comparison 2

Now left reaches:

```text id="8m2q7c"
'm'
```

Right reaches:

```text id="n4v6sy"
'm'
```

Compare:

```text id="r8k1wp"
lower('m') == lower('m')
```

✅ Match.

Move:

```text id="s5x9qa"
l++
r--
```

---

#### 🔹 Comparison 3

Left:

```text id="k3f7zm"
'a'
```

Right:

```text id="u6n2pc"
'a'
```

Match ✅

Move both.

---

#### 🔹 Comparison 4

Left:

```text id="t8q4yb"
'n'
```

Right:

```text id="n9w3ks"
'n'
```

Match ✅

Move both.

---

#### 🔹 Comparison 5

Left reaches:

```text id="h2m7vx"
','
```

`,` is **not alphanumeric**.

So:

```text id="z5c1pr"
if not alnum(s[l]):
    l++
    continue
```

We move `l` forward and **skip the comma**.

---

#### 🔹 Continue

The left pointer encounters spaces and punctuation such as:

```text id="0y5z8w"
' '
','
' '
```

These are not alphanumeric, so the algorithm simply skips them.

Similarly, if the right pointer encounters punctuation:

```text id="4j7n2a"
':'
' '
```

it moves `r` backward.

The algorithm effectively ignores:

```text id="e7n0qp"
spaces
commas
colon
```

and compares only letters/numbers.

---

#### What characters are actually compared?

The original string:

```text id="q4f7yc"
A man, a plan, a canal: Panama
```

After ignoring spaces and punctuation and converting to lowercase:

```text id="n5m8ws"
amanaplanacanalpanama
```

Now look at the two ends:

```text id="6k2p4v"
a m a n a p l a n a c a n a l p a n a m a
↑                                     ↑
a                                     a
```

They match.

Continue inward:

```text id="7r3x8c"
a == a
m == m
a == a
n == n
a == a
p == p
l == l
...
```

Every corresponding character matches.

Eventually:

```text id="q0d4sm"
l >= r
```

The loop ends.

---

### 3. `return true`

Since we never found a mismatch:

```text id="w8n2ka"
return true
```

### ✅ Final Output

```text id="j3x6qp"
true
```

### 🧠 Main idea

The algorithm **doesn't actually create a cleaned string**.

It uses two pointers:

```text id="v7k2ms"
LEFT →  A man, a plan, a canal: Panama  ← RIGHT
          ↓                         ↓
      skip symbols              skip symbols
          ↓                         ↓
       compare letters
```

It ignores:

* spaces
* commas
* colon
* other non-alphanumeric characters

and compares letters **case-insensitively**.

Therefore:

```text id="7y1n4q"
"A man, a plan, a canal: Panama"

                ↓

"amanaplanacanalpanama"

                ↓

Palindrome ✅

Output = true
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Clean + Reverse** | **O(n)** | **O(n)** |
| **Optimized · Two Pointers** | **O(n)** | **O(1)** |

---

# 3Sum

**LeetCode #15** · [LeetCode](https://leetcode.com/problems/3sum/) · **Medium**

> **Array · Find all unique triplets that sum to zero**

### Approaches

#### 1. Brute Force
Use three nested loops to check every possible triplet. Use a **Set** to remove duplicate triplets.

**Pseudo Code:**

```text id="6x1g2p"
given arr

for i ← 0 to n − 3:
    for j ← i + 1 to n − 2:
        for k ← j + 1 to n − 1:
            if arr[i] + arr[j] + arr[k] == 0:
                record (arr[i], arr[j], arr[k])

dedupe results    // use a Set → O(n) extra space
```
### Dry Run
Sure. This is the **brute-force approach for 3Sum**. It checks every possible combination of **3 different indices** and records the ones whose sum is `0`.

### Input

```text
nums = [-1, 0, 1, 2, -1, -4]
```

Pseudocode:

```text
for i ← 0 to n − 3:
    for j ← i + 1 to n − 2:
        for k ← j + 1 to n − 1:
            if arr[i] + arr[j] + arr[k] == 0:
                record (i, j, k)

dedupe results
```

Here:

```text
n = 6
```

---

### 1. Outer loop

```text
for i ← 0 to n − 3
```

Since:

```text
n − 3 = 3
```

So:

```text
i = 0, 1, 2, 3
```

`i` chooses the **first element**.

---

### 2. `i = 0`

```text
arr[0] = -1
```

Now `j` starts from `i + 1`:

```text
j = 1 to 4
```

#### j = 1

```text
arr[1] = 0
```

Now `k` starts from `j + 1 = 2`.

##### k = 2

```text
arr[0] + arr[1] + arr[2]

= -1 + 0 + 1
= 0
```

✅ Record:

```text
(-1, 0, 1)
```

---

##### k = 3

```text
-1 + 0 + 2 = 1
```

❌ Not 0.

---

##### k = 4

```text
-1 + 0 + (-1) = -2
```

❌

---

##### k = 5

```text
-1 + 0 + (-4) = -5
```

❌

---

#### j = 2

```text
arr[2] = 1
```

Now `k = 3,4,5`.

##### k = 3

```text
-1 + 1 + 2 = 2
```

❌

##### k = 4

```text
-1 + 1 + (-1) = -1
```

❌

##### k = 5

```text
-1 + 1 + (-4) = -4
```

❌

---

#### j = 3

```text
arr[3] = 2
```

##### k = 4

```text
-1 + 2 + (-1) = 0
```

✅ Record:

```text
(-1, 2, -1)
```

This corresponds to the values:

```text
[-1, -1, 2]
```

##### k = 5

```text
-1 + 2 + (-4) = -3
```

❌

---

#### j = 4

```text
arr[4] = -1
```

##### k = 5

```text
-1 + (-1) + (-4) = -6
```

❌

---

### 3. `i = 1`

```text
arr[1] = 0
```

Now `j = 2`.

#### j = 2

```text
arr[2] = 1
```

##### k = 3

```text
0 + 1 + 2 = 3
```

❌

##### k = 4

```text
0 + 1 + (-1) = 0
```

✅ Record:

```text
[0, 1, -1]
```

This is the same combination as:

```text
[-1,0,1]
```

but in a different index order.

##### k = 5

```text
0 + 1 + (-4) = -3
```

❌

---

#### j = 3

```text
0 + 2 + (-1) = 1
```

❌

```text
0 + 2 + (-4) = -2
```

❌

---

#### j = 4

```text
0 + (-1) + (-4) = -5
```

❌

---

### 4. `i = 2`

```text
arr[2] = 1
```

Possible combinations:

```text
1 + 2 + (-1) = 2
1 + 2 + (-4) = -1
1 + (-1) + (-4) = -4
```

None are `0`.

So no new result.

---

### 5. `i = 3`

```text
arr[3] = 2
```

Only possible combination:

```text
2 + (-1) + (-4) = -3
```

❌ No result.

---

### 6. Results before deduplication

We found:

```text
[-1, 0, 1]
[-1, 2, -1]
[0, 1, -1]
```

But notice:

```text
[-1, 0, 1]
```

and

```text
[0, 1, -1]
```

contain the **same three values**.

They are duplicates, just discovered through different indices/order.

---

### 7. `dedupe results`

Remove duplicate triplets.

Normalize/sort each triplet:

```text
[-1, 0, 1] → [-1, 0, 1]

[-1, 2, -1] → [-1, -1, 2]

[0, 1, -1] → [-1, 0, 1]
```

Now remove the duplicate:

```text
[-1, 0, 1]
```

appears twice, so keep only one.

---

### ✅ Final Output

```text
[
    [-1, -1, 2],
    [-1, 0, 1]
]
```

The order of the two triplets may vary.

### Verify

First triplet:

```text
-1 + -1 + 2 = 0
```

Second:

```text
-1 + 0 + 1 = 0
```

So the final answer is:

```text
✅ [[-1,-1,2],[-1,0,1]]
```

### 🧠 What the three loops are doing

```text
i → choose first element
j → choose second element
k → choose third element
```

For example:

```text
i = 0 → -1
j = 3 → 2
k = 4 → -1

-1 + 2 + -1 = 0
```

That's why this brute-force approach checks **every possible triplet**.


#### 2. Optimized (Sort + Two Pointers)
Sort the array first. Fix one element, then use `left` and `right` pointers for the remaining two elements. Skip duplicates to ensure unique triplets.

**Pseudo Code:**

```text id="q8xqv7"
sort arr

for i ← 0 to n − 3:
    if i > 0 and arr[i] == arr[i − 1]:
        skip

    left ← i + 1
    right ← n − 1

    while left < right:
        sum ← arr[i] + arr[left] + arr[right]

        if sum == 0:
            record (arr[i], arr[left], arr[right])
            left++
            right--

            skip duplicate values for left and right

        else if sum < 0:
            left++

        else:
            right--
```
### Dry Run
Sure. This is the **optimized Two-Pointer approach for 3Sum**.

### Input

```text
arr = [-4, -1, -1, 0, 1, 2]
```

The array is already sorted.

```text
index:  0   1   2   3  4  5
arr:   -4  -1  -1   0  1  2
```

Pseudocode:

```text
sort arr

for i ← 0 to n − 3:
    if i > 0 and arr[i] == arr[i − 1]: skip
    left ← i + 1
    right ← n − 1

    while left < right:
        sum = arr[i] + arr[left] + arr[right]

        if sum == 0 → record; advance; skip dups
        else if sum < 0 → left++
        else             → right--
```

---

### 1. `sort arr`

Sorting puts the numbers in increasing order.

Here it is already:

```text
[-4, -1, -1, 0, 1, 2]
```

Sorting is important because it allows us to use the **two-pointer technique**.

---

### 2. `for i ← 0 to n − 3`

There are 6 elements:

```text
n = 6
```

So:

```text
i = 0, 1, 2, 3
```

`i` represents the **first number** of our triplet.

---

#### 🔹 i = 0

```text
arr[i] = -4
```

No duplicate check is needed because `i = 0`.

Set:

```text
left = 1
right = 5
```

So:

```text
        i   L           R
        ↓   ↓           ↓
arr = [-4, -1, -1, 0, 1, 2]
```

---

##### While `left < right`

##### First calculation

```text
sum = -4 + (-1) + 2
    = -3
```

Since:

```text
sum < 0
```

we need a **larger sum**.

Because the array is sorted, move `left` right:

```text
left++
```

Now:

```text
left = 2
right = 5
```

---

##### Second calculation

```text
sum = -4 + (-1) + 2
    = -3
```

Still negative.

Move `left`:

```text
left = 3
```

---

##### Third calculation

```text
sum = -4 + 0 + 2
    = -2
```

Still negative.

```text
left = 4
```

---

##### Fourth calculation

```text
sum = -4 + 1 + 2
    = -1
```

Still negative.

```text
left = 5
```

Now:

```text
left == right
```

Stop.

No triplet with `-4` gives sum `0`.

---

#### 🔹 i = 1

Now:

```text
arr[1] = -1
```

Set:

```text
left = 2
right = 5
```

Visual:

```text
             i   L           R
             ↓   ↓           ↓
arr = [-4, -1, -1, 0, 1, 2]
```

---

##### First calculation

```text
sum = -1 + (-1) + 2
    = 0
```

✅ Found a triplet!

Record:

```text
[-1, -1, 2]
```

So:

```text
result = [[-1,-1,2]]
```

Now:

```text
advance left and right
```

Therefore:

```text
left = 3
right = 4
```

There are no duplicate values immediately around these pointers, so continue.

---

##### Second calculation

```text
sum = -1 + 0 + 1
    = 0
```

✅ Found another triplet.

Record:

```text
[-1, 0, 1]
```

Now:

```text
result = [
    [-1,-1,2],
    [-1,0,1]
]
```

Advance:

```text
left = 4
right = 3
```

Now:

```text
left > right
```

Stop this iteration.

---

#### 🔹 i = 2

Now:

```text
arr[2] = -1
```

Check:

```text
if i > 0 and arr[i] == arr[i-1]
```

We have:

```text
arr[2] = -1
arr[1] = -1
```

They are equal.

Therefore:

```text
skip
```

We skip this `i`.

##### Why?

Because we already processed `-1` at `i = 1`.

Processing the second `-1` would produce duplicate triplets.

---

#### 🔹 i = 3

Now:

```text
arr[3] = 0
```

Set:

```text
left = 4
right = 5
```

Calculate:

```text
sum = 0 + 1 + 2
    = 3
```

Since:

```text
sum > 0
```

we need a **smaller sum**.

So move `right` left:

```text
right--
```

Now:

```text
right = 4
```

Therefore:

```text
left == right
```

Stop.

No triplet starting with `0` gives sum `0`.

---

### Final result

We found only two unique triplets:

```text
[-1, -1, 2]
[-1, 0, 1]
```

### ✅ Output

```text
[[-1,-1,2],[-1,0,1]]
```

---

### 🧠 Why does `left++` or `right--` work?

This is the important part of the optimized approach.

Because the array is sorted:

```text
[-4, -1, -1, 0, 1, 2]
```

If:

```text
sum < 0
```

the sum is too small.

So increase `left` to get a larger number:

```text
left++
```

If:

```text
sum > 0
```

the sum is too large.

So decrease `right` to get a smaller number:

```text
right--
```

If:

```text
sum == 0
```

we found our triplet:

```text
record
left++
right--
```

For this input:

```text
-1 + -1 + 2 = 0
-1 +  0 + 1 = 0
```

Therefore:

```text
✅ [[-1,-1,2],[-1,0,1]]
```

The optimized solution takes **O(n²)** time after sorting, compared with the previous brute-force **O(n³)** approach.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n³)** | **O(n)** |
| **Optimized · Sort + Two Pointers** | **O(n²)** | **O(1)** |

---

# Remove Duplicates from Sorted Array

**LeetCode #26** · [LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) · **Easy**

> **Sorted Array · Two Pointers · Remove duplicates in place**

### Approaches

#### 1. Brute Force (Second Array)
Create a new array and add an element only when it is different from the last unique element.

**Pseudo Code:**

```text id="v4rj2m"
unique ← []

for x in arr:
    if unique is empty or unique.last ≠ x:
        unique.append(x)

return unique.length
```
### Dry Run
Sure. This is the **Remove Duplicates from Sorted Array** approach.

### Input

```text
arr = [0,0,1,1,1,2,2,3,3,4]
```

Pseudocode:

```text
unique ← []

for x in arr:
    if unique is empty or unique.last ≠ x:
        unique.append(x)

return unique.length
```

The important idea is: **because the array is sorted, duplicate values are next to each other.** We only add `x` when it is different from the last value we added.

---

### 1. `unique ← []`

Create an empty array:

```text
unique = []
```

---

### 2. First `x = 0`

`unique` is empty, so add `0`.

```text
unique = [0]
```

---

### 3. Second `x = 0`

Check:

```text
unique.last = 0
x = 0
```

They are equal ❌, so don't add.

```text
unique = [0]
```

---

### 4. `x = 1`

Check:

```text
unique.last = 0
x = 1
```

Different ✅, so add.

```text
unique = [0,1]
```

---

### 5. `x = 1`

```text
unique.last = 1
x = 1
```

Same ❌ → don't add.

```text
unique = [0,1]
```

---

### 6. `x = 1`

Again:

```text
unique.last = 1
x = 1
```

Same ❌.

```text
unique = [0,1]
```

---

### 7. `x = 2`

```text
unique.last = 1
x = 2
```

Different ✅.

```text
unique = [0,1,2]
```

---

### 8. `x = 2`

Same as last value ❌:

```text
unique = [0,1,2]
```

---

### 9. `x = 3`

Different from `2` ✅:

```text
unique = [0,1,2,3]
```

---

### 10. `x = 3`

Same as last value ❌:

```text
unique = [0,1,2,3]
```

---

### 11. `x = 4`

Different from `3` ✅:

```text
unique = [0,1,2,3,4]
```

---

### Complete dry run

| `x` | `unique` after processing |
| --: | ------------------------- |
|   0 | `[0]`                     |
|   0 | `[0]`                     |
|   1 | `[0,1]`                   |
|   1 | `[0,1]`                   |
|   1 | `[0,1]`                   |
|   2 | `[0,1,2]`                 |
|   2 | `[0,1,2]`                 |
|   3 | `[0,1,2,3]`               |
|   3 | `[0,1,2,3]`               |
|   4 | `[0,1,2,3,4]`             |

So finally:

```text
unique = [0,1,2,3,4]
```

Then:

```text
return unique.length
```

There are **5** unique elements.

### ✅ Final Output

```text
5
```

The unique array is:

```text
[0,1,2,3,4]
```

### 🧠 Remember

The condition:

```text
unique.last ≠ x
```

means:

> **"Is the current number different from the last unique number?"**

If yes → add it.
If no → skip it.

So:

```text
[0,0,1,1,1,2,2,3,3,4]
 ↓
[0,1,2,3,4]
 ↓
length = 5
```

**Answer = `5`**.


#### 2. Optimized (Write Index)
Use a **write index** to place each new unique value in the next position. Since the array is sorted, duplicates are next to each other.

**Pseudo Code:**

```text id="w8k3qp"
k ← 1

for i ← 1 to n − 1:
    if arr[i] ≠ arr[k − 1]:
        arr[k] ← arr[i]
        k ← k + 1

return k
```
### Dry Run
This pseudocode is used to **remove duplicates from a sorted array in-place**.

### Pseudocode

```text
k ← 1
for i ← 1 to n − 1:
    if arr[i] ≠ arr[k − 1]:
        arr[k] ← arr[i]
        k ← k + 1
return k
```

Input:

```text
arr = [1,1,2,2,2,3,4,4]
```

`n = 8`

### Line-by-line explanation

### 1. `k ← 1`

`k` tells us the position where the **next unique element** should be placed.

Initially:

```text
k = 1
```

The first element `arr[0] = 1` is already unique, so we keep it.

---

### 2. `for i ← 1 to n − 1`

We start checking from index `1` because index `0` is already considered.

```text
i = 1, 2, 3, 4, 5, 6, 7
```

---

### 3. `if arr[i] ≠ arr[k − 1]`

We compare the current element with the **last unique element**.

If they are different, it means we found a new unique element.

---

#### Dry Run Table

| i | arr[i] |  k | arr[k-1] | Comparison | Action            |
| - | -----: | -: | -------: | ---------- | ----------------- |
| 1 |      1 |  1 |        1 | 1 ≠ 1 ❌    | Skip              |
| 2 |      2 |  1 |        1 | 2 ≠ 1 ✅    | `arr[1]=2`, `k=2` |
| 3 |      2 |  2 |        2 | 2 ≠ 2 ❌    | Skip              |
| 4 |      2 |  2 |        2 | 2 ≠ 2 ❌    | Skip              |
| 5 |      3 |  2 |        2 | 3 ≠ 2 ✅    | `arr[2]=3`, `k=3` |
| 6 |      4 |  3 |        3 | 4 ≠ 3 ✅    | `arr[3]=4`, `k=4` |
| 7 |      4 |  4 |        4 | 4 ≠ 4 ❌    | Skip              |

After the operations, the beginning of the array becomes:

```text
[1, 2, 3, 4, ...]
```

The remaining elements after index `k-1` are irrelevant.

So:

```text
k = 4
```

### ✅ Final Output

```text
return k
```

**Output:**

```text
4
```

The unique elements are:

```text
[1, 2, 3, 4]
```

### 🧠 Main idea

`k` is the **count of unique elements**.

For this input:

```text
[1,1,2,2,2,3,4,4]
```

there are **4 unique elements**, so the answer is:

```text
4
```

Time complexity: **O(n)**
Extra space: **O(1)**


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Second Array** | **O(n)** | **O(n)** |
| **Optimized · Write Index** | **O(n)** | **O(1)** |

---

# Merge Sorted Array

**LeetCode #88** · [LeetCode](https://leetcode.com/problems/merge-sorted-array/) · **Easy**

> **Sorted Arrays · Two Pointers · Merge from the Back**

### Approaches

#### 1. Brute Force (Concatenate + Sort)
Copy `nums2` into the empty spaces at the end of `nums1`, then sort the entire array.

**Pseudo Code:**

```text
// Copy nums2 into the tail of nums1

for j ← 0 to n − 1:
    nums1[m + j] ← nums2[j]

sort(nums1)
```
### Dry Run
This pseudocode is for **Merge Sorted Array**. The idea is to first copy all elements of `nums2` into the empty spaces at the end of `nums1`, then sort the complete array.

### Given input

```text
nums1 = [1,4,7,0,0,0]
m = 3

nums2 = [2,3,6]
n = 3
```

Here:

* `m = 3` → first **3 elements** of `nums1` are actual values: `[1,4,7]`
* The `0`s are empty spaces.
* `n = 3` → `nums2` has 3 actual values.

---

### Line 1

```text
# copy nums2 into the tail of nums1
```

This is just a **comment**. It tells us what the next line is going to do.

We want to put:

```text
nums2 = [2,3,6]
```

into the empty portion of `nums1`.

Current:

```text
nums1 = [1,4,7,0,0,0]
                 ↑ ↑ ↑
               empty
```

---

### Line 2

```text
for j ← 0 to n − 1:
```

Since:

```text
n = 3
```

the loop runs:

```text
j = 0
j = 1
j = 2
```

---

### Line 3

```text
nums1[m + j] ← nums2[j]
```

This copies each element of `nums2` into the tail of `nums1`.

#### When `j = 0`

```text
nums1[m + j]
= nums1[3 + 0]
= nums1[3]

nums2[0] = 2
```

So:

```text
nums1[3] = 2
```

Array becomes:

```text
[1,4,7,2,0,0]
```

---

#### When `j = 1`

```text
nums1[m + j]
= nums1[3 + 1]
= nums1[4]

nums2[1] = 3
```

So:

```text
nums1[4] = 3
```

Array:

```text
[1,4,7,2,3,0]
```

---

#### When `j = 2`

```text
nums1[m + j]
= nums1[3 + 2]
= nums1[5]

nums2[2] = 6
```

So:

```text
nums1[5] = 6
```

Array:

```text
[1,4,7,2,3,6]
```

So after the loop:

```text
nums1 = [1,4,7,2,3,6]
```

---

### Line 4

```text
sort(nums1)
```

Now sort the complete `nums1`:

```text
[1,4,7,2,3,6]
```

After sorting:

```text
[1,2,3,4,6,7]
```

### ✅ Final Output

```text
[1,2,3,4,6,7]
```

### Complete dry run

| j | `nums2[j]` | Position `m+j` | nums1           |
| - | ---------: | -------------: | --------------- |
| 0 |          2 |              3 | `[1,4,7,2,0,0]` |
| 1 |          3 |              4 | `[1,4,7,2,3,0]` |
| 2 |          6 |              5 | `[1,4,7,2,3,6]` |

Then:

```text
sort(nums1)
```

gives:

```text
[1,2,3,4,6,7]
```

**Final answer: `nums1 = [1,2,3,4,6,7]`**

The important part to understand is:

```text
nums1[m + j] = nums2[j]
```

`m` tells us **where the empty portion of `nums1` starts**. Here `m = 3`, so copying starts at index `3`.


#### 2. Optimized (Merge Backwards)
Use three pointers:
* `i` → last valid element in `nums1`
* `j` → last element in `nums2`
* `k` → last position in `nums1`

Compare `nums1[i]` and `nums2[j]`, and place the larger value at `nums1[k]`. Move the pointers backward.

**Pseudo Code:**

```text
i ← m − 1
j ← n − 1
k ← m + n − 1

while j ≥ 0:
    if i ≥ 0 and nums1[i] > nums2[j]:
        nums1[k] ← nums1[i]
        i ← i − 1
    else:
        nums1[k] ← nums2[j]
        j ← j − 1

    k ← k − 1
```
### Dry Run
This is the **optimized Merge Sorted Array** approach. Instead of copying `nums2` and then sorting, we merge both arrays **from right to left**.

### Given input

```text
nums1 = [1,4,7,0,0,0]
m = 3

nums2 = [2,3,6]
n = 3
```

The actual elements are:

```text
nums1 → [1,4,7,_,_,_]
nums2 → [2,3,6]
```

We fill `nums1` from the **back** so that we don't overwrite its existing elements.

---

### 1. Initialize `i`

```text
i ← m − 1
```

Since:

```text
m = 3
```

we get:

```text
i = 3 − 1 = 2
```

So `i` points to the last actual element of `nums1`:

```text
nums1 = [1,4,7,0,0,0]
           ↑
          i=2
```

`nums1[i] = 7`

---

### 2. Initialize `j`

```text
j ← n − 1
```

Since:

```text
n = 3
```

we get:

```text
j = 2
```

So `j` points to the last element of `nums2`:

```text
nums2 = [2,3,6]
           ↑
          j=2
```

`nums2[j] = 6`

---

### 3. Initialize `w`

```text
w ← m + n − 1
```

```text
w = 3 + 3 − 1
w = 5
```

`w` tells us **where to put the largest element**.

```text
nums1 = [1,4,7,0,0,0]
                  ↑
                 w=5
```

---

### 4. The while loop

```text
while j ≥ 0:
```

We continue until all elements of `nums2` have been placed.

Initially:

```text
j = 2
```

So loop starts.

---

#### Iteration 1

Current:

```text
i = 2 → nums1[i] = 7
j = 2 → nums2[j] = 6
w = 5
```

Condition:

```text
i ≥ 0 and nums1[i] > nums2[j]
```

becomes:

```text
2 ≥ 0 AND 7 > 6
```

✅ True.

So we choose `7`.

```text
nums1[w] ← nums1[i]
```

Therefore:

```text
nums1[5] = 7
```

Then `i--`:

```text
i = 1
```

And:

```text
w = w - 1
w = 4
```

Array:

```text
[1,4,7,0,0,7]
```

---

#### Iteration 2

Current:

```text
i = 1 → nums1[i] = 4
j = 2 → nums2[j] = 6
w = 4
```

Check:

```text
4 > 6
```

❌ False.

So we choose `nums2[j]`, which is `6`.

```text
nums1[4] = 6
```

Then `j--`:

```text
j = 1
```

And:

```text
w = 3
```

Array:

```text
[1,4,7,0,6,7]
```

---

#### Iteration 3

Current:

```text
i = 1 → 4
j = 1 → 3
w = 3
```

Check:

```text
4 > 3
```

✅ True.

Choose `4`:

```text
nums1[3] = 4
```

Then:

```text
i = 0
w = 2
```

Array:

```text
[1,4,7,4,6,7]
```

---

#### Iteration 4

Current:

```text
i = 0 → 1
j = 1 → 3
w = 2
```

Check:

```text
1 > 3
```

❌ False.

Choose `3`:

```text
nums1[2] = 3
```

Then:

```text
j = 0
w = 1
```

Array:

```text
[1,4,3,4,6,7]
```

---

#### Iteration 5

Current:

```text
i = 0 → 1
j = 0 → 2
w = 1
```

Check:

```text
1 > 2
```

❌ False.

Choose `2`:

```text
nums1[1] = 2
```

Then:

```text
j = -1
w = 0
```

Array:

```text
[1,2,3,4,6,7]
```

---

### Why does the loop stop?

We have:

```text
j = -1
```

So:

```text
while j ≥ 0
```

becomes:

```text
-1 ≥ 0
```

❌ False.

Loop ends.

### ✅ Final Output

```text
nums1 = [1,2,3,4,6,7]
```

### Complete dry run

| Step | `i` | `j` | `w` | Compare | Put |
| ---- | --: | --: | --: | ------- | --- |
| 1    |   2 |   2 |   5 | 7 > 6 ✅ | 7   |
| 2    |   1 |   2 |   4 | 4 > 6 ❌ | 6   |
| 3    |   1 |   1 |   3 | 4 > 3 ✅ | 4   |
| 4    |   0 |   1 |   2 | 1 > 3 ❌ | 3   |
| 5    |   0 |   0 |   1 | 1 > 2 ❌ | 2   |

Final:

```text
[1,2,3,4,6,7]
```

### 🧠 Main idea

There are **three pointers**:

```text
i → last actual element in nums1
j → last element in nums2
w → last empty position in nums1
```

We always put the **larger element at `w`** and move backward.

That's why this approach is efficient:

* **Time:** `O(m + n)`
* **Extra space:** `O(1)`

No separate array and **no sorting** is required.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Concatenate + Sort** | **O((m + n) log(m + n))** | **O(1)** |
| **Optimized · Merge Backwards** | **O(m + n)** | **O(1)** |

---

# Move Zeroes

**LeetCode #283** · [LeetCode](https://leetcode.com/problems/move-zeroes/) · **Easy**

> **Array · Two Pointers · Move zeroes to the end in place**

### Approaches

#### 1. Brute Force (Extra Array)
Create a new array, place all non-zero elements first, and leave the remaining positions as `0`.

**Pseudo Code:**

```text id="7v6k2p"
given arr

aux ← new array of size n filled with 0
write ← 0

for i ← 0 to n − 1:
    if arr[i] ≠ 0:
        aux[write] ← arr[i]
        write ← write + 1

copy aux back into arr
```
### Dry Run
This pseudocode is for **Move Zeroes**. It moves all non-zero elements to the front while keeping their original order, and the remaining positions stay `0`.

### Input

```text
arr = [0,1,0,3,12]
```

`n = 5`

---

### 1. Create auxiliary array

```text
aux ← new array of size n filled with 0
```

Create a new array of size `5`, initially filled with zero:

```text
aux = [0,0,0,0,0]
```

---

### 2. Initialize `write`

```text
write ← 0
```

`write` tells us **where to put the next non-zero element**.

```text
write = 0
```

---

### 3. Loop through the array

```text
for i ← 0 to n − 1:
```

Since `n = 5`, `i` goes:

```text
0, 1, 2, 3, 4
```

---

#### i = 0

```text
arr[0] = 0
```

Check:

```text
if arr[i] ≠ 0
```

```text
0 ≠ 0 ❌
```

So we **don't copy** it.

```text
aux = [0,0,0,0,0]
write = 0
```

---

#### i = 1

```text
arr[1] = 1
```

Check:

```text
1 ≠ 0 ✅
```

So:

```text
aux[write++] = arr[i]
```

Currently:

```text
write = 0
```

Therefore:

```text
aux[0] = arr[1]
aux[0] = 1
```

Then `write++` means increase `write` by 1:

```text
write = 1
```

Now:

```text
aux = [1,0,0,0,0]
```

---

#### i = 2

```text
arr[2] = 0
```

Check:

```text
0 ≠ 0 ❌
```

Skip it.

```text
aux = [1,0,0,0,0]
write = 1
```

---

#### i = 3

```text
arr[3] = 3
```

Check:

```text
3 ≠ 0 ✅
```

So:

```text
aux[write++] = arr[i]
```

Currently:

```text
write = 1
```

Therefore:

```text
aux[1] = 3
```

Then:

```text
write = 2
```

Now:

```text
aux = [1,3,0,0,0]
```

---

#### i = 4

```text
arr[4] = 12
```

Check:

```text
12 ≠ 0 ✅
```

Currently:

```text
write = 2
```

So:

```text
aux[2] = 12
```

Then:

```text
write = 3
```

Now:

```text
aux = [1,3,12,0,0]
```

---

### 4. Copy auxiliary array back

```text
copy aux back into arr
```

Currently:

```text
aux = [1,3,12,0,0]
arr = [0,1,0,3,12]
```

Copy `aux` into `arr`:

```text
arr = [1,3,12,0,0]
```

### ✅ Final Output

```text
[1,3,12,0,0]
```

### Complete dry run

| `i` | `arr[i]` | Action   | `aux`          | `write` |
| --: | -------: | -------- | -------------- | ------: |
|   0 |        0 | Skip     | `[0,0,0,0,0]`  |       0 |
|   1 |        1 | Put `1`  | `[1,0,0,0,0]`  |       1 |
|   2 |        0 | Skip     | `[1,0,0,0,0]`  |       1 |
|   3 |        3 | Put `3`  | `[1,3,0,0,0]`  |       2 |
|   4 |       12 | Put `12` | `[1,3,12,0,0]` |       3 |

Then copy:

```text
aux → arr
```

Result:

```text
[1,3,12,0,0]
```

### 🧠 Main idea

The important line is:

```text
aux[write++] = arr[i]
```

It means:

> "If the current element is non-zero, put it at the next available position in `aux`."

So the non-zero elements:

```text
1 → 3 → 12
```

come to the front **in the same order**, and because `aux` was initially filled with zeros, the remaining positions automatically stay `0`.

**Time:** `O(n)`
**Extra space:** `O(n)` because we created `aux`.


#### 2. Optimized (Same-Direction Two Pointers)
Use a **write index** (`slow`) to place non-zero elements at the front while scanning the array with `fast`. Swapping preserves the relative order of non-zero elements.

**Pseudo Code:**

```text id="3j9x1c"
given arr

slow ← 0

for fast ← 0 to n − 1:
    if arr[fast] ≠ 0:
        swap(arr[slow], arr[fast])
        slow ← slow + 1
```

### Dry Run
This is the **optimized Move Zeroes** approach. Unlike the previous pseudocode, this one works **in-place**, so it does not need an extra array.

### Input

```text
arr = [0,1,0,3,12]
```

`n = 5`

---

### 1. Initialize `slow`

```text
slow ← 0
```

`slow` points to the position where the **next non-zero element** should go.

```text
slow = 0
```

---

### 2. Start the `fast` loop

```text
for fast ← 0 to n − 1:
```

Since `n = 5`:

```text
fast = 0, 1, 2, 3, 4
```

`fast` scans every element.

---

### The Loop

#### 🔹 fast = 0

```text
arr[fast] = arr[0] = 0
```

Check:

```text
arr[fast] ≠ 0
0 ≠ 0 ❌
```

So nothing happens.

```text
arr  = [0,1,0,3,12]
slow = 0
```

---

#### 🔹 fast = 1

```text
arr[fast] = arr[1] = 1
```

Check:

```text
1 ≠ 0 ✅
```

So we execute:

```text
swap(arr[slow], arr[fast])
```

Currently:

```text
slow = 0
fast = 1
```

So:

```text
swap(arr[0], arr[1])
```

Before:

```text
[0,1,0,3,12]
 ↑ ↑
slow fast
```

After swap:

```text
[1,0,0,3,12]
```

Then:

```text
slow++
```

So:

```text
slow = 1
```

---

#### 🔹 fast = 2

```text
arr[fast] = arr[2] = 0
```

Check:

```text
0 ≠ 0 ❌
```

Skip.

```text
arr  = [1,0,0,3,12]
slow = 1
```

---

#### 🔹 fast = 3

```text
arr[fast] = arr[3] = 3
```

Check:

```text
3 ≠ 0 ✅
```

Swap:

```text
swap(arr[slow], arr[fast])
```

Currently:

```text
slow = 1
fast = 3
```

So:

```text
swap(arr[1], arr[3])
```

Before:

```text
[1,0,0,3,12]
   ↑     ↑
 slow   fast
```

After:

```text
[1,3,0,0,12]
```

Then:

```text
slow++
```

```text
slow = 2
```

---

#### 🔹 fast = 4

```text
arr[fast] = arr[4] = 12
```

Check:

```text
12 ≠ 0 ✅
```

Swap:

```text
swap(arr[slow], arr[fast])
```

Currently:

```text
slow = 2
fast = 4
```

So:

```text
swap(arr[2], arr[4])
```

Before:

```text
[1,3,0,0,12]
     ↑     ↑
    slow  fast
```

After:

```text
[1,3,12,0,0]
```

Then:

```text
slow++
```

```text
slow = 3
```

---

### ✅ Final Output

```text
[1,3,12,0,0]
```

### Complete Dry Run

| `fast` | `arr[fast]` | Action           | Array          | `slow` |
| -----: | ----------: | ---------------- | -------------- | -----: |
|      0 |           0 | Skip             | `[0,1,0,3,12]` |      0 |
|      1 |           1 | Swap index 0 & 1 | `[1,0,0,3,12]` |      1 |
|      2 |           0 | Skip             | `[1,0,0,3,12]` |      1 |
|      3 |           3 | Swap index 1 & 3 | `[1,3,0,0,12]` |      2 |
|      4 |          12 | Swap index 2 & 4 | `[1,3,12,0,0]` |      3 |

### 🧠 Main idea

Think of the pointers like this:

```text
slow → position where next non-zero should go
fast → scans the entire array
```

Whenever `fast` finds a non-zero:

```text
swap(arr[slow], arr[fast])
slow++
```

So all non-zero values gradually move to the **left**, while zeroes move to the **right**.

**Time:** `O(n)`
**Extra Space:** `O(1)` ✅

This is better than the previous approach because the previous one used an `aux` array requiring `O(n)` extra space.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Extra Array** | **O(n)** | **O(n)** |
| **Optimized · Two Pointers** | **O(n)** | **O(1)** |

---

# Sort Colors

**LeetCode #75** · [LeetCode](https://leetcode.com/problems/sort-colors/) · **Medium**

> **Array · Dutch National Flag · In-place · Three Pointers**

### Approaches

#### 1. Brute Force (Counting Sort)
Count the number of `0`s, `1`s, and `2`s, then overwrite the array in two passes.

**Pseudo Code:**

```text
given arr

count ← [0, 0, 0]

for v in arr:
    count[v]++

write count[0] zeros
then write count[1] ones
then write count[2] twos

return arr
```
### Dry Run
This pseudocode is for **Sort Colors / Dutch National Flag problem**, but this version uses a **counting approach**.

The array contains only:

```text
0, 1, 2
```

### Input

```text id="q3kq8w"
arr = [2,0,2,1,1,0]
```

---

### 1. Create count array

```text id="l8k0h4"
count ← [0, 0, 0]
```

Here:

```text
count[0] → number of 0s
count[1] → number of 1s
count[2] → number of 2s
```

Initially:

```text id="2w1h7d"
count = [0,0,0]
```

---

### 2. Count each element

```text id="1ocd1k"
for v in arr:
    count[v]++
```

This means:

> Go through every element `v` in `arr` and increase its corresponding count.

#### Iterations

Input:

```text id="qfj2su"
[2,0,2,1,1,0]
```

##### First element: `2`

```text
count[2]++
```

```text id="bq7v5g"
count = [0,0,1]
```

##### Second element: `0`

```text
count[0]++
```

```text id="x7rqg3"
count = [1,0,1]
```

##### Third element: `2`

```text
count[2]++
```

```text id="3w8g9p"
count = [1,0,2]
```

##### Fourth element: `1`

```text
count[1]++
```

```text id="8sg2gq"
count = [1,1,2]
```

##### Fifth element: `1`

```text
count[1]++
```

```text id="j4p3m1"
count = [1,2,2]
```

##### Sixth element: `0`

```text
count[0]++
```

```text id="u0r8dh"
count = [2,2,2]
```

So finally:

```text id="h6e6mi"
count[0] = 2
count[1] = 2
count[2] = 2
```

---

### 3. Write the zeroes

```text id="s8h7vf"
write count[0] zeros
```

Since:

```text id="h8g0h4"
count[0] = 2
```

write two `0`s:

```text id="9smw7e"
[0,0]
```

---

### 4. Write the ones

```text id="v5y8iq"
then count[1] ones
```

Since:

```text id="zjv1xk"
count[1] = 2
```

write two `1`s:

```text id="q6b9z1"
[0,0,1,1]
```

---

### 5. Write the twos

```text id="p9c2qk"
then count[2] twos
```

Since:

```text id="8z7j4p"
count[2] = 2
```

write two `2`s:

```text id="d0n8qu"
[0,0,1,1,2,2]
```

---

### 6. Return the array

```text id="v5h7xw"
return arr
```

So the final sorted array is:

```text id="5n0q7r"
[0,0,1,1,2,2]
```

### Complete Dry Run

| Element | `count`   |
| ------: | --------- |
|       2 | `[0,0,1]` |
|       0 | `[1,0,1]` |
|       2 | `[1,0,2]` |
|       1 | `[1,1,2]` |
|       1 | `[1,2,2]` |
|       0 | `[2,2,2]` |

Then:

```text
2 zeros → [0,0]
2 ones  → [0,0,1,1]
2 twos  → [0,0,1,1,2,2]
```

### ✅ Final Output

```text
[0,0,1,1,2,2]
```

### 🧠 Main idea

The algorithm **doesn't compare elements with each other**.

It simply:

```text
Count how many 0s
Count how many 1s
Count how many 2s
        ↓
Write them back in order
```

**Time:** `O(n)`
**Extra space:** `O(1)` because `count` always has only 3 elements.


#### 2. Better (Two-Pass Partition)
Move `0`s to the front and `2`s to the end using swaps. The remaining elements are `1`s.

**Pseudo Code:**

```text
given arr

write ← 0                    // pass 1: 0s → front

for i ← 0 to n − 1:
    if arr[i] == 0:
        swap(arr[write], arr[i])
        write++

// 0s settled at [0, write)

back ← n − 1
i ← write                    // pass 2: 2s → back

while i ≤ back:
    if arr[i] == 2:
        swap(arr[i], arr[back])
        back--
    else:                    // arr[i] == 1
        i++

return arr
```

### Dry Run
This is a **two-pass approach for Sort Colors**. It sorts the array containing only `0`, `1`, and `2`:

* **Pass 1:** Move all `0`s to the front.
* **Pass 2:** Move all `2`s to the back.
* Whatever remains in the middle will automatically be `1`s.

### Input

```text
arr = [2,0,1,0,1,2,0]
```

`n = 7`

---

### Pass 1: Move all 0s to the front

#### 1. Initialize `write`

```text
write ← 0
```

`write` tells us where the **next 0** should be placed.

```text
write = 0
```

---

#### 2. Loop through the array

```text
for i ← 0 to n − 1:
```

So:

```text
i = 0,1,2,3,4,5,6
```

We check every element.

---

##### i = 0

```text
arr[0] = 2
```

Check:

```text
arr[i] == 0
2 == 0 ❌
```

Nothing happens.

```text
arr = [2,0,1,0,1,2,0]
write = 0
```

---

##### i = 1

```text
arr[1] = 0
```

Check:

```text
0 == 0 ✅
```

Execute:

```text
swap(arr[write], arr[i])
```

Currently:

```text
write = 0
i = 1
```

So:

```text
swap(arr[0], arr[1])
```

Before:

```text
[2,0,1,0,1,2,0]
 ↑ ↑
 w i
```

After:

```text
[0,2,1,0,1,2,0]
```

Then:

```text
write++
```

```text
write = 1
```

---

##### i = 2

```text
arr[2] = 1
```

```text
1 == 0 ❌
```

Skip.

```text
arr = [0,2,1,0,1,2,0]
write = 1
```

---

##### i = 3

```text
arr[3] = 0
```

`0 == 0` ✅

Swap:

```text
swap(arr[1], arr[3])
```

Before:

```text
[0,2,1,0,1,2,0]
   ↑     ↑
 write   i
```

After:

```text
[0,0,1,2,1,2,0]
```

Then:

```text
write = 2
```

---

##### i = 4

```text
arr[4] = 1
```

```text
1 == 0 ❌
```

Skip.

```text
arr = [0,0,1,2,1,2,0]
write = 2
```

---

##### i = 5

```text
arr[5] = 2
```

```text
2 == 0 ❌
```

Skip.

---

##### i = 6

```text
arr[6] = 0
```

`0 == 0` ✅

Swap:

```text
swap(arr[2], arr[6])
```

Before:

```text
[0,0,1,2,1,2,0]
     ↑         ↑
   write       i
```

After:

```text
[0,0,0,2,1,2,1]
```

Then:

```text
write = 3
```

---

#### After Pass 1

```text
arr = [0,0,0,2,1,2,1]
```

The comment:

```text
// 0s settled at [0, write)
```

means indices:

```text
[0, 1, 2]
```

contain all the zeros.

Because `write = 3`, the range `[0,3)` means:

```text
index 0 → 0
index 1 → 0
index 2 → 0
```

So:

```text
[0,0,0 | 2,1,2,1]
```

---

### Pass 2: Move all 2s to the back

#### 1. Initialize `back`

```text
back ← n − 1
```

Since:

```text
n = 7
```

we get:

```text
back = 6
```

`back` tells us where the **next 2** should go.

---

#### 2. Initialize `i`

```text
i ← write
```

Since:

```text
write = 3
```

we get:

```text
i = 3
```

We start checking from index `3` because indices `0–2` already contain the sorted zeros.

Current array:

```text
[0,0,0,2,1,2,1]
       ↑       ↑
       i      back
```

---

#### 3. While loop

```text
while i ≤ back:
```

We continue while `i` has not crossed `back`.

---

##### Iteration 1

```text
i = 3
back = 6
```

```text
arr[3] = 2
```

Condition:

```text
if arr[i] == 2
```

`2 == 2` ✅

So:

```text
swap(arr[i], arr[back])
```

Swap:

```text
swap(arr[3], arr[6])
```

Before:

```text
[0,0,0,2,1,2,1]
       ↑       ↑
       i      back
```

After:

```text
[0,0,0,1,1,2,2]
```

Then:

```text
back--
```

So:

```text
back = 5
```

##### Important

We **do not increase `i`** here.

Why?

Because after swapping, we don't yet know what came into `arr[i]`. We need to check it again.

Now:

```text
i = 3
back = 5
```

---

##### Iteration 2

```text
arr[3] = 1
```

So:

```text
arr[i] == 2
1 == 2 ❌
```

Go to `else`:

```text
i++
```

Therefore:

```text
i = 4
```

Array remains:

```text
[0,0,0,1,1,2,2]
```

---

##### Iteration 3

```text
arr[4] = 1
```

Again:

```text
1 == 2 ❌
```

So:

```text
i++
```

```text
i = 5
```

---

##### Iteration 4

```text
arr[5] = 2
```

`2 == 2` ✅

Swap:

```text
swap(arr[5], arr[back])
```

Currently:

```text
i = 5
back = 5
```

So we're swapping the same position:

```text
swap(arr[5], arr[5])
```

Array remains:

```text
[0,0,0,1,1,2,2]
```

Then:

```text
back--
```

```text
back = 4
```

---

Now:

```text
i = 5
back = 4
```

Check:

```text
i ≤ back
5 ≤ 4 ❌
```

Loop stops.

---

### Final Output

```text
[0,0,0,1,1,2,2]
```

### Complete dry run

#### Pass 1 — move `0`s

| `i` | `arr[i]` | Action     | Array             | `write` |
| --: | -------: | ---------- | ----------------- | ------: |
|   0 |        2 | Skip       | `[2,0,1,0,1,2,0]` |       0 |
|   1 |        0 | Swap 0 & 1 | `[0,2,1,0,1,2,0]` |       1 |
|   2 |        1 | Skip       | `[0,2,1,0,1,2,0]` |       1 |
|   3 |        0 | Swap 1 & 3 | `[0,0,1,2,1,2,0]` |       2 |
|   4 |        1 | Skip       | `[0,0,1,2,1,2,0]` |       2 |
|   5 |        2 | Skip       | `[0,0,1,2,1,2,0]` |       2 |
|   6 |        0 | Swap 2 & 6 | `[0,0,0,2,1,2,1]` |       3 |

#### Pass 2 — move `2`s

| `i` | `back` | `arr[i]` | Action     | Array             |
| --: | -----: | -------: | ---------- | ----------------- |
|   3 |      6 |        2 | Swap 3 & 6 | `[0,0,0,1,1,2,2]` |
|   3 |      5 |        1 | `i++`      | `[0,0,0,1,1,2,2]` |
|   4 |      5 |        1 | `i++`      | `[0,0,0,1,1,2,2]` |
|   5 |      5 |        2 | Swap 5 & 5 | `[0,0,0,1,1,2,2]` |

### ✅ Final Answer

```text
arr = [0,0,0,1,1,2,2]
```

### 🧠 Main idea

Think of the two passes as:

```text
Pass 1:
[anything] → [0s | anything]

Pass 2:
[0s | anything] → [0s | 1s | 2s]
```

So we don't need counting and don't need an extra array.

**Time:** `O(n)`
**Extra space:** `O(1)` ✅


#### 3. Optimized (Dutch National Flag)
Use three pointers:
* `low` → position for `0`
* `mid` → current element
* `high` → position for `2`

Rules:
* If `0` → swap with `low`, move `low` and `mid`.
* If `1` → move `mid`.
* If `2` → swap with `high`, move `high` only.

**Pseudo Code:**

```text
low ← 0
mid ← 0
high ← n − 1

while mid ≤ high:

    if arr[mid] == 0:
        swap(arr[low], arr[mid])
        low++
        mid++

    else if arr[mid] == 1:
        mid++

    else:                    // arr[mid] == 2
        swap(arr[mid], arr[high])
        high--

return arr
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Counting Sort** | **O(n)** | **O(1)** |
| **Better · Two-Pass Partition** | **O(n)** | **O(1)** |
| **Optimized · Dutch National Flag** | **O(n)** | **O(1)** |

---

# Rotate Array by K Places

**LeetCode #189** · [LeetCode](https://leetcode.com/problems/rotate-array/) · **Medium**

> **Array · Three Reversals · In-place · No Extra Array**

### Approaches

#### 1. Brute Force (Rotate by One)
Move the last element to the front, shifting the remaining elements one position to the right. Repeat `k` times.

**Pseudo Code:**

```text id="9f3r2a"
given arr, k

repeat k times:
    move the last element to the front
    shift the remaining elements one position right

return arr
```
### Dry Run
This pseudocode is used to **rotate an array to the right by `k` positions**.

Given:

```text
arr = [1,2,3,4,5,6,7]
k = 3
```

We have to move the **last element to the front** 3 times.

---

### Pseudocode

```text id="5x5q8w"
repeat k times:
    move the last element to the front, shifting the rest right
return arr
```

### Line 1

```text id="7v1m3k"
repeat k times:
```

Since:

```text id="f7h2x9"
k = 3
```

we perform the operation **3 times**.

---

### 🔹 1st repetition

Current array:

```text id="7o8v2m"
[1,2,3,4,5,6,7]
```

Last element is:

```text id="3g9x1a"
7
```

Move `7` to the front and shift everything else one position right:

```text id="2s6k4p"
[7,1,2,3,4,5,6]
```

---

### 🔹 2nd repetition

Current:

```text id="h4j8q2"
[7,1,2,3,4,5,6]
```

Last element:

```text id="p3m7x1"
6
```

Move `6` to the front:

```text id="q8v2n5"
[6,7,1,2,3,4,5]
```

---

### 🔹 3rd repetition

Current:

```text id="r5k9w3"
[6,7,1,2,3,4,5]
```

Last element:

```text id="m2z6p8"
5
```

Move `5` to the front:

```text id="a7c4x1"
[5,6,7,1,2,3,4]
```

---

### Final `return arr`

```text id="j9q3v7"
return arr
```

So the final output is:

```text id="w2n6k8"
[5,6,7,1,2,3,4]
```

### Complete dry run

| Rotation | Array             |
| -------- | ----------------- |
| Initial  | `[1,2,3,4,5,6,7]` |
| 1st      | `[7,1,2,3,4,5,6]` |
| 2nd      | `[6,7,1,2,3,4,5]` |
| 3rd      | `[5,6,7,1,2,3,4]` |

### ✅ Final Output

```text
[5,6,7,1,2,3,4]
```

### 🧠 Main idea

Every repetition does:

```text
Last element → Front
Everything else → shift right
```

Since `k = 3`, we do it 3 times.

**Time complexity:** `O(n × k)`
**Extra space:** `O(1)` if the shifting is done in-place.


#### 2. Optimized (Reverse Three Times)
* First, calculate `k = k % n`.
* Reverse the entire array.
* Reverse the first `k` elements.
* Reverse the remaining `n − k` elements.

**Pseudo Code:**

```text id="2c7m1x"
given arr, k

k ← k mod n

reverse(arr, 0, n − 1)
reverse(arr, 0, k − 1)
reverse(arr, k, n − 1)

return arr
```
### Dry Run
This is the **optimized method to rotate an array to the right by `k` positions** using the **reversal algorithm**.

Given:

```text
arr = [1,2,3,4,5,6,7]
k = 3
```

`n = 7`

---

### 1. `k ← k mod n`

```text id="y6kq2m"
k ← k mod n
```

This makes `k` smaller than `n`.

Here:

```text
k = 3
n = 7

k = 3 mod 7
k = 3
```

So `k` remains:

```text id="q3v8x1"
k = 3
```

#### Why do we do this?

If `k` was `10`, rotating by 10 positions in an array of 7 is the same as rotating by:

```text
10 mod 7 = 3
```

---

### 2. Reverse the entire array

```text id="m7r2p4"
reverse(arr, 0, n − 1)
```

Here:

```text
0 to n − 1
= 0 to 6
```

So reverse:

```text id="d8w5q1"
[1,2,3,4,5,6,7]
```

After reversing:

```text id="z4c9n2"
[7,6,5,4,3,2,1]
```

---

### 3. Reverse the first `k` elements

```text id="p2x7m6"
reverse(arr, 0, k − 1)
```

Since:

```text
k = 3
```

we get:

```text
reverse(arr, 0, 2)
```

So reverse the first 3 elements:

```text id="v6n3q8"
[7,6,5 | 4,3,2,1]
```

Reverse `[7,6,5]`:

```text id="h1k5r9"
[5,6,7,4,3,2,1]
```

---

### 4. Reverse the remaining elements

```text id="x8m4p2"
reverse(arr, k, n − 1)
```

Substitute:

```text
k = 3
n − 1 = 6
```

So:

```text id="w3q7j1"
reverse(arr, 3, 6)
```

We reverse:

```text
[5,6,7 | 4,3,2,1]
             ↑
          indices 3–6
```

Reverse `[4,3,2,1]`:

```text id="c5v9k2"
[5,6,7,1,2,3,4]
```

---

### Complete Dry Run

| Step                 | Array             |
| -------------------- | ----------------- |
| Initial              | `[1,2,3,4,5,6,7]` |
| Reverse entire array | `[7,6,5,4,3,2,1]` |
| Reverse first 3      | `[5,6,7,4,3,2,1]` |
| Reverse remaining    | `[5,6,7,1,2,3,4]` |

### ✅ Final Output

```text
[5,6,7,1,2,3,4]
```

---

### 🧠 Why does this work?

We want:

```text
[1,2,3,4 | 5,6,7]
```

Right rotation by `3` means:

```text
[5,6,7 | 1,2,3,4]
```

The three reversals achieve exactly that:

```text
Original:
[1,2,3,4,5,6,7]

① Reverse everything:
[7,6,5,4,3,2,1]

② Reverse first 3:
[5,6,7,4,3,2,1]

③ Reverse remaining:
[5,6,7,1,2,3,4]
```

### Complexity

**Time:** `O(n)` because each element is reversed a constant number of times.

**Extra space:** `O(1)` because we reverse the array **in-place**.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Rotate by One** | **O(n · k)** | **O(1)** |
| **Optimized · Reverse Three Times** | **O(n)** | **O(1)** |

---

# 4Sum

**LeetCode #18** · [LeetCode](https://leetcode.com/problems/4sum/) · **Medium**

> **Array · Sort + Two Pointers · Find Unique Quadruples**

### Approaches

#### 1. Brute Force (Four Nested Loops)
Use four loops to check every possible quadruple, then remove duplicates.

**Pseudo Code:**

```text
for i < j < k < l:
    if arr[i] + arr[j] + arr[k] + arr[l] == target:
        record it

de-duplicate the results
```
### Dry Run
This pseudocode is for **4Sum using brute force**. We choose **4 different indices** `i, j, k, l` such that:

```text
i < j < k < l
```

and check whether their sum equals `target`.

### Input

```text
arr = [1,0,-1,0,-2,2]
target = 0
```

There are 6 elements, so we check every possible combination of 4 elements.

---

### 1. `for i < j < k < l`

```text
for i < j < k < l:
```

This means we choose 4 indices in increasing order.

For example:

```text
i=0, j=1, k=2, l=3
```

is valid.

But:

```text
i=2, j=0, k=1, l=3
```

is not valid because the indices are not increasing.

This also prevents using the **same index twice**.

---

### 2. Check the sum

```text
if arr[i] + arr[j] + arr[k] + arr[l] == target:
```

For every group of 4 elements, calculate their sum.

Since:

```text
target = 0
```

we are looking for four numbers whose sum is `0`.

---

### 3. `record it`

```text
record it
```

If the sum is `0`, save that group of four values.

---

### Let's check the combinations

For:

```text
arr = [1,0,-1,0,-2,2]
```

#### Combination 1

```text
[1,0,-1,0]
```

Sum:

```text
1 + 0 + (-1) + 0 = 0
```

✅ Record:

```text
[1,0,-1,0]
```

---

#### Combination 2

```text
[1,0,-1,-2]
```

Sum:

```text
1 + 0 - 1 - 2 = -2
```

❌ Not recorded.

---

#### Combination 3

```text
[1,0,-1,2]
```

Sum:

```text
1 + 0 - 1 + 2 = 2
```

❌

---

#### Combination 4

```text
[1,0,0,-2]
```

Sum:

```text
1 + 0 + 0 - 2 = -1
```

❌

---

#### Combination 5

```text
[1,0,0,2]
```

Sum:

```text
1 + 0 + 0 + 2 = 3
```

❌

---

#### Combination 6

```text
[1,0,-2,2]
```

Sum:

```text
1 + 0 - 2 + 2 = 1
```

❌

---

#### Combination 7

```text
[1,-1,0,-2]
```

Sum:

```text
1 - 1 + 0 - 2 = -2
```

❌

---

#### Combination 8

```text
[1,-1,0,2]
```

Sum:

```text
1 - 1 + 0 + 2 = 2
```

❌

---

#### Combination 9

```text
[1,-1,-2,2]
```

Sum:

```text
1 - 1 - 2 + 2 = 0
```

✅ Record:

```text
[1,-1,-2,2]
```

---

#### Combination 10

```text
[1,0,-2,2]
```

Sum:

```text
1 + 0 - 2 + 2 = 1
```

❌

---

#### Combination 11

```text
[0,-1,0,-2]
```

Sum:

```text
0 - 1 + 0 - 2 = -3
```

❌

---

#### Combination 12

```text
[0,-1,0,2]
```

Sum:

```text
0 - 1 + 0 + 2 = 1
```

❌

---

#### Combination 13

```text
[0,-1,-2,2]
```

Sum:

```text
0 - 1 - 2 + 2 = -1
```

❌

---

#### Combination 14

```text
[0,0,-2,2]
```

Sum:

```text
0 + 0 - 2 + 2 = 0
```

✅ Record:

```text
[0,0,-2,2]
```

---

### 4. De-duplicate the results

```text
de-duplicate the results
```

This means if the same **value combination** is found more than once, keep only one copy.

A common way is to sort each quadruplet:

```text
[1,0,-1,0] → [-1,0,0,1]
[1,-1,-2,2] → [-2,-1,1,2]
[0,0,-2,2] → [-2,0,0,2]
```

So the unique 4Sum results are:

```text
[
  [-2,-1,1,2],
  [-2,0,0,2],
  [-1,0,0,1]
]
```

### ✅ Final Output

```text
[[-2,-1,1,2],
 [-2,0,0,2],
 [-1,0,0,1]]
```

### 🧠 Main idea

The brute-force approach checks **every possible group of 4 elements**:

```text
i < j < k < l
       ↓
4 elements
       ↓
sum == target?
       ↓
yes → record
```

For `n = 6`, there are:

```text
C(6,4) = 15
```

possible combinations.

**Time complexity:** `O(n⁴)`
**Extra space:** depends on how we store/de-duplicate the results.


#### 2. Optimized (Sort + Two Pointers)
Sort the array, fix the first two elements using two loops, then use `lo` and `hi` pointers for the remaining two elements. Skip duplicates to avoid duplicate quadruples.

**Pseudo Code:**

```text
sort(arr)

for i:
    skip if arr[i] == arr[i − 1]

    for j > i:
        skip duplicates
        lo ← j + 1
        hi ← n − 1

        if sum == target:
            record
            skip duplicates
            move both

        else if sum < target:
            lo ← lo + 1

        else:
            hi ← hi − 1

return quadruples
```
### Dry Run
This is the **optimized 4Sum approach** using **sorting + two pointers**.

Input:

```text id="8f1zqa"
arr = [-2,-1,0,0,1,2]
target = 0
```

We want **4 numbers whose sum is 0**.

---

### 1. Sort the array

```text id="q7m3ax"
sort(arr)
```

The array is already sorted:

```text id="4p8y2w"
[-2,-1,0,0,1,2]
```

---

### 2. First loop: choose `i`

```text id="v9x5kt"
for i:
    skip if arr[i] == arr[i − 1]
```

`i` chooses the **first number** of our quadruple.

We will then choose a second number `j`, and use two pointers `lo` and `hi` to find the remaining two.

---

### 3. Second loop: choose `j`

```text id="2g7q1m"
for j > i:
```

`j` chooses the **second number**.

Then:

```text id="h4x8nd"
lo ← j + 1
hi ← n − 1
```

So `lo` starts immediately after `j`, while `hi` starts at the end.

---

### 4. Check the sum

We calculate:

```text id="x1c6v9"
sum = arr[i] + arr[j] + arr[lo] + arr[hi]
```

Then:

```text
sum == target
```

If the sum is:

* `0` → record quadruple
* `< 0` → increase `lo`
* `> 0` → decrease `hi`

Because the array is sorted, these pointer movements help us find the answer efficiently.

---

### Complete Dry Run

Array:

```text id="h7s3kp"
[-2,-1,0,0,1,2]
```

Indices:

```text id="5v0n8x"
  0   1 2 3 4 5
 -2  -1 0 0 1 2
```

---

#### 🔹 i = 0

```text id="y6p2rm"
arr[i] = -2
```

Now `j` starts at `1`.

##### j = 1

```text id="m5w9q3"
arr[j] = -1
lo = 2
hi = 5
```

Values:

```text
-2 + (-1) + 0 + 2
```

Sum:

```text id="x2k7av"
-1
```

Since:

```text
sum < target
```

we increase `lo`:

```text id="j3p8cd"
lo = 3
```

---

##### Now `lo = 3`

Values:

```text id="7n4q1b"
-2 + (-1) + 0 + 2 = -1
```

Still `< 0`.

So:

```text id="s8d2kf"
lo = 4
```

Now:

```text id="m0r6xy"
-2 + (-1) + 1 + 2 = 0
```

✅ Found a quadruple:

```text id="q4v7mz"
[-2,-1,1,2]
```

Record it.

Then:

```text id="z5c9la"
lo++
hi--
```

So:

```text id="k8n3wp"
lo = 5
hi = 4
```

Now:

```text
lo < hi
5 < 4 ❌
```

Stop this `j`.

---

##### j = 2

Now:

```text id="u2f6qa"
arr[j] = 0
lo = 3
hi = 5
```

Calculate:

```text id="r3x9kc"
-2 + 0 + 0 + 2 = 0
```

✅ Found:

```text id="w6p1nz"
[-2,0,0,2]
```

Record it.

Move both:

```text
lo = 4
hi = 4
```

Stop because:

```text
lo < hi
4 < 4 ❌
```

---

##### j = 3

`arr[3] = 0`.

But:

```text
arr[3] == arr[2]
```

Both are `0`.

So:

```text id="e8t4vq"
skip duplicate j
```

This prevents finding the same value combination again.

---

##### j = 4

```text
arr[j] = 1
lo = 5
hi = 5
```

Since:

```text
lo < hi
5 < 5 ❌
```

No possible pair.

So `i = 0` is finished.

---

#### 🔹 i = 1

Now:

```text id="c1k8ds"
arr[1] = -1
```

`j` starts at `2`.

##### j = 2

```text id="z9m4hx"
arr[j] = 0
lo = 3
hi = 5
```

Calculate:

```text id="p3x7kc"
-1 + 0 + 0 + 2 = 1
```

`sum > 0`, so:

```text id="b6q2mn"
hi--
```

```text id="r8v4za"
hi = 4
```

Now:

```text
-1 + 0 + 0 + 1 = 0
```

✅ Found:

```text id="n5w2qt"
[-1,0,0,1]
```

Record it.

Move both:

```text id="k7c3px"
lo = 4
hi = 3
```

Stop.

---

##### j = 3

`arr[3] = 0`, but:

```text
arr[3] == arr[2]
```

So skip duplicate `j`.

---

#### 🔹 i = 2

```text
arr[2] = 0
```

`j = 3`.

```text
lo = 4
hi = 5
```

Calculate:

```text id="q6n1bz"
0 + 0 + 1 + 2 = 3
```

`sum > 0`, so:

```text
hi--
```

```text
hi = 4
```

Now:

```text
lo < hi
4 < 4 ❌
```

Stop.

---

#### 🔹 i = 3

Now:

```text
arr[3] = 0
```

But:

```text
arr[3] == arr[2]
```

So we **skip duplicate `i`**.

---

### Final quadruples

We found:

```text
[-2,-1,1,2]
[-2,0,0,2]
[-1,0,0,1]
```

Therefore:

### ✅ Final Output

```text
[[-2,-1,1,2],
 [-2,0,0,2],
 [-1,0,0,1]]
```

---

### 🧠 Most important part

For every `i` and `j`, we use:

```text
lo → moves right when sum is too small
hi → moves left when sum is too large
```

Because the array is sorted:

```text
sum < 0  → lo++
sum > 0  → hi--
sum == 0 → record + move both
```

### Complexity

```text
Sorting       → O(n log n)
Four-pointer  → O(n³)
Total         → O(n³)
```

Extra space is approximately **O(1)** apart from the space needed to store the answer.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Four Nested Loops** | **O(n⁴)** | **O(1)** |
| **Optimized · Sort + Two Pointers** | **O(n³)** | **O(1)** |
