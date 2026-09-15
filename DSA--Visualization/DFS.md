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
