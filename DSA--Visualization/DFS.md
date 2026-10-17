# Table of Contents

- [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
- [Path Sum](#path-sum)
- [Validate Binary Search Tree](#validate-binary-search-tree)
- [Binary Tree Tilt](#binary-tree-tilt)
- [Diameter of a Binary Tree](#diameter-of-a-binary-tree)
- [Path Sum II](#path-sum-ii)
- [Longest Univalue Path](#longest-univalue-path)
- [Invert Binary Tree](#invert-binary-tree)
- [Same Tree](#same-tree)
- [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
- [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)
- [Serialize and Deserialize Binary Tree](#serialize-and-deserialize-binary-tree)

---

# Maximum Depth of Binary Tree

**LeetCode #104** · [LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/) · **Easy**

> **Binary Tree · Recursive DFS · Post-order · Depth = 1 + max(left, right)**

### Approaches

#### 1. Optimized (Recursive DFS)
Recursively find the depth of the left and right subtrees, then return `1 + max(left, right)`. For a `null` node, return `0`.

**Pseudo Code:**

```text
depth(node):

    push frame

    if node is null: return 0      // base case

    L = depth(node.left)

    R = depth(node.right)

    return 1 + max(L, R)            // combine
```
### Dry Run

Problem: Maximum Depth of a Binary Tree

Input: `root = [3, 9, 20, null, null, 15, 7]`

The goal is to find the maximum depth (height) of the binary tree — the number of nodes along the longest path from the root to a leaf.

### 1. Understand the input tree

```text
        3
       / \
      9   20
          / \
         15  7
```

The longest paths are `3 → 20 → 15` and `3 → 20 → 7`. Each path contains 3 nodes.

### 2. Explain the pseudocode line by line

Line 1:

```text
depth(node):
    push frame
```

- Call the `depth` function for the current node.
- `push frame` means a recursive call adds a new frame to the call stack. Each frame remembers which node is being processed.

Line 2:

```text
if node is null: return 0
```

- This is the base case.
- If the node does not exist, its depth is `0`.
- This stops recursion when the function reaches a missing child.

Line 3:

```text
L = depth(node.left)
```

- Recursively calculate the depth of the left subtree.
- Store the result in `L`.

Line 4:

```text
R = depth(node.right)
```

- Recursively calculate the depth of the right subtree.
- Store the result in `R`.

Line 5:

```text
return 1 + max(L, R)
```

- `max(L, R)` chooses the deeper subtree.
- Add `1` to count the current node.
- Return the depth of the entire subtree rooted at this node.

### 3. Dry run step by step

Recursion first travels down to the leaf nodes, then calculates depths while returning back up the tree.

| Node | Left depth (`L`) | Right depth (`R`) | Calculation | Returned depth |
| :---: | :---: | :---: | :---: | :---: |
| `15` | 0 | 0 | `1 + max(0, 0)` | 1 |
| `7` | 0 | 0 | `1 + max(0, 0)` | 1 |
| `9` | 0 | 0 | `1 + max(0, 0)` | 1 |
| `20` | 1 | 1 | `1 + max(1, 1)` | 2 |
| `3` | 1 | 2 | `1 + max(1, 2)` | 3 |

### Visualize the return process:

* Node `15` → Returns depth `1`
* Node `7` → Returns depth `1`
* Node `20` → `1 + max(1, 1) = 2`
* Node `3` → `1 + max(1, 2) = 3`

### ✅ Final Output

Output:

# `3`

The maximum depth is 3, because the longest root-to-leaf path contains three nodes.

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — each node is visited once.
- Space: \\(O(h)\\) for the recursive call stack, where \\(h\\) is the tree height. Worst case: \\(O(n)\\); balanced tree: \\(O(\log n)\\).

Interview tip: The base case returns `0` for a null node. Each real node adds `1` to the maximum depth of its two subtrees.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Path Sum

**LeetCode #112** · [LeetCode](https://leetcode.com/problems/path-sum/) · **Easy**

> **Binary Tree · Recursive DFS · Carry remaining sum down · Root-to-leaf**

### Approaches

#### 1. Optimized (Recursive DFS)
Subtract `node.val` from the remaining target as you move down. At a leaf, return `remaining == 0`; otherwise, check the left or right subtree.

**Pseudo Code:**

```text
hasPathSum(node, remaining):

    remaining -= node.val

    if leaf: return remaining == 0     // base case

    if hasPathSum(node.left, remaining): return true

    if hasPathSum(node.right, remaining): return true

    return false                       // combine

// answer = hasPathSum(root, target)
```

### Dry Run

Problem: Path Sum

This algorithm solves the Path Sum problem for a binary tree.

It checks whether there is at least one path from the root to a leaf whose node values add up to `targetSum`.

A leaf is a node that has no left or right child.

### 1. Understand the input tree

Input: `root = [5, 4, 8, 11, null, 13, 4, 7, 2, null, null, null, 1]`

`targetSum = 22`

The tree looks like this:

```text
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

The successful path is `5 → 4 → 11 → 2`.

### 2. Explain the pseudocode line by line

Line 1:

```text
hasPathSum(node, remaining):
```

The function receives:
- `node`: the current node.
- `remaining`: the sum still needed to reach the target.

Initially, `remaining = 22`.

Line 2:

```text
remaining -= node.val
```

Subtract the current node's value from the remaining target.

For example, at the root node `5`:

`remaining = 22 - 5 = 17`

Now we need another `17` from the path below this node.

Line 3:

```text
if leaf: return remaining == 0
```

Check whether the current node is a leaf.

If it is a leaf, return `true` only if `remaining == 0`.

For example, along the path `5 → 4 → 11 → 2`:

\\[ 5+4+11+2=22 \\]

After subtracting each value, the remaining sum becomes `0` at node `2`. Since node `2` is a leaf, the function returns `true`.

If the remaining sum is not zero at a leaf, that path fails.

Line 4:

```text
if hasPathSum(node.left, remaining): return true
```

Recursively check the left subtree.

If the left subtree contains a valid root-to-leaf path, immediately return `true`.

Line 5:

```text
if hasPathSum(node.right, remaining): return true
```

If the left subtree does not find a valid path, check the right subtree.

If either side succeeds, the function returns `true`.

Line 6:

```text
return false
```

If the current node is not a successful leaf and neither subtree finds a valid path, return `false`.

Important: The pseudocode assumes `node` is not null when the function starts. In an implementation, add a null-node check before accessing `node.val`, such as `if node is null: return false`.

### 3. Dry run

Let's follow the path exploration:

| Current node | Remaining before subtraction | Calculation | Remaining after | Result |
| :---: | :---: | :---: | :---: | :--- |
| `5` | 22 | `22 - 5` | 17 | Not a leaf, go left |
| `4` | 17 | `17 - 4` | 13 | Not a leaf, go left |
| `11` | 13 | `13 - 11` | 2 | Not a leaf, explore children |
| `7` | 2 | `2 - 7` | -5 | Leaf with `remaining ≠ 0` → Fails |
| `2` | 2 | `2 - 2` | 0 | Leaf with `remaining == 0` → **Succeeds!** |

The algorithm first explores node `7`. That path fails because it reaches a leaf with remaining sum `-5`.

Then it explores node `2`. That path succeeds because it reaches a leaf with remaining sum `0`.

The recursive call returns `true` from node `2` to node `11`, then to node `4`, and finally to node `5`.

### ✅ Final Output

Output:

# `true`

Because at least one root-to-leaf path has sum `22`:

\\[ \boxed{5+4+11+2=22} \\]

### 🧠 Key idea

Complexity:
- Time: \\(O(n)\\) in the worst case, because the algorithm may visit every node.
- Auxiliary space: \\(O(h)\\) for the recursion stack, where \\(h\\) is the tree height.

Interview tip: The path must start at the root and end at a leaf. Finding any group of nodes that adds up to the target is not enough.


### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Validate Binary Search Tree

**LeetCode #98** · [LeetCode](https://leetcode.com/problems/validate-binary-search-tree/) · **Medium**

> **Binary Tree · DFS · Carry `low/high` bounds down**

### Approaches

#### 1. Brute Force (Subtree min/max)
For each node, check `max(left subtree) < node < min(right subtree)`.

**Pseudo Code:**

```text
isValid(root):

    for each node:

        if max(left subtree) >= node or min(right subtree) <= node: return false

    return true
```
### Dry Run

Problem: Validate Binary Search Tree (BST)

Input: `root = [5, 1, 4, null, null, 3, 6]`

The goal is to check whether the given binary tree is a valid Binary Search Tree (BST).

A valid BST follows two rules:

- Every value in the left subtree must be less than the node's value.
- Every value in the right subtree must be greater than the node's value.

### 1. Understand the input tree

```text
        5
       / \
      1   4
         / \
        3   6
```

Notice that node `4` is the right child of `5`. Since `4 < 5`, the tree violates the BST rule.

### 2. Explain the pseudocode line by line

Line 1:

```text
isValid(root):
```

Defines a function that checks whether the tree follows the BST rules.

Line 2:

```text
for each node:
```

Visit each node in the tree and check its relationship with the values in its left and right subtrees.

Line 3:

```text
if max(left subtree) >= node:
    return false
```

- Find the maximum value in the left subtree.
- If that value is greater than or equal to the current node, the tree is invalid.
- All left-subtree values must be strictly smaller.

Line 4:

```text
if min(right subtree) <= node:
    return false
```

- Find the minimum value in the right subtree.
- If that value is less than or equal to the current node, the tree is invalid.
- All right-subtree values must be strictly greater.

Line 5:

```text
return true
```

If every node passes both checks, return `true`.

Important: These checks must apply to the entire left and right subtrees, not just the immediate children.

### 3. Dry run

Start at the root node `5`.

| Node | Left-subtree maximum | Right-subtree minimum | Result |
| :---: | :---: | :---: | :--- |
| `5` | 1 | 3 | Invalid: right-subtree minimum `3` is less than `5` |
| `1` | No children | No children | No violation |
| `4` | 3 | 6 | No local violation: `3 < 4 < 6` |

The crucial detail is that node `4` lies in the right subtree of node `5`, so it must be greater than `5`. But `4 < 5`, making the tree invalid.

The pseudocode as written would detect the violation at node `5` if it correctly calculates the maximum and minimum across the full subtrees.

### ✅ Final Output

Output:

# `false`

The tree is not a valid BST because node `4` is in the right subtree of `5`, but its value is smaller than `5`.

### 🧠 Key idea

For the given approach, repeatedly finding subtree minimums and maximums can take \\(O(n^2)\\) time in the worst case.

A more efficient standard approach passes a valid range `(min, max)` down the tree, checking each node once:
- Time: \\(O(n)\\)
- Space: \\(O(h)\\) for recursion, where \\(h\\) is the tree height.

#### 2. Optimized (DFS with bounds)
Pass valid `(low, high)` bounds down the tree. For the left child use `(low, node.val)`, and for the right child use `(node.val, high)`.

**Pseudo Code:**

```text
valid(node, low, high):

    if node is null: return true

    if not (low < node.val < high): return false

    return valid(node.left,  low, node.val)

       and valid(node.right, node.val, high)

// answer = valid(root, -inf, +inf)
```

### Dry Run

Problem: Validate Binary Search Tree using Bounds

This algorithm checks whether a binary tree is a Valid Binary Search Tree (BST).

In a BST:
- Every value in the left subtree must be smaller than the node's value.
- Every value in the right subtree must be greater than the node's value.
- These rules must hold for the entire subtree, not just the direct children.

### 1. Understand the input tree

Input: `root = [5, 3, 8, 1, 4, 6, 9]`

The tree looks like this:

```text
        5
       / \
      3   8
     / \ / \
    1  4 6  9
```

Root = 5; left subtree = 3, 1, 4; right subtree = 8, 6, 9.

### 2. Explain the pseudocode line by line

Line 1:

```text
valid(node, low, high):
```

The function receives three values:
- `node`: the current node being checked.
- `low`: the minimum allowed boundary.
- `high`: the maximum allowed boundary.

These boundaries tell us which values are allowed at the current position.

Line 2:

```text
if node is null: return true
```

If the current node does not exist, return `true`.

An empty subtree does not violate BST rules. This is the base case of recursion.

Line 3:

```text
if not (low < node.val < high): return false
```

Check whether the current node's value is strictly between `low` and `high`.

For example, if `low = 3` and `high = 5`, the current value must be greater than 3 and less than 5.
- Value `4` → valid.
- Value `3` → invalid, because it equals the lower boundary.
- Value `5` → invalid, because it equals the upper boundary.

Line 4:

```text
return valid(node.left, low, node.val)
```

Recursively check the left subtree.

The current node's value becomes the new upper boundary because every value in its left subtree must be smaller.

Line 5:

```text
and valid(node.right, node.val, high)
```

Recursively check the right subtree.

The current node's value becomes the new lower boundary because every value in its right subtree must be greater.

The `and` means both subtrees must be valid.

Final line:

```text
answer = valid(root, -inf, +inf)
```

Start at the root with no restrictions:
- `low = -∞`
- `high = +∞`

Every node must satisfy the boundaries passed down from its ancestors.

### 3. Dry run

| Node | Allowed range | Check | Result |
| :---: | :---: | :---: | :---: |
| `5` | \\((-\infty,+\infty)\\) | \\(-\infty < 5 < +\infty\\) | Valid |
| `3` | \\((-\infty,5)\\) | \\(-\infty < 3 < 5\\) | Valid |
| `1` | \\((-\infty,3)\\) | \\(-\infty < 1 < 3\\) | Valid |
| `4` | \\((3,5)\\) | \\(3 < 4 < 5\\) | Valid |
| `8` | \\((5,+\infty)\\) | \\(5 < 8 < +\infty\\) | Valid |
| `6` | \\((5,8)\\) | \\(5 < 6 < 8\\) | Valid |
| `9` | \\((8,+\infty)\\) | \\(8 < 9 < +\infty\\) | Valid |

Every node satisfies its allowed range, so both recursive calls return `true`.

### ✅ Final Output

Output:

# `true`

Why? Every value in the left subtree of `5` is less than `5`, and every value in the right subtree is greater than `5`. The same rule holds for all other nodes.

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — each node is checked once.
- Auxiliary space: \\(O(h)\\) — recursive call stack, where \\(h\\) is the tree's height. In a balanced tree, this is \\(O(\log n)\\); in a skewed tree, it can be \\(O(n)\\).

Interview tip: Passing `low` and `high` boundaries is more efficient than repeatedly finding the maximum of each left subtree and minimum of each right subtree.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force** | **O(n²)** | **O(h)** |
| **Optimized** | **O(n)** | **O(h)** |

---

# Binary Tree Tilt

**LeetCode #563** · [LeetCode](https://leetcode.com/problems/binary-tree-tilt/) · **Easy**

> **Binary Tree · Post-order DFS · Return subtree sum · Accumulate `|L − R|`**

### Approaches

#### 1. Optimized (Post-order DFS)
Recursively calculate left and right subtree sums, add `abs(leftSum - rightSum)` to the total tilt, then return the current subtree sum.

**Pseudo Code:**

```text id="f7c2q1"
total = 0

sum(node):

    if node is null: return 0

    sumL = sum(node.left)

    sumR = sum(node.right)

    total += abs(sumL - sumR)         // accumulate tilt

    return node.val + sumL + sumR     // subtree sum

// answer = total
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Diameter of a Binary Tree

**LeetCode #543** · [LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/) · **Easy**

> **Binary Tree · Post-order DFS · Height + global maximum · Widest split**

### Approaches

#### 1. Optimized (Post-order DFS)
Calculate the height of each subtree, update the diameter using `leftHeight + rightHeight`, then return `1 + max(leftHeight, rightHeight)` to the parent.

**Pseudo Code:**

```text id="q4n8z2"
best = 0

height(node):

    if node is null: return -1          // base case

    L = height(node.left)

    R = height(node.right)

    best = max(best, (L+1) + (R+1))     // path through node

    return 1 + max(L, R)                // height for parent

// answer = best
```

### Dry Run

Problem: Diameter of a Binary Tree

This algorithm finds the Diameter of a Binary Tree.

The diameter is the number of edges in the longest path between any two nodes in the tree. The path does not have to pass through the root.

### 1. Understand the input tree

Input: `root = [1, 2, 3, 4, 5]`

The tree looks like this:

```text
        1
       / \
      2   3
     / \
    4   5
```

Longest path: `4 → 2 → 1 → 3`, or `5 → 2 → 1 → 3`.

The longest path has 3 edges, so the expected diameter is `3`.

### 2. Explain the pseudocode line by line

Line 1:

```text
best = 0
```

`best` stores the maximum diameter found so far.

Initially, no path has been found, so `best = 0`.

Line 2:

```text
height(node):
```

This function calculates the height of the current node and updates `best` whenever it finds a longer path.

Line 3:

```text
if node is null: return -1
```

If the node does not exist, return `-1`.

Why `-1` instead of `0`?

Because this algorithm measures height in edges, not nodes.

For a leaf node:
- Its left child is null, so `L = -1`.
- Its right child is null, so `R = -1`.
- Its height becomes `1 + max(-1, -1) = 0`.

Therefore, a leaf has height `0` edges.

Line 4:

```text
L = height(node.left)
```

Recursively calculate the height of the left subtree.

Line 5:

```text
R = height(node.right)
```

Recursively calculate the height of the right subtree.

Line 6:

```text
best = max(best, (L+1) + (R+1))
```

Calculate the longest path that passes through the current node.

- `L + 1` = number of edges from the current node down to the deepest node on its left.
- `R + 1` = number of edges from the current node down to the deepest node on its right.
- Add both sides to get the path length through this node.

Then `max()` keeps whichever is larger: the old `best` or the newly calculated path.

Line 7:

```text
return 1 + max(L, R)
```

Return the current node's height to its parent.

We choose the taller subtree and add one edge to reach it from the current node.

Important: The function returns height, while `best` stores the diameter. They are different values.

### 3. Dry run step by step

The function processes the tree from the bottom upward because of recursion.

| Current node | `L` | `R` | Path through node `(L+1)+(R+1)` | `best` |
| :---: | :---: | :---: | :---: | :---: |
| `4` | -1 | -1 | 0 | 0 |
| `5` | -1 | -1 | 0 | 0 |
| `2` | 0 | 0 | 2 | 2 |
| `3` | -1 | -1 | 0 | 2 |
| `1` | 1 | 0 | 3 | 3 |

Let's understand the important calculations:

#### At node 2:

Its children are `4` and `5`, both leaves.
- `L = 0`, `R = 0`
- Path through node 2 = `(0+1) + (0+1) = 2`
- `best = max(0, 2) = 2`
- Height returned = `1 + max(0, 0) = 1`

#### At node 1:

Its left subtree has height `1`, and its right subtree has height `0`.
- `L = 1`, `R = 0`
- Path through node 1 = `(1+1) + (0+1) = 3`
- `best = max(2, 3) = 3`
- Height returned = `1 + max(1, 0) = 2`

The traversal is complete, and `best` contains the longest path length.

### ✅ Final Output

Output:

# `3`

The diameter is 3 edges (`answer = best = 3`).

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\) — each node is visited once.
- Auxiliary space: \\(O(h)\\) — recursion stack, where \\(h\\) is the tree height. Worst case: \\(O(n)\\).

Interview tip: The diameter is the longest path through any node, not necessarily the root. That's why we update `best` at every node instead of returning only the root's height.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Path Sum II

**LeetCode #113** · [LeetCode](https://leetcode.com/problems/path-sum-ii/) · **Medium**

> **Binary Tree · DFS + Backtracking · Collect root-to-leaf paths**

### Approaches

#### 1. Optimized (DFS + Backtracking)
Add each node to `path` while going down. When a leaf reaches the target, add a copy of the path to `results`. After exploring both children, `pop()` the node to backtrack.

**Pseudo Code:**

```text id="z7k3p1"
dfs(node, remaining):

    path.push(node.val); remaining -= node.val    // go down

    if leaf and remaining == 0: results.push(copy of path)

    dfs(node.left,  remaining)

    dfs(node.right, remaining)

    path.pop()                                    // backtrack

// answer = results
```

### Dry Run

Problem: Path Sum II

This algorithm solves Path Sum II. It finds all root-to-leaf paths whose node values add up to `targetSum`.

Unlike the previous problem, which returned only `true` or `false`, this algorithm stores every valid path in `results`.

### 1. Understand the input tree

Input: `root = [5, 4, 8, 11, null, 13, 4, 7, 2, null, null, 5, 1]`

`targetSum = 22`

The tree looks like this:

```text
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

The valid paths are highlighted in the dry run below.

### 2. Explain the pseudocode line by line

Line 1:

```text
dfs(node, remaining):
```

This function explores the tree using Depth-First Search (DFS).

- `node` is the current node.
- `remaining` is how much sum is still needed.
- `path` stores the nodes in the current path.
- `results` stores all valid paths.

Initially, `remaining = 22`, `path = []`, and `results = []`.

Line 2:

```text
path.push(node.val)
remaining -= node.val
```

Step A: `path.push(node.val)`

Add the current node's value to the path.

Step B: `remaining -= node.val`

Subtract the current node's value from the remaining target.

For example, at node `5`:

- `path = [5]`
- `remaining = 22 - 5 = 17`

Then at node `4`:

- `path = [5, 4]`
- `remaining = 17 - 4 = 13`

We keep going deeper until we reach a leaf or finish exploring that branch.

Line 3:

```text
if leaf and remaining == 0:
    results.push(copy of path)
```

Check two conditions:

1. The current node must be a leaf.
2. The remaining sum must equal `0`.

If both are true, save a copy of the current path in `results`.

Why a copy? Because `path` will change as DFS continues and backtracks. Saving a copy preserves the path exactly as it was when the answer was found.

Line 4:

```text
dfs(node.left, remaining)
```

Recursively explore the left child using the updated remaining sum.

Line 5:

```text
dfs(node.right, remaining)
```

After exploring the left side, explore the right child.

In a complete implementation, call DFS only when the child is not null, or make the function immediately return for a null node.

Line 6:

```text
path.pop()
```

This is backtracking.

Remove the current node's value from `path` after exploring its children. This allows DFS to try another branch without keeping values from the previous branch.

For example:

- Before backtracking: `path = [5, 4, 11, 7]`
- After `path.pop()`: `path = [5, 4, 11]`

Now the algorithm can explore node `2` using the correct path.

### 3. Dry run

#### Path 1: `5 → 4 → 11 → 7`

| Node | Remaining after subtraction | Leaf? | Action |
| :---: | :---: | :---: | :---: |
| **5** | 17 | No | Continue |
| **4** | 13 | No | Continue |
| **11** | 2 | No | Continue |
| **7** | -5 | Yes | Not saved |

The sum is \\(5+4+11+7=27\\), so this path is not valid.

#### Path 2: `5 → 4 → 11 → 2`

| Node | Remaining after subtraction | Leaf? | Action |
| :---: | :---: | :---: | :---: |
| **5** | 17 | No | Continue |
| **4** | 13 | No | Continue |
| **11** | 2 | No | Continue |
| **2** | 0 | Yes | Save path |

The sum is \\(5+4+11+2=22\\), so the algorithm saves a copy of `[5, 4, 11, 2]`.

#### Path 3: `5 → 8 → 4 → 5`

| Node | Remaining after subtraction | Leaf? | Action |
| :---: | :---: | :---: | :--- |
| **5** | 17 | No | Continue |
| **8** | 9 | No | Continue |
| **4** | 5 | No | Continue |
| **5** | 0 | Yes | Save path |

The sum is \\(5+8+4+5=22\\), so the algorithm saves `[5, 8, 4, 5]`.

The other root-to-leaf paths do not produce the target sum.

### ✅ Final Output

Output:

# `[[5, 4, 11, 2], [5, 8, 4, 5]]`

The order follows the DFS traversal, exploring the left subtree before the right subtree.

### 🧠 Key idea

Complexity:
- Time: \\(O(nh)\\) in the general case when copying each valid path costs up to \\(O(h)\\), plus the cost of traversing the tree. Here, \\(n\\) is the number of nodes and \\(h\\) is the tree height.
- Auxiliary space: \\(O(h)\\) for the recursion stack and current path, excluding the stored results.

Interview tip: `push()` chooses a node, recursive calls explore its descendants, and `pop()` removes the node when returning. This choose–explore–undo pattern is the core idea of backtracking.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Longest Univalue Path

**LeetCode #687** · [LeetCode](https://leetcode.com/problems/longest-univalue-path/) · **Medium**

> **Binary Tree · Post-order DFS · Equal-value arms · Global maximum**

### Approaches

#### 1. Optimized (Post-order DFS)
For each node, get the longest equal-value arm from the left and right. If a child has the same value, extend its arm by `1`. Update `best = leftArm + rightArm`, then return the longer arm to the parent.

**Pseudo Code:**

```text id="x3m8q2"
best = 0

arrow(node):

    lRaw = arrow(node.left);  rRaw = arrow(node.right)

    lArm = (left matches)  ? lRaw + 1 : 0

    rArm = (right matches) ? rRaw + 1 : 0

    best = max(best, lArm + rArm)       // path bending here

    return max(lArm, rArm)              // arm for parent

// answer = best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Invert Binary Tree

**LeetCode #226** · [LeetCode](https://leetcode.com/problems/invert-binary-tree/) · **Easy**

> **Binary Tree · Pre-order DFS · Swap left and right children**

### Approaches

#### 1. Optimized (Recursive DFS)
At each node, swap its left and right children, then recursively invert both subtrees.

**Pseudo Code:**

```text id="p5r8k2"
invert(node):

    if node is null: return            // base case

    swap node.left, node.right         // pre-order: act, then descend

    invert(node.left)

    invert(node.right)

    return                             // subtree mirrored

// answer = inverted root
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Same Tree

**LeetCode #100** · [LeetCode](https://leetcode.com/problems/same-tree/) · **Easy**

> **Binary Tree · Recursive DFS · Lockstep traversal · Compare structure + values**

### Approaches

#### 1. Optimized (Recursive Lockstep DFS)
Traverse both trees together. If both nodes are `null`, they match; if only one is `null` or their values differ, return `false`. Otherwise, compare both left and right subtrees.

**Pseudo Code:**

```text id="k2v9m4"
same(p, q):

    if p is null and q is null: return true   // both empty → fine

    if p is null or q is null:  return false  // shape differs

    if p.val != q.val:          return false  // value differs

    return same(p.left, q.left) AND same(p.right, q.right)

// answer = same(p, q)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Lowest Common Ancestor of a Binary Tree

**LeetCode #236** · [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) · **Medium**

> **Binary Tree · Post-order DFS · Bubble targets up · Split point = LCA**

### Approaches

#### 1. Optimized (Recursive DFS)
Return `p` or `q` when found. Recursively search both subtrees; if both return non-null, the current node is the LCA. Otherwise, bubble the non-null result upward.

**Pseudo Code:**

```text
lca(node):

    if node is null: return null

    if node is p or node is q: return node   // found a target

    left  = lca(node.left)

    right = lca(node.right)

    if left and right: return node           // split → LCA

    return left or right                     // carry target up

// answer = lca(root)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Binary Tree Maximum Path Sum

**LeetCode #124** · [LeetCode](https://leetcode.com/problems/binary-tree-maximum-path-sum/) · **Hard**

> **Binary Tree · Post-order DFS · Maximum gain · Clamp negative arms**

### Approaches

#### 1. Optimized (Post-order DFS)
Calculate the maximum gain from each subtree, treating negative gains as `0`. Update the global maximum using `node.val + leftGain + rightGain`, then return only the larger arm to the parent.

**Pseudo Code:**

```text id="k7p3x9"
best = −∞

gain(node):

    if node is null: return 0

    L = max(gain(node.left),  0)   // drop negative arms

    R = max(gain(node.right), 0)

    best = max(best, node.val + L + R)   // bend here

    return node.val + max(L, R)          // extend parent

// answer = best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(h)** |

---

# Serialize and Deserialize Binary Tree

**LeetCode #297** · [LeetCode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) · **Hard**

> **Binary Tree · Pre-order traversal · Null markers · Rebuild from stream**

### Approaches

#### 1. Optimized (Serialize)
Traverse in pre-order and store each node value. Use a null marker such as `#` for missing children so the tree structure remains unambiguous.

**Pseudo Code:**

```text id="n4k8p2"
serialize(node):

    if node: append(node.val)         // record on arrival (pre-order)

    else:    append("#"); return      // null marker keeps it unambiguous

    serialize(node.left); serialize(node.right)

// result = "1,2,#,#,3,4,#,#,5,#,#"
```

#### 2. Optimized (Deserialize)
Read the values in the same pre-order sequence. For each value, create a node; for `#`, return `null`, then recursively build the left and right subtrees.

**Pseudo Code:**

```text id="v6m2q9"
build():

    token = next()                    // consume left→right

    if token == "#": return null      // empty subtree

    node = new Node(token)

    node.left = build(); node.right = build()

    return node                       // first call returns the root
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Serialize** | **O(n)** | **O(n)** |
| **Deserialize** | **O(n)** | **O(n)** |

---
