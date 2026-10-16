# Table of Contents

- [Level Order Sum](#level-order-sum)
- [Rightmost Node](#rightmost-node)
- [Zigzag Level Order](#zigzag-level-order)
- [Maximum Width of Binary Tree](#maximum-width-of-binary-tree)
- [Minimum Knight Moves](#minimum-knight-moves)
- [Rotting Oranges](#rotting-oranges)
- [01 Matrix](#01-matrix)
- [Bus Routes](#bus-routes)

---

# Level Order Sum

**LeetCode #1161** · [LeetCode](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/) · **Medium**

> **Binary Tree · BFS · Level-order traversal · Sum each level**

### Approaches

#### 1. Optimized (BFS by level)
Use a queue and process exactly `levelSize` nodes at a time; calculate each level's sum and keep the level with the maximum sum.

**Pseudo Code:**

```text id="2k9fva"
levelSums(root):

    queue = [root]

    while queue not empty:

        levelSize = len(queue)

        sum = 0

        for _ in range(levelSize):

            node = queue.dequeue()

            sum += node.val

            enqueue node.children

        levelSums.append(sum)

    return levelSums   // max() for heaviest level
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(width)** |

---

# Rightmost Node

**LeetCode #199** · [LeetCode](https://leetcode.com/problems/binary-tree-right-side-view/) · **Medium**

> **Binary Tree · BFS · Level-order traversal · Last node of each level**

### Approaches

#### 1. Optimized (BFS by level)
Process each level using `levelSize`; the last node dequeued from that level is the node visible from the right side.

**Pseudo Code:**

```text id="z7n4qc"
rightSideView(root):

    queue = [root]

    while queue not empty:

        levelSize = len(queue)

        for i in range(levelSize):

            node = queue.dequeue()

            if i == levelSize - 1: rightView.append(node.val)

            enqueue node.children

    return rightView
```
### Dry Run

Problem: Binary Tree Right Side View

Input: `root = [1, 2, 3, 4, null, null, null, 5]`

The goal is to return the nodes visible when looking at a binary tree from its right side.

### 1. Understand the input tree

The rightmost visible node at each level is selected.

The tree is:

```text
        1
       / \
      2   3
     /
    4
   /
  5
```

The `null` values mean those child nodes do not exist.

### 2. Explain the pseudocode line by line

Line 1:

```text
rightSideView(root):
```

Defines a function that returns the values visible from the right side of the tree.

Line 2:

```text
queue = [root]
```

- Create a queue containing the root node, `1`.
- A queue follows FIFO: First In, First Out.
- This allows us to visit the tree level by level.

Line 3:

```text
while queue not empty:
```

Continue until there are no more nodes to process.

Line 4:

```text
levelSize = len(queue)
```

Save the number of nodes currently in the queue. This tells us how many nodes belong to the current level.

Line 5:

```text
for i in range(levelSize):
```

Process exactly `levelSize` nodes for this level. In Python-style indexing, `i` starts at `0`.

Line 6:

```text
node = queue.dequeue()
```

Remove the first node from the queue.

Line 7:

```text
if i == levelSize - 1:
    rightView.append(node.val)
```

- Check whether this is the last node being processed at the current level.
- If yes, add its value to `rightView`.
- This works because nodes are processed from left to right, so the last node is the rightmost node at that level.

Line 8:

```text
enqueue node.children
```

Add the node's existing children to the queue, normally left child first and then right child. Ignore any `null` children.

Line 9:

```text
return rightView
```

Return the collected rightmost values.

### 3. Dry run

Initially:

`queue = [1]`, `rightView = []`

| Level | Nodes processed | `levelSize` | Selected node | `rightView` |
| :---: | :---: | :---: | :---: | :--- |
| **1** | `[1]` | 1 | 1 | `[1]` |
| **2** | `[2, 3]` | 2 | 3 | `[1, 3]` |
| **3** | `[4]` | 1 | 4 | `[1, 3, 4]` |
| **4** | `[5]` | 1 | 5 | `[1, 3, 4, 5]` |

Notice that at Level 2, node `2` is processed first and node `3` last. Therefore, `3` is selected.

### ✅ Final Output

Output:

# `[1, 3, 4, 5]`

### 🧠 Key idea

Time and space complexity:
- Time complexity: \\(O(n)\\) — every node is visited once.
- Space complexity: \\(O(n)\\) — the queue can hold up to a level's worth of nodes, and the result stores the visible nodes.

Interview tip: The key condition is `i == levelSize - 1`. It selects the last node at each level, provided children are enqueued from left to right.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(width)** |

---

# Zigzag Level Order

**LeetCode #103** · [LeetCode](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) · **Medium**

> **Binary Tree · BFS · Level-order traversal · Alternate direction**

### Approaches

#### 1. Optimized (BFS + direction flag)
Process each level normally with a queue, then reverse the collected values on alternate levels.

**Pseudo Code:**

```text id="q8m3kf"
zigzag(root):

    queue = [root]; leftToRight = true

    while queue not empty:

        levelSize = len(queue)

        collected = []

        for _ in range(levelSize):

            node = queue.dequeue()

            collected.append(node.val)

            enqueue node.children

        result.append(collected if leftToRight else reversed(collected))

        leftToRight = not leftToRight

    return result
```
### Dry Run

Problem: Binary Tree Zigzag Level Order Traversal

Input: `root = [3, 9, 20, null, null, 15, 7]`

The goal is to traverse a binary tree level by level, but alternate the direction at each level:

- Level 1: left to right
- Level 2: right to left
- Level 3: left to right
- Continue alternating.

### 1. Understand the input tree

The tree looks like this:

```text
        3
       / \
      9   20
          / \
         15  7
```

### 2. Explain the pseudocode line by line

Line 1:

```text
zigzag(root):
    queue = [root]
    leftToRight = true
```

- Create a queue starting with the root node `3`.
- `leftToRight = true` means the first level will be read from left to right.

Line 2:

```text
while queue not empty:
```

Keep processing levels until the queue becomes empty.

Line 3:

```text
levelSize = len(queue)
```

Count how many nodes are currently in the queue. This tells us how many nodes belong to the current level.

Line 4:

```text
collected = []
```

Create an empty list to store the values of the current level.

Line 5:

```text
for _ in range(levelSize):
```

Process exactly `levelSize` nodes.

The underscore `_` means we don't need the loop counter.

Line 6:

```text
node = queue.dequeue()
```

Remove the first node from the queue.

Line 7:

```text
collected.append(node.val)
```

Add the current node's value to `collected`.

Line 8:

```text
enqueue node.children
```

Add the node's existing children to the queue, left child first and then right child. Ignore `null` children.

Line 9:

```text
result.append(
    collected if leftToRight else reversed(collected)
)
```

This is the main zigzag logic:

- If `leftToRight` is `true`, append `collected` as it is.
- If `leftToRight` is `false`, append the reversed list.

Line 10:

```text
leftToRight = not leftToRight
```

Switch the direction for the next level:

- `true` becomes `false`.
- `false` becomes `true`.

Line 11:

```text
return result
```

Return all the levels in zigzag order.

### 3. Dry run step by step

Initial state:

```text
queue = [3]
leftToRight = true
result = []
```

#### Level 1 (Left → Right)

- Node processed: `3`
- `levelSize = 1`
- `collected = [3]`
- Direction is left to right, so append `[3]`.
- Toggle direction to `false`.

`result = [[3]]`

#### Level 2 (Right → Left)

- Nodes processed: `9`, `20`
- `levelSize = 2`
- Process nodes `9` and `20`.
- `collected = [9, 20]`
- Direction is right to left, so reverse it to `[20, 9]`.
- Toggle direction to `true`.

`result = [[3], [20, 9]]`

#### Level 3 (Left → Right)

- Nodes processed: `15`, `7`
- `levelSize = 2`
- Process nodes `15` and `7`.
- `collected = [15, 7]`
- Direction is left to right, so keep the list unchanged.

`result = [[3], [20, 9], [15, 7]]`

The queue is now empty, so the loop stops.

### ✅ Final Output

Output:

# `[[3], [20, 9], [15, 7]]`

### 🧠 Key idea

Time and space complexity:
- Time complexity: \\(O(n)\\) for visiting every node, plus the cost of reversing levels. Overall, it remains \\(O(n)\\).
- Space complexity: \\(O(n)\\) for the queue, level lists, and result.

Interview tip: The queue handles normal level-order traversal. The `leftToRight` flag and list reversal are what make the traversal zigzag.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(width)** |

---

# Maximum Width of Binary Tree

**LeetCode #662** · [LeetCode](https://leetcode.com/problems/maximum-width-of-binary-tree/) · **Medium**

> **Binary Tree · BFS · Position indexing · Left = 2i, Right = 2i + 1**

### Approaches

#### 1. Optimized (BFS + position indices)
Give each node a complete-tree index, then for each level calculate `rightIndex - leftIndex + 1`; normalize indices to avoid large values.

**Pseudo Code:**

```text id="m2x7kp"
widthOfTree(root):

    queue = [(root, 0)]

    while queue not empty:

        base = queue.front.index   // normalize

        width = queue.back.index - base + 1

        maxWidth = max(maxWidth, width)

        for _ in range(len(queue)):

            node, i = queue.dequeue()   // i already normalized

            enqueue (node.left, 2*i), (node.right, 2*i+1)

    return maxWidth
```
### Dry Run

Problem: Maximum Width of a Binary Tree

Input: `root = [1, 3, 2, 5, null, null, 9, 6, null, 7]`

The goal is to find the maximum width at any level of the binary tree.

Width includes both the leftmost and rightmost non-null nodes, plus any empty positions between them, as if the tree were a complete binary tree.

### 1. Understand the input tree

The level-order input represents this tree:

The tree is:

```text
          1
        /   \
       3     2
      /       \
     5         9
    /         /
   6         7
```

Notice that some positions are empty. Those gaps matter when calculating the width.

### 2. Explain the pseudocode line by line

Line 1:

```text
widthOfTree(root):
    queue = [(root, 0)]
```

- Create a queue containing the root node and its index.
- The root starts at index `0`.
- Each queue entry stores `(node, index)`.

Line 2:

```text
while queue not empty:
```

Continue processing the tree level by level until the queue is empty.

Line 3:

```text
base = queue.front.index
```

- Get the index of the first node at the current level.
- This is used to normalize indices so they don't grow too large for deep trees.

Line 4:

```text
width = queue.back.index - base + 1
```

- `queue.back.index` is the index of the last node at the current level.
- Subtract `base` to measure the distance between the first and last positions.
- Add `1` because both endpoints count.

For example, if the first node has index `0` and the last has index `3`, width is `3 - 0 + 1 = 4`, even if two positions between them are empty.

Line 5:

```text
maxWidth = max(maxWidth, width)
```

- Compare the current level's width with the largest width found so far.
- Keep whichever is greater.
- Initialize `maxWidth = 0` before the loop.

Line 6:

```text
for _ in range(len(queue)):
```

Process only the nodes that were in the queue at the start of this level. Save that length before the loop because new children will be added.

Line 7:

```text
node, i = queue.dequeue()
```

Remove a node and its index from the queue.

Important correction: The pseudocode needs to normalize the index before calculating child indices:

```text
i = i - base
```

This step is missing from the supplied pseudocode, despite the comment saying that `i` is already normalized.

Line 8:

```text
enqueue (node.left, 2*i), (node.right, 2*i+1)
```

Use these formulas to calculate the children's positions:

- Left child: `2 * i`
- Right child: `2 * i + 1`

Only enqueue children that are not `null`.

Line 9:

```text
return maxWidth
```

Return the largest width found across all levels.

### 3. Dry run step by step

Start with:

```text
queue = [(1, 0)]
maxWidth = 0
```

#### Level 1

Queue: `[(1, 0)]`

- First index = `0`, last index = `0`
- Width = `0 - 0 + 1 = 1`
- `maxWidth = 1`
- Children: `3` at index `0`, `2` at index `1`

#### Level 2

Queue: `[(3, 0), (2, 1)]`

- First index = `0`, last index = `1`
- Width = `1 - 0 + 1 = 2`
- `maxWidth = 2`
- The existing children are `5` at index `0` and `9` at index `3`.

#### Level 3

Queue: `[(5, 0), (9, 3)]`

- First index = `0`, last index = `3`
- Width = `3 - 0 + 1 = 4`
- `maxWidth = 4`
- Node `5` has child `6` at index `0`.
- Node `9` has child `7` at index `6`.

The positions at this level span indices 0 through 3, including empty positions.

#### Level 4

Queue: `[(6, 0), (7, 6)]`

- First index = `0`, last index = `6`
- Width = `6 - 0 + 1 = 7`
- `maxWidth = 7`

The queue becomes empty after processing these leaf nodes.

### 4. Width summary

| Level | Node values | Indexed positions | Width |
| :---: | :---: | :---: | :---: |
| **1** | `[1]` | `0` to `0` | 1 |
| **2** | `[3, 2]` | `0` to `1` | 2 |
| **3** | `[5, 9]` | `0` to `3` | 4 |
| **4** | `[6, 7]` | `0` to `6` | 7 |

### ✅ Final Output

Output:

# `7`

The maximum width occurs at Level 4. Although that level contains only two actual nodes, their positions are far apart, with empty positions between them.

### 🧠 Key idea

Time and space complexity:
- Time: \\(O(n)\\), because each node is processed once.
- Space: \\(O(n)\\) in the worst case for the queue.

Interview tip: Maximum width is not the same as the number of nodes at a level. You must count the gaps between the leftmost and rightmost positions too.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(width)** |

---

# Minimum Knight Moves

**LeetCode #1197** · [LeetCode](https://leetcode.com/problems/minimum-knight-moves/) · **Medium**

> **Graph · BFS · Shortest path · 8 knight moves**

### Approaches

#### 1. Optimized (BFS)
Treat each board position as a graph node; from each position explore the 8 possible knight moves. The first time the target is reached gives the minimum moves.

**Pseudo Code:**

```text id="j4k8px"
queue = [start]; dist(start) = 0

while queue not empty:

    cell = queue.dequeue()

    if cell == target: return dist(cell)

    for each of the 8 knight moves:

        if neighbour in bounds and unvisited:

            dist = dist(cell) + 1; enqueue it

return -1   // unreachable
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(V)** | **O(V)** |

---

# Rotting Oranges

**LeetCode #994** · [LeetCode](https://leetcode.com/problems/rotting-oranges/) · **Medium**

> **Grid · Multi-source BFS · Level-by-level spread · Minutes**

### Approaches

#### 1. Optimized (Multi-source BFS)
Start BFS with all rotten oranges; process one level per minute, rot adjacent fresh oranges, and count remaining fresh oranges to detect unreachable ones.

**Pseudo Code:**

```text id="q7n2mv"
queue = all rotten cells    // multi-source seed

fresh = count of 1s

minute = 0

while queue not empty and fresh > 0:

    minute++

    for each cell in this minute’s batch:

        for each fresh neighbour:

            rot it, fresh--, enqueue it

    queue = newly rotted

return fresh == 0 ? minute : -1
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(R·C)** | **O(R·C)** |

---

# 01 Matrix

**LeetCode #542** · [LeetCode](https://leetcode.com/problems/01-matrix/) · **Medium**

> **Grid · Multi-source BFS · Distance to nearest 0**

### Approaches

#### 1. Optimized (Multi-source BFS)
Start BFS from all `0` cells with distance `0`; expand level by level and assign each unvisited `1` cell the distance of the current BFS level.

**Pseudo Code:**

```text id="n5r8kc"
dist = grid; mark 1-cells unknown

queue = all 0-cells    // multi-source seed, distance 0

ring = 0

while queue not empty:

    ring++

    for each cell in this frontier:

        for each unknown neighbour:

            dist = ring; enqueue it

    queue = next frontier

return dist
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(R·C)** | **O(R·C)** |

---

# Bus Routes

**LeetCode #815** · [LeetCode](https://leetcode.com/problems/bus-routes/) · **Hard**

> **Graph · BFS · Routes as states · Each level = one more bus**

### Approaches

#### 1. Optimized (BFS over routes)
Map each stop to the routes serving it, start with routes from `source`, and use BFS to switch between routes sharing a stop; the first route containing `target` gives the minimum buses.

**Pseudo Code:**

```text
numBusesToDest(routes, source, target):

    build stopToRoutes: stop -> [route ids]

    if source == target: return 0

    queue = routes serving source, each with buses = 1

    visited = those routes

    while queue not empty:

        route = queue.dequeue()      // front

        if target in route: return buses[route]

        for stop in route:

            for r in stopToRoutes[stop]:

                if r not visited:

                    buses[r] = buses[route] + 1; enqueue r

    return -1
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(S²)** | **O(N + S)** |
