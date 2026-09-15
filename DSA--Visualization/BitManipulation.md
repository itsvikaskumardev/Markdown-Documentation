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

#### 2. Optimized (XOR)
XOR all numbers together. Since `x ^ x = 0` and `0 ^ x = x`, every pair cancels out and only the single number remains.

**Pseudo Code:**

```text id="r2n8qp"
acc ← 0

for x in nums:
    acc ← acc ^ x    // pairs cancel

return acc
```

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
