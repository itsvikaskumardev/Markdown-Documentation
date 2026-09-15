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

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Four Nested Loops** | **O(n⁴)** | **O(1)** |
| **Optimized · Sort + Two Pointers** | **O(n³)** | **O(1)** |
