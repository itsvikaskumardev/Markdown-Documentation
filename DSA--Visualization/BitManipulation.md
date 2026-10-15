# Table of Contents

- [Single Number](#single-number)
- [Number of 1 Bits](#number-of-1-bits)
- [Missing Number](#missing-number)
- [Reverse Bits](#reverse-bits)
- [Sum of Two Integers](#sum-of-two-integers)

---

# Single Number

**LeetCode #136** · [LeetCode](https://leetcode.com/problems/single-number/) · **Easy**

> **Array · XOR Fold · Pairs Cancel**

### Approaches

#### 1. Brute Force (Hash Count)
Use a frequency map to count each number, then return the number whose count is `1`.

**Pseudo Code:**

```text id="v7m3kx"
count ← {}

for x in nums:
    count[x]++

return the key with count == 1
```

### Dry Run

Problem: Find the Single Number

Input: `nums = [4, 1, 2, 1, 2]`

The goal is to find the number that appears exactly once. Every other number appears twice.

### 1. Explain the pseudocode line by line

Line 1:

```text
count ← {}
```

- Create an empty dictionary (frequency map).
- It will store each number and how many times it appears.
- Example: `{4: 1, 1: 2}` means `4` appeared once and `1` appeared twice.

Line 2:

```text
for x in nums:
    count[x]++
```

- Visit every number in the array.
- `count[x]++` means increase the count of `x` by 1.
- If `x` is not already in the dictionary, its count starts at `0`, then increases to `1`.

Line 3:

```text
return the key with count == 1
```

- Check the dictionary for a number whose count is `1`.
- Return that number because it appears only once.

### 2. Dry run

Input: `[4, 1, 2, 1, 2]`

| Step | Current `x` | Dictionary after update |
| :---: | :---: | :--- |
| **1** | 4 | `{4: 1}` |
| **2** | 1 | `{4: 1, 1: 1}` |
| **3** | 2 | `{4: 1, 1: 1, 2: 1}` |
| **4** | 1 | `{4: 1, 1: 2, 2: 1}` |
| **5** | 2 | `{4: 1, 1: 2, 2: 2}` |

Now check which number has a count of `1`:

- `4` → appears once
- `1` → appears twice
- `2` → appears twice

### ✅ Final Output

Output:

# `4`

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — visit each array element once.
- Space: \\(O(n)\\) — the dictionary can store up to \\(n\\) distinct numbers.

Interview tip: The dictionary method counts frequencies. There is also an XOR-based solution for this problem that uses \\(O(1)\\) extra space.

#### 2. Optimized (XOR)
XOR all numbers together. Since `x ^ x = 0` and `0 ^ x = x`, every pair cancels out and only the single number remains.

**Pseudo Code:**

```text id="r2n8qp"
acc ← 0

for x in nums:
    acc ← acc ^ x    // pairs cancel

return acc
```

### Dry Run

Problem: Find the Single Number Using XOR

Input: `nums = [4, 1, 2, 1, 2]`

The goal is to find the number that appears only once. This time, we use the XOR (`^`) operator instead of a dictionary.

### 1. Explain the pseudocode line by line

Line 1:

```text
acc ← 0
```

- Create a variable `acc` and initialize it to `0`.
- `acc` stores the running XOR result.

Line 2:

```text
for x in nums:
    acc ← acc ^ x
```

- Visit every number in the array.
- XOR the current number `x` with `acc`.
- Store the result back in `acc`.

What is XOR (`^`)?

XOR compares two numbers bit by bit:

| Expression | Result | Why? |
| :---: | :---: | :--- |
| `4 ^ 4` | `0` | Same numbers cancel |
| `1 ^ 1` | `0` | Same numbers cancel |
| `0 ^ 5` | `5` | XOR with 0 keeps the number |
| `4 ^ 1` | `5` | Different numbers produce a new result |

The key properties are:

- \\(a \oplus a = 0\\)
- \\(a \oplus 0 = a\\)
- XOR is commutative and associative, so the order does not matter.

Line 3:

```text
return acc
```

- Return the final XOR result.
- Every number that appears twice cancels itself out, leaving the number that appears once.

### 2. Dry run step by step

Input: `[4, 1, 2, 1, 2]`

| Step | Current `x` | Calculation | `acc` |
| :---: | :---: | :---: | :---: |
| **Initial** | — | `acc = 0` | 0 |
| **1** | 4 | `0 ^ 4` | 4 |
| **2** | 1 | `4 ^ 1` | 5 |
| **3** | 2 | `5 ^ 2` | 7 |
| **4** | 1 | `7 ^ 1` | 6 |
| **5** | 2 | `6 ^ 2` | 4 |

The final result is `4`.

### 3. Why does it work?

We can rearrange the XOR operations because their order does not matter:

\\[ 4 \oplus 1 \oplus 2 \oplus 1 \oplus 2 \\]

Group the repeated numbers together:

\\[ 4 \oplus (1 \oplus 1) \oplus (2 \oplus 2) \\]

Since equal numbers XOR to zero:

\\[ 4 \oplus 0 \oplus 0 = 4 \\]

### ✅ Final Output

Output:

# `4`

### 🧠 Key idea

Time and space complexity:
- Time complexity: \\(O(n)\\) — each element is processed once.
- Extra space complexity: \\(O(1)\\) — only the `acc` variable is used.

Remember: The dictionary method counts how many times each number appears, while the XOR method cancels out pairs without needing a dictionary.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Hash Count** | **O(n)** | **O(n)** |
| **Optimized · XOR** | **O(n)** | **O(1)** |

---

# Number of 1 Bits

**LeetCode #191** · [LeetCode](https://leetcode.com/problems/number-of-1-bits/) · **Easy**

> **Bit Manipulation · Hamming Weight · `n & (n - 1)`**

### Approaches

#### 1. Brute Force (Check Each Bit)
Check every bit using `(n >> i) & 1` and count how many bits are `1`.

**Pseudo Code:**

```text
count ← 0

for i in 0..width-1:
    count += (n >> i) & 1

return count
```
### Dry Run

Problem: Count the Number of Set Bits

Input: `n = 11`

This pseudocode counts how many 1s are present in the binary representation of `n`. This is also called counting set bits or the Hamming weight.

### 1. Explain the pseudocode line by line

Line 1:

```text
count ← 0
```

- Initialize `count` to `0`.
- It will store the total number of `1` bits found.

Line 2:

```text
for i in 0..width-1:
```

- Loop through every bit position of the number.
- `width` is the number of bits being checked.
- Bit positions start at `0`, from the rightmost bit.

Line 3:

```text
count += (n >> i) & 1
```

This line has two bitwise operations:

- `n >> i`: Shift the binary number right by `i` positions.
- `& 1`: Check the rightmost bit after shifting. The result is either `0` or `1`.
- `count +=`: Add that result to `count`.

Line 4:

```text
return count
```

- Return the total number of set bits.

### 2. Dry run with `n = 11`

First, convert `11` to binary:

\\[ 11 = (1011)_2 \\]

Binary representation: `1011`

It contains three `1` bits.

Assuming we check 4 bits (`width = 4`):

| `i` | `n >> i` in binary | `(n >> i) & 1` | `count` |
| :---: | :---: | :---: | :---: |
| **Initial** | — | — | 0 |
| **0** | `1011` (11) | 1 | 1 |
| **1** | `0101` (5) | 1 | 2 |
| **2** | `0010` (2) | 0 | 2 |
| **3** | `0001` (1) | 1 | 3 |

### ✅ Final Output

Output:

# `3`

Why? The binary representation of `11` is `1011`, which contains three `1`s.

### 🧠 Key idea

Complexity:
- Time: \\(O(\text{width})\\) — checks each bit once.
- Extra space: \\(O(1)\\) — uses only a counter and loop variable.

Interview tip: In a fixed-width integer, `width` is usually the number of bits in the integer type. For this example, four bits are enough to represent `11`.

#### 2. Optimized (`n & (n - 1)`)
Each operation removes the **rightmost set bit** (`1`) from `n`. Keep doing this until `n = 0`; the number of operations equals the number of set bits.

**Pseudo Code:**

```text
count ← 0

while n != 0:
    n ← n & (n - 1)    // clear lowest set bit
    count++

return count
```

### Dry Run

Problem: Count Set Bits Using `n & (n - 1)`

Input: `n = 128`

This pseudocode counts the number of `1` bits in a number's binary representation. It uses a clever bitwise trick to remove one `1` bit in each loop iteration.

### 1. Explain the pseudocode line by line

Line 1:

```text
count ← 0
```

- Initialize `count` to `0`.
- It records how many `1` bits we remove.

Line 2:

```text
while n != 0:
```

- Keep looping as long as `n` is not zero.
- When all the `1` bits are removed, `n` becomes `0` and the loop stops.

Line 3:

```text
n ← n & (n - 1)
```

- `n - 1` subtracts one from `n`.
- `&` is the bitwise AND operator.
- The expression `n & (n - 1)` clears the rightmost (lowest) set bit, meaning it changes the lowest `1` to `0`.

Line 4:

```text
count++
```

- Increase `count` by `1`.
- Each iteration removes exactly one set bit, so we count one bit each time.

Line 5:

```text
return count
```

- Return the total number of `1` bits.

### 2. Dry run with `n = 128`

First, convert `128` to binary:

\\[ 128 = (10000000)_2 \\]

There is only one `1` bit.

| Step | `n` before | `n - 1` in binary | `n & (n - 1)` | `count` |
| :---: | :---: | :---: | :---: | :---: |
| **Initial** | 128 (`10000000`) | — | — | 0 |
| **1** | 128 (`10000000`) | `01111111` | `00000000` = 0 | 1 |

After the first iteration, `n = 0`. The loop stops.

### 3. Another quick example

For `n = 11` (`1011` in binary):

- `1011 & 1010 = 1010` → count = 1
- `1010 & 1001 = 1000` → count = 2
- `1000 & 0111 = 0000` → count = 3

The answer is `3`.

### ✅ Final Output

Output:

# `1`

Why? The binary representation of `128` is `10000000`, which contains exactly one `1` bit.

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(k)\\), where \\(k\\) is the number of set bits. Each iteration removes one `1`.
- Extra space: \\(O(1)\\), because only a counter and a few variables are needed.

Remember: The previous method checks every bit position. This method checks only the `1` bits, so it can be faster when a number has few set bits.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Check Each Bit** | **O(#bits)** | **O(1)** |
| **Optimized · `n & (n - 1)`** | **O(#set bits)** | **O(1)** |

---

# Missing Number

**LeetCode #268** · [LeetCode](https://leetcode.com/problems/missing-number/) · **Easy**

> **Array · XOR Index ⊕ Value · Pairs Cancel**

### Approaches

#### 1. Brute Force (Sum Formula)
Calculate the expected sum of `0...n` and subtract the actual sum of the array.

**Pseudo Code:**

```text
expected ← n·(n+1)/2
actual ← 0

for x in nums:
    actual ← actual + x

return expected − actual
```
### Dry Run

Problem: Find the Missing Number

Input: `nums = [3, 0, 1]`

The goal is to find the missing number from the range `0` to `n`, where `n` is the length of the array.

For `[3, 0, 1]`, the numbers should be `0, 1, 2, 3`. The missing number is `2`.

### 1. Explain the pseudocode line by line

Line 1:

```text
expected ← n · (n + 1) / 2
```

- Calculate the sum of all numbers from `0` to `n`.
- Formula: \\(\frac{n(n+1)}{2}\\)
- Here, `n = 3`, because the array contains 3 elements.

Calculation:

\\[ \text{expected}=\frac{3(3+1)}{2}=\frac{12}{2}=6 \\]

So, `expected = 6`.

Line 2:

```text
for x in nums:
    actual ← actual + x
```

- Visit every element in the array.
- Add each number to `actual`.
- Important: Before the loop, initialize `actual ← 0`. This initialization is missing from your pseudocode.

Line 3:

```text
return expected − actual
```

- Subtract the sum of the existing numbers from the expected sum.
- The difference is the missing number.

### 2. Dry run

Input: `[3, 0, 1]`

| Step | Current `x` | Calculation | `actual` |
| :---: | :---: | :---: | :---: |
| **Initial** | — | `actual = 0` | 0 |
| **1** | 3 | `0 + 3` | 3 |
| **2** | 0 | `3 + 0` | 3 |
| **3** | 1 | `3 + 1` | 4 |

Now calculate the result:

\\[ \text{missing}=\text{expected}-\text{actual} \\]

\\[ =6-4=2 \\]

### ✅ Final Output

Output:

# `2`

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — one pass through the array.
- Extra space: \\(O(1)\\) — only a few variables are needed.

Remember: The formula calculates the sum if no number were missing. Subtracting the actual sum reveals the missing number.

#### 2. Optimized (XOR)
XOR all numbers from `0...n` with all values in the array. Matching numbers cancel out because `x ^ x = 0`, leaving only the missing number.

**Pseudo Code:**

```text
acc ← 0

acc ← acc ^ n          // top index has no slot

for i in 0..n-1:
    acc ← acc ^ i      // fold the index
    acc ← acc ^ nums[i]  // fold the value (pairs cancel)

return acc
```

### Dry Run

Problem: Find the Missing Number Using XOR

Input: `nums = [0, 1]`

The goal is to find the missing number from the range `0` to `n`, where `n` is the array length.

Here, the array has 2 elements, so the complete range should be `[0, 1, 2]`. The missing number is `2`.

### 1. Explain the pseudocode line by line

Line 1:

```text
acc ← 0
```

- Initialize `acc` to `0`.
- This variable stores the running XOR result.

Line 2:

```text
acc ← acc ^ n
```

- XOR `acc` with `n`.
- Since the array length is `2`, this becomes `0 ^ 2 = 2`.
- We include `n` because the full range goes from `0` through `n`, but the array has only `n` elements.

Line 3:

```text
for i in 0..n-1:
```

- Loop through the array indices from `0` to `n-1`.
- For this input, the indices are `0` and `1`.

Line 4:

```text
acc ← acc ^ i
```

- XOR the current index `i` into `acc`.
- This includes the expected numbers from the range `0` through `n-1`.

Line 5:

```text
acc ← acc ^ nums[i]
```

- XOR the array value at the current index into `acc`.
- Numbers that appear in both the expected range and the array cancel out because `x ^ x = 0`.

Line 6:

```text
return acc
```

- Return the final XOR result, which is the missing number.

### 2. Dry run

Input: `nums = [0, 1]`, so `n = 2`.

| Step | Operation | `acc` |
| :---: | :---: | :---: |
| **Initial** | `acc = 0` | 0 |
| **Include `n`** | `0 ^ 2` | 2 |
| **Index `i = 0`** | `2 ^ 0` | 2 |
| **Value `nums[0] = 0`** | `2 ^ 0` | 2 |
| **Index `i = 1`** | `2 ^ 1` | 3 |
| **Value `nums[1] = 1`** | `3 ^ 1` | 2 |

The final value remains `2` because `0` and `1` each appear twice in the XOR operations and cancel out.

### ✅ Final Output

Output:

# `2`

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — each array element is processed once.
- Extra space: \\(O(1)\\) — only the `acc` variable and loop index are needed.

Remember: This method combines the expected numbers and the actual array values using XOR. Every matching number cancels itself, leaving only the missing number.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Sum Formula** | **O(n)** | **O(1)** |
| **Optimized · XOR** | **O(n)** | **O(1)** |

---

# Reverse Bits

**LeetCode #190** · [LeetCode](https://leetcode.com/problems/reverse-bits/) · **Easy**

> **Bit Manipulation · Build the Result Bit by Bit**

### Approaches

#### 1. Optimized (Build Result Bit by Bit)
Process all **32 bits** one at a time. Take the lowest bit using `n & 1`, append it to `result` using `(result << 1) | bit`, then shift `n` right. After 32 iterations, `result` contains the reversed bits.

**Pseudo Code:**

```text
result ← 0

repeat WIDTH times:
    result ← (result << 1) | (n & 1)  // pull lowest bit of n into result
    n >>= 1                           // discard that bit

return result
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Build Result Bit by Bit** | **O(32) = O(1)** | **O(1)** |

---

# Sum of Two Integers

**LeetCode #371** · [LeetCode](https://leetcode.com/problems/sum-of-two-integers/) · **Medium**

> **Bit Manipulation · XOR for Sum · AND + Shift for Carry**

### Approaches

#### 1. Optimized (XOR Sum + Carry)
Use `a ^ b` to calculate the sum **without carry**, and `(a & b) << 1` to calculate the **carry**. Repeat until there is no carry (`b == 0`).

**Pseudo Code:**

```text
// a + b without + or −

while b != 0:

    carry ← (a & b) << 1   // overflow columns
    a ← a ^ b              // sum without carry
    b ← carry

return a
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **XOR Sum + Carry** | **O(1)** | **O(1)** |
