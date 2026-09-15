# Spiral Matrix

**LeetCode #54** · [LeetCode](https://leetcode.com/problems/spiral-matrix/) · **Medium**

> **Matrix · Four Shrinking Boundaries · Spiral Order**

### Approaches

#### 1. Boundary Simulation
Maintain four boundaries:
* `top` → top row
* `bottom` → bottom row
* `left` → left column
* `right` → right column

**Pseudo Code:**

```text
top=0, bottom=R−1, left=0, right=C−1

while top ≤ bottom and left ≤ right:

    for c in left..right:  take (top, c)

    top++

    for r in top..bottom:  take (r, right)

    right−−

    if top ≤ bottom:

        for c in right..left: take (bottom, c)

        bottom−−

    if left ≤ right:

        for r in bottom..top: take (r, left)

        left++

return collected
```

**Traverse:**

1. Left → right across `top`
2. Top → bottom down `right`
3. Right → left across `bottom`
4. Bottom → top up `left`

After each traversal, shrink the corresponding boundary. Check boundaries before the last two traversals to avoid duplicates.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Boundary Simulation** | **O(R · C)** | **O(1)** |

---

# Rotate Image

**LeetCode #48** · [LeetCode](https://leetcode.com/problems/rotate-image/) · **Medium**

> **Matrix · 90° Clockwise Rotation · In-place**

### Approaches

#### 1. Optimized (Transpose + Reverse Rows)
* **Step 1:** Transpose the matrix by swapping `M[r][c]` with `M[c][r]`.
* **Step 2:** Reverse every row.

This rotates the matrix **90° clockwise** without using another 2D array.

**Pseudo Code:**

```text id="x3q8nm"
given matrix (n × n)

// phase 1: transpose

for r in 0..n−1:
    for c in r+1..n−1:
        swap M[r][c], M[c][r]

// phase 2: reverse each row

for each row:
    swap M[r][c], M[r][n−1−c]

done
```

#### 2. Direct 4-Cycle Rotation

**Pseudo Code:**

```text id="n5v2kd"
given matrix (n × n)

for layer in 0..n/2:

    for i in layer..n−1−layer:

        save top

        top   ← left
        left  ← bottom
        bottom ← right
        right  ← saved top
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Transpose + Reverse Rows** | **O(n²)** | **O(1)** |
| **Direct 4-Cycle Rotation** | **O(n²)** | **O(1)** |

---

# Set Matrix Zeroes

**LeetCode #73** · [LeetCode](https://leetcode.com/problems/set-matrix-zeroes/) · **Medium**

> **Matrix · Mark Rows/Columns Containing 0 · In-place**

### Approaches

#### 1. Brute Force (Row & Column Sets)
First record all rows and columns containing `0` using two sets. Then make those rows and columns `0`.

**Pseudo Code:**

```text
given matrix

zeroRows = {}, zeroCols = {}

for each cell:
    if it is 0:
        add its row, its col

// second pass

for each cell:
    if row or col flagged:
        cell = 0
```

#### 2. Optimized (First Row/Column Markers)
Use the **first row and first column** to mark which rows and columns should become `0`. Handle the first row and first column separately to avoid losing their original information.

**Pseudo Code:**

```text
firstRowZero = any 0 in row 0

firstColZero = any 0 in col 0

for r,c in interior:
    if M[r][c]==0:
        M[r][0]=0
        M[0][c]=0

for r,c in interior:
    if M[r][0]==0 or M[0][c]==0:
        M[r][c]=0

if firstRowZero:
    zero row 0

if firstColZero:
    zero col 0

done
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Row & Column Sets** | **O(R · C)** | **O(R + C)** |
| **Optimized · First Row/Column Markers** | **O(R · C)** | **O(1)** |

---

# Find Missing and Repeated Values

**LeetCode #2965** · [LeetCode](https://leetcode.com/problems/find-missing-and-repeated-values/) · **Easy**

> **Matrix · Find One Repeated and One Missing Value · Sums and Squares**

### Approaches

#### 1. Brute Force (Count Occurrences)
Use a frequency map to count each value, then find the value appearing twice and the value appearing zero times.

**Pseudo Code:**

```text
count ← {}

for each cell:
    count[value] ← count[value] + 1

return the value counted twice, and the one counted zero times
```

#### 2. Optimized (Sums and Squares)
Use the **sum** and **sum of squares** equations for `1 ... n²` to derive the repeated and missing values without extra space.

**Pseudo Code:**

```text
expected Σ ← m(m+1)/2
expected Σ² ← m(m+1)(2m+1)/6
where m = n²

measure the actual Σ and Σ² in one pass

a − b ← Σ − expectedΣ
// a = repeated, b = missing

a + b ← (Σ² − expectedΣ²) / (a − b)

return [(sumDiff + plus) / 2, plus − a]
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Count Occurrences** | **O(n²)** | **O(n²)** |
| **Optimized · Sums and Squares** | **O(n²)** | **O(1)** |

---