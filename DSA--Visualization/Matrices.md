# Table of Contents

- [Spiral Matrix](#spiral-matrix)
- [Rotate Image](#rotate-image)
- [Set Matrix Zeroes](#set-matrix-zeroes)
- [Find Missing and Repeated Values](#find-missing-and-repeated-values)

---

# Spiral Matrix

**LeetCode #54** · [LeetCode](https://leetcode.com/problems/spiral-matrix/) · **Medium**

> **Matrix · Four Shrinking Boundaries · Spiral Order**

Soln : DSA 01-Page No 79
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

### Dry Run
This pseudocode is used to **traverse a matrix in spiral order** — first top row, then right column, then bottom row, then left column, and repeat inward.

Given:

```text
matrix =
[
 [1,  2,  3,  4],
 [5,  6,  7,  8],
 [9, 10, 11, 12]
]
```

There are **3 rows × 4 columns**.

```text
R = 3
C = 4
```

### Pseudocode

```text id="q7m2kc"
top = 0
bottom = R−1
left = 0
right = C−1

while top ≤ bottom and left ≤ right:

    for c in left..right:
        take (top, c)
    top++

    for r in top..bottom:
        take (r, right)
    right−−

    if top ≤ bottom:
        for c in right..left:
            take (bottom, c)
        bottom−−

    if left ≤ right:
        for r in bottom..top:
            take (r, left)
        left++

return collected
```

---

### 1. Initialize boundaries

#### `top = 0`

Top row is row `0`.

```text
top = 0
```

#### `bottom = R−1`

Since `R = 3`:

```text
bottom = 3 - 1 = 2
```

So rows are:

```text
0
1
2
```

#### `left = 0`

First column:

```text
left = 0
```

#### `right = C−1`

Since `C = 4`:

```text
right = 4 - 1 = 3
```

So columns are:

```text
0  1  2  3
```

Therefore initially:

```text
top = 0
bottom = 2
left = 0
right = 3
```

---

### 2. `while top ≤ bottom and left ≤ right`

We continue while there is still a valid rectangle to process.

Initially:

```text
0 ≤ 2  ✅
0 ≤ 3  ✅
```

So enter the loop.

---

### 3. Take the top row

```text
for c in left..right:
    take (top, c)
```

Currently:

```text
top = 0
left = 0
right = 3
```

So:

```text
(0,0) → 1
(0,1) → 2
(0,2) → 3
(0,3) → 4
```

Collected:

```text
[1,2,3,4]
```

Matrix:

```text
[1, 2, 3, 4]  ← taken
[5, 6, 7, 8]
[9,10,11,12]
```

---

### 4. `top++`

We have already processed row `0`.

Move the top boundary down:

```text
top = 1
```

Now:

```text
top = 1
bottom = 2
left = 0
right = 3
```

---

### 5. Take the right column

```text
for r in top..bottom:
    take (r, right)
```

Current:

```text
top = 1
bottom = 2
right = 3
```

So:

```text
(1,3) → 8
(2,3) → 12
```

Collected:

```text
[1,2,3,4,8,12]
```

Matrix:

```text
[1, 2, 3, 4]
[5, 6, 7, 8]  ← 8
[9,10,11,12]  ← 12
```

---

### 6. `right--`

Column `3` is completed.

Move right boundary left:

```text
right = 2
```

Now:

```text
top = 1
bottom = 2
left = 0
right = 2
```

---

### 7. `if top ≤ bottom`

Check:

```text
1 ≤ 2
```

True ✅

So we process the bottom row.

---

### 8. Take bottom row from right to left

```text
for c in right..left:
    take (bottom, c)
```

Current:

```text
bottom = 2
right = 2
left = 0
```

Move from column `2` to `0`:

```text
(2,2) → 11
(2,1) → 10
(2,0) → 9
```

Collected:

```text
[1,2,3,4,8,12,11,10,9]
```

Matrix:

```text
[1,  2,  3,  4]
[5,  6,  7,  8]
[9, 10, 11, 12] ← taken right → left
```

---

### 9. `bottom--`

Bottom row `2` is completed.

```text
bottom = 1
```

Now:

```text
top = 1
bottom = 1
left = 0
right = 2
```

Only row `1` remains.

---

### 10. `if left ≤ right`

Check:

```text
0 ≤ 2
```

True ✅

So process the left column.

---

### 11. Take left column from bottom to top

```text
for r in bottom..top:
    take (r, left)
```

Current:

```text
bottom = 1
top = 1
left = 0
```

So:

```text
(1,0) → 5
```

Collected:

```text
[1,2,3,4,8,12,11,10,9,5]
```

---

### 12. `left++`

Column `0` is completed.

```text
left = 1
```

Now boundaries are:

```text
top = 1
bottom = 1
left = 1
right = 2
```

---

### 13. Next `while` iteration

Check:

```text
top ≤ bottom
1 ≤ 1 ✅

left ≤ right
1 ≤ 2 ✅
```

So continue.

#### Take top row

```text
for c in left..right
```

That means:

```text
(1,1) → 6
(1,2) → 7
```

Collected:

```text
[1,2,3,4,8,12,11,10,9,5,6,7]
```

Then:

```text
top++
```

So:

```text
top = 2
```

---

#### Take right column

Now:

```text
top = 2
bottom = 1
```

The loop:

```text
for r in top..bottom
```

has no valid range because:

```text
2 > 1
```

So nothing is added.

Then:

```text
right--
```

```text
right = 1
```

The remaining boundaries are invalid:

```text
top = 2
bottom = 1
```

So the `while` loop stops.

---

### Final Spiral Order

Let's visualize the direction:

```text
1 → 2 → 3 → 4
            ↓
5 → 6 → 7   8
↑           ↓
9 ← 10 ← 11 12
```

The actual order is:

```text
1 → 2 → 3 → 4
              ↓
              8
              ↓
              12
              ↓
11 ← 10 ← 9
↑
5
↓
6 → 7
```

### ✅ Final Output

```text
[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

### 🧠 Main idea

The four boundaries shrink after every round:

```text
Top row    → left to right
Right col  → top to bottom
Bottom row → right to left
Left col   → bottom to top
```

Then the boundaries move inward:

```text
top++
right--
bottom--
left++
```

**Time Complexity:** `O(R × C)` — every element is visited once.
**Extra Space:** `O(1)` apart from the `collected` output.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Boundary Simulation** | **O(R · C)** | **O(1)** |

---

# Rotate Image

**LeetCode #48** · [LeetCode](https://leetcode.com/problems/rotate-image/) · **Medium**

> **Matrix · 90° Clockwise Rotation · In-place**
Soln : DSA 01-Page No 66

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
### Dry Run
This pseudocode **rotates an `n × n` matrix 90° clockwise**.

It does this in **2 phases**:

1. **Transpose** the matrix.
2. **Reverse every row**.

Given:

```text
matrix =
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```

---

### Phase 1: Transpose

Transpose means converting:

```text
M[r][c] ↔ M[c][r]
```

Basically, **rows become columns**.

#### Code

```text id="v2q8n7"
for r in 0..n−1:
    for c in r+1..n−1:
        swap M[r][c], M[c][r]
```

Here:

```text
n = 3
```

So:

```text
r = 0, 1, 2
```

But `c` starts from `r+1` because we only need to swap the **upper triangle**. We don't want to swap elements twice.

---

#### `r = 0`

Now:

```text
c = r + 1
  = 1
```

##### `c = 1`

Swap:

```text
M[0][1] ↔ M[1][0]
```

That means:

```text
2 ↔ 4
```

Matrix becomes:

```text id="x3i4qk"
[1,4,3]
[2,5,6]
[7,8,9]
```

---

##### `c = 2`

Swap:

```text
M[0][2] ↔ M[2][0]
```

That means:

```text
3 ↔ 7
```

Matrix:

```text id="4a4p9v"
[1,4,7]
[2,5,6]
[3,8,9]
```

---

#### `r = 1`

Now:

```text
c = r + 1
  = 2
```

Swap:

```text
M[1][2] ↔ M[2][1]
```

That means:

```text
6 ↔ 8
```

Matrix becomes:

```text id="s6j4fh"
[1,4,7]
[2,5,8]
[3,6,9]
```

---

#### `r = 2`

Now:

```text
c = 3
```

But column `3` doesn't exist because indices are only `0,1,2`.

So nothing happens.

#### After Phase 1

The matrix is:

```text id="z8d7x4"
[
 [1,4,7],
 [2,5,8],
 [3,6,9]
]
```

This is the **transpose**.

---

### Phase 2: Reverse Each Row

Now we run:

```text id="0u6zkw"
for each row:
    swap M[r][c], M[r][n−1−c]
```

Since:

```text
n = 3
```

the opposite index is:

```text
n - 1 - c
= 3 - 1 - c
= 2 - c
```

We reverse each row.

---

#### Row 0

Current:

```text id="q1g9w3"
[1,4,7]
```

Swap first and last:

```text
1 ↔ 7
```

Becomes:

```text id="w4zj2p"
[7,4,1]
```

---

#### Row 1

Current:

```text id="3jv5za"
[2,5,8]
```

Swap:

```text
2 ↔ 8
```

Becomes:

```text id="9w3f6k"
[8,5,2]
```

---

#### Row 2

Current:

```text id="qk7h1a"
[3,6,9]
```

Swap:

```text
3 ↔ 9
```

Becomes:

```text id="p4z8mc"
[9,6,3]
```

---

### Final Matrix

```text id="e8z1qv"
[
 [7,4,1],
 [8,5,2],
 [9,6,3]
]
```

### ✅ Final Output

```text id="nq4y7s"
[[7,4,1],
 [8,5,2],
 [9,6,3]]
```

This is the original matrix rotated **90° clockwise**:

```text id="6c1q3m"
Original:        90° Clockwise:

1 2 3            7 4 1
4 5 6     →      8 5 2
7 8 9            9 6 3
```

### 🧠 Main trick to remember

```text
90° clockwise rotation
        ↓
    Transpose
        ↓
 Reverse every row
```

**Time Complexity:** `O(n²)`
**Extra Space:** `O(1)` because everything is done using swaps in-place.


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
### Dry Run
This pseudocode rotates an `n × n` matrix **90° clockwise in-place**, using a **layer-by-layer 4-way swap**.

Given:

```text
matrix =
[
 [5,  1,  9, 11],
 [2,  4,  8, 10],
 [13, 3,  6,  7],
 [15,14, 12, 16]
]
```

---

### Pseudocode

```text
for layer in 0..n/2:
    for i in layer..n−1−layer:
        save top
        top    ← left
        left   ← bottom
        bottom ← right
        right  ← saved top
```

The idea is to take **4 elements at a time**:

```text
        top
         ↓
left →      ← right
         ↑
       bottom
```

For a **90° clockwise rotation**:

```text
top    ← left
right  ← top
bottom ← right
left   ← bottom
```

---

### Initial Matrix

```text
5   1   9   11
2   4   8   10
13  3   6   7
15  14  12  16
```

Here:

```text
n = 4
```

There are `n/2 = 2` layers:

```text
Layer 0 → outer layer
Layer 1 → inner layer
```

---

### Layer 0 — Outer Layer

```text
layer = 0
```

The outer boundary is:

```text
5   1   9   11
2   4   8   10
13  3   6   7
15  14  12  16
```

We process the positions around this layer.

---

#### i = 0

The four elements are:

```text
top    = 5
left   = 15
bottom = 16
right  = 11
```

The code says:

```text
save top
```

So:

```text
saved top = 5
```

Then:

```text
top ← left
```

So:

```text
top = 15
```

Then:

```text
left ← bottom
```

So:

```text
left = 16
```

Then:

```text
bottom ← right
```

So:

```text
bottom = 11
```

Finally:

```text
right ← saved top
```

So:

```text
right = 5
```

The matrix becomes:

```text
15  1   9   5
2   4   8   10
13  3   6   7
16  14  12  11
```

---

#### i = 1

Now the next four elements around the outer layer are:

```text
top    = 1
left   = 14
bottom = 12
right  = 10
```

Save top:

```text
saved top = 1
```

Then:

```text
top ← left
     = 14

left ← bottom
     = 12

bottom ← right
        = 10

right ← saved top
       = 1
```

Matrix becomes:

```text
15  14  9   5
2   4   8   1
13  3   6   7
16  12  10  11
```

---

#### i = 2

Now:

```text
top    = 9
left   = 12? 
```

Be careful: for this rotation, the positions for the third outer swap are:

```text
top    = 9
left   = 3
bottom = 14
right  = 8
```

So:

```text
saved top = 9
top       = 3
left      = 14
bottom    = 8
right     = 9
```

Matrix becomes:

```text
15  14  3   5
2   4   8   1
13  3?  6   7
16  12  10  11
```

However, this reveals an important issue: **the pseudocode as written is incomplete/ambiguous because it does not specify the coordinates of `top`, `left`, `bottom`, and `right`.**

For the standard 4-way rotation algorithm, the correct coordinate-based pseudocode is:

```text
for layer = 0 .. n/2 - 1:
    first = layer
    last = n - 1 - layer

    for i = first .. last - 1:

        offset = i - first

        top = M[first][i]

        M[first][i] = M[last-offset][first]

        M[last-offset][first] = M[last][last-offset]

        M[last][last-offset] = M[i][last]

        M[i][last] = top
```

Using that standard interpretation, the correct final result is:

```text
7   13  2   5
6   3   4   1
8   4   8   9
16  12  10  11
```

But this result is inconsistent because the input values must each appear exactly once. So let's correct the calculation directly.

For the given matrix, the **correct 90° clockwise rotation** is:

```text
15  13  2   5
14  3   4   1
12  6   8   9
16  7   10  11
```

### ✅ Final Output

```text
[
 [15,13,2,5],
 [14,3,4,1],
 [12,6,8,9],
 [16,7,10,11]
]
```

Visual check:

```text
Original                  90° clockwise

5   1   9   11            15  13  2   5
2   4   8   10     →      14  3   4   1
13  3   6   7             12  6   8   9
15  14  12  16            16  7   10  11
```

**Important:** The pseudocode you provided is missing the **coordinates** for `top`, `left`, `bottom`, and `right`. The intended algorithm is clearly the standard 4-way layer rotation, but to dry-run it exactly, those coordinate calculations need to be specified.


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
### Dry Run
This pseudocode is for **Set Matrix Zeroes**.

### 🧠 Main idea

If any cell contains `0`, make its **entire row and entire column `0`**.

Given:

```text
matrix =
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

---

### Pseudocode

```text
zeroRows = {}
zeroCols = {}

for each cell:
    if it is 0:
        add its row, its col

// second pass
for each cell:
    if row or col flagged:
        cell = 0
```

---

### Step 1: Create `zeroRows` and `zeroCols`

```text
zeroRows = {}
zeroCols = {}
```

These are sets used to remember which rows and columns contain a zero.

Initially:

```text
zeroRows = {}
zeroCols = {}
```

---

### Step 2: First pass through every cell

```text
for each cell:
    if it is 0:
        add its row, its col
```

We check every element.

Matrix with indices:

```text
       col
       0 1 2
      -------
row 0 |1 1 1
row 1 |1 0 1
row 2 |1 1 1
```

We find:

```text
matrix[1][1] = 0
```

So its:

```text
row = 1
col = 1
```

Add them:

```text
zeroRows = {1}
zeroCols = {1}
```

#### Important

We **don't immediately make the row/column zero**.

We only remember them.

This prevents newly-created zeroes from incorrectly affecting other rows/columns.

---

### Step 3: Second pass

Now:

```text
for each cell:
    if row or col flagged:
        cell = 0
```

Meaning:

> If the cell's row is in `zeroRows` OR its column is in `zeroCols`, make it `0`.

We have:

```text
zeroRows = {1}
zeroCols = {1}
```

---

#### Row 0

```text
[1,1,1]
```

* `(0,0)` → row 0 not flagged, column 0 not flagged → keep `1`
* `(0,1)` → column 1 flagged → `0`
* `(0,2)` → not flagged → keep `1`

Row becomes:

```text
[1,0,1]
```

---

#### Row 1

Original:

```text
[1,0,1]
```

Row `1` is flagged, so **every cell becomes `0`**:

```text
[0,0,0]
```

---

#### Row 2

```text
[1,1,1]
```

* `(2,0)` → keep `1`
* `(2,1)` → column 1 flagged → `0`
* `(2,2)` → keep `1`

Row becomes:

```text
[1,0,1]
```

---

### Final Matrix

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

### ✅ Final Output

```text
[[1,0,1],
 [0,0,0],
 [1,0,1]]
```

### 🧠 Visual understanding

Original:

```text
1  1  1
1  0  1
1  1  1
```

The `0` is at **row 1, column 1**.

So:

```text
        ↓
1  0  1
0  0  0  ← entire row
1  0  1
        ↑
     column
```

**Time Complexity:** `O(R × C)`
**Space Complexity:** `O(R + C)` for `zeroRows` and `zeroCols`.


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

### Dry Run
This is the **optimized Set Matrix Zeroes** approach.

The key idea is to use the **first row and first column as markers**, so we don't need separate `zeroRows` and `zeroCols` sets.

Given:

```text
matrix =
[
 [0,1,2,0],
 [3,4,5,2],
 [1,3,1,5]
]
```

---

### 1. `firstRowZero = any 0 in row 0`

Check the first row:

```text
[0,1,2,0]
```

There are zeroes at column `0` and column `3`.

Therefore:

```text
firstRowZero = true
```

We remember this because later the first row itself needs to become zero.

---

### 2. `firstColZero = any 0 in col 0`

Check the first column:

```text
0
3
1
```

There is a `0` at:

```text
M[0][0]
```

Therefore:

```text
firstColZero = true
```

So currently:

```text
firstRowZero = true
firstColZero = true
```

---

### 3. First interior pass

```text
for r,c in interior:
    if M[r][c] == 0:
        M[r][0] = 0
        M[0][c] = 0
```

#### What is "interior"?

We don't process the first row or first column here.

For our `3 × 4` matrix:

```text
       0  1  2  3
     ------------
0 |   0  1  2  0
1 |   3  4  5  2
2 |   1  3  1  5
```

Interior is:

```text
M[1][1], M[1][2], M[1][3]
M[2][1], M[2][2], M[2][3]
```

Notice there are **no zeroes** in the interior.

So nothing is changed during this pass.

Matrix remains:

```text
[
 [0,1,2,0],
 [3,4,5,2],
 [1,3,1,5]
]
```

---

### 4. Second interior pass

```text
for r,c in interior:
    if M[r][0] == 0 or M[0][c] == 0:
        M[r][c] = 0
```

Now we use the **first row and first column as markers**.

Currently:

```text
First row:
[0, 1, 2, 0]

First column:
[0,
 3,
 1]
```

---

#### Row 1

##### `M[1][1]`

Check:

```text
M[1][0] = 3
M[0][1] = 1
```

Neither is zero.

So:

```text
M[1][1] = 4
```

---

##### `M[1][2]`

Check:

```text
M[1][0] = 3
M[0][2] = 2
```

Neither is zero.

Keep:

```text
M[1][2] = 5
```

---

##### `M[1][3]`

Check:

```text
M[1][0] = 3
M[0][3] = 0
```

Column `3` is marked because the original first row had a zero there.

Therefore:

```text
M[1][3] = 0
```

Row 1 becomes:

```text
[3,4,5,0]
```

---

#### Row 2

##### `M[2][1]`

Check:

```text
M[2][0] = 1
M[0][1] = 1
```

Neither zero.

Keep:

```text
3
```

---

##### `M[2][2]`

Check:

```text
M[2][0] = 1
M[0][2] = 2
```

Neither zero.

Keep:

```text
1
```

---

##### `M[2][3]`

Check:

```text
M[2][0] = 1
M[0][3] = 0
```

Column `3` is marked.

Therefore:

```text
M[2][3] = 0
```

Row 2 becomes:

```text
[1,3,1,0]
```

---

### 5. `if firstRowZero: zero row 0`

We already found:

```text
firstRowZero = true
```

So make the **entire first row zero**.

Before:

```text
[0,1,2,0]
```

After:

```text
[0,0,0,0]
```

---

### 6. `if firstColZero: zero col 0`

We also found:

```text
firstColZero = true
```

So make the **entire first column zero**.

Before:

```text
0
3
1
```

After:

```text
0
0
0
```

---

### Final Matrix

Therefore:

```text
[
 [0,0,0,0],
 [0,4,5,0],
 [0,3,1,0]
]
```

### ✅ Final Output

```text
[[0,0,0,0],
 [0,4,5,0],
 [0,3,1,0]]
```

### 🧠 Why we need `firstRowZero` and `firstColZero`

The first row and first column are being used as **storage/markers**.

For example, the original:

```text
[0,1,2,0]
```

contains zeroes that tell us:

* **Column 0 → zero**
* **Column 3 → zero**
* **Row 0 → zero**

We save these facts in:

```text
firstRowZero = true
firstColZero = true
```

Otherwise, using the first row/column as markers could make us lose information about whether they originally contained zeroes.

**Time Complexity:** `O(R × C)`
**Extra Space:** `O(1)` — no separate row/column arrays or sets.

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