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
