# Course Schedule

**LeetCode #207** · [LeetCode](https://leetcode.com/problems/course-schedule/) · **Medium**

> **Graph · Topological Sort · Kahn’s Algorithm · In-degree**

### Approaches

#### 1. Optimized (Kahn’s Algorithm)
Start with courses having `in-degree = 0`, process them and reduce the in-degree of their neighbors; if all courses are processed, there is no cycle.

**Pseudo Code:**

```text
build graph; edge b → a for prereq [a, b]

in-deg[v] = number of edges into v

queue = all v with in-deg[v] == 0

processed = 0

while queue not empty:

    u = pop; processed += 1

    for u → v: in-deg[v] -= 1; if 0: push v

return processed == numCourses   // all drained = DAG
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(V + E)** | **O(V + E)** |

---

# Course Schedule II

**LeetCode #210** · [LeetCode](https://leetcode.com/problems/course-schedule-ii/) · **Medium**

> **Graph · Topological Sort · Kahn’s Algorithm · Collect order**

### Approaches

#### 1. Optimized (Kahn’s Algorithm)
Start with all courses having `in-degree = 0`, process them and append each to `order`; if `order` contains all courses, it is a valid topological order, otherwise a cycle exists.

**Pseudo Code:**

```text
build graph; edge b → a for prereq [a, b]

in-deg[v] = number of edges into v

queue = all v with in-deg[v] == 0

order = []

while queue not empty:

    u = pop; order.append(u)

    for u → v: in-deg[v] -= 1; if 0: push v

return len(order) == numCourses ? order : []
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(V + E)** | **O(V + E)** |

---
---

# Shortest Path Algorithms

**Concept** · BFS vs Dijkstra vs Bellman-Ford

> **Graph · Shortest Path · Choose by edge weights**

### Approaches

#### 1. Concept
**BFS:** Use for **unweighted graphs** — finds the shortest path by fewest edges.

**Dijkstra:** Use when **all edge weights ≥ 0** — greedily processes the nearest node using a min-heap.

**Bellman-Ford:** Use when **negative edge weights** are possible — relax all edges `V−1` times and detect negative cycles with one extra pass.

**Pseudo Code:**

```text
// choose by the edge weights:

unweighted          -> BFS          (fewest edges)

weights all >= 0    -> Dijkstra     (greedy, heap)

any negative weight -> Bellman-Ford (relax all, V-1x)

Dijkstra: dist[src]=0; pop nearest; finalize;

          relax u->v: if d+w < dist[v]: update

Bellman-Ford: relax EVERY edge, V-1 times;

          one more pass relaxes => negative cycle
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **BFS** | **O(V + E)** | **O(V)** |
| **Dijkstra** | **O(E log V)** | **O(V)** |
| **Bellman-Ford** | **O(V · E)** | **O(V)** |

---
---

# Network Delay Time

**LeetCode #743** · [LeetCode](https://leetcode.com/problems/network-delay-time/) · **Medium**

> **Graph · Dijkstra · Min-heap · Finalize nearest unfinished node**

### Approaches

#### 1. Optimized (Dijkstra)
Start with `dist[source] = 0`, repeatedly process the unvisited node with minimum distance, relax its outgoing edges, and take the maximum shortest distance. If any node remains unreachable, return `-1`.

**Pseudo Code:**

```text
given graph, source

dist[source] = 0, rest = ∞; pq = {(0, source)}

while pq not empty:

    (d, u) = pop min

    if u already done: skip

    mark u done

    for (u → v, w):

        if d + w < dist[v]: dist[v] = d+w; push

answer = max(dist)   // −1 if any unreachable
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(E log V)** | **O(V + E)** |

---

# Cheapest Flights Within K Stops

**LeetCode #787** · [LeetCode](https://leetcode.com/problems/cheapest-flights-within-k-stops/) · **Medium**

> **Graph · Bounded Bellman-Ford · K+1 hops · Limited relaxations**

### Approaches

#### 1. Optimized (Bounded Bellman-Ford)
Run Bellman-Ford for **K+1 rounds**, using a copy of the previous distances in each round so that each round adds at most one hop. Return `dist[dst]`, or `-1` if unreachable.

**Pseudo Code:**

```text
dist = [∞...]; dist[src] = 0

repeat K+1 times:

    prev = copy(dist)        // freeze last round

    for each (u → v, w):

        if prev[u] + w < dist[v]:

            dist[v] = prev[u] + w

    // each round adds at most one hop

return dist[dst] (or −1 if ∞)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(K · E)** | **O(V)** |

---

# Path With Minimum Effort

**LeetCode #1631** · [LeetCode](https://leetcode.com/problems/path-with-minimum-effort/) · **Medium**

> **Graph · Dijkstra · Minimize maximum edge · Grid**

### Approaches

#### 1. Optimized (Dijkstra)
Treat the effort to reach a cell as the **maximum height difference along the path**. For each neighbor, calculate `max(currentEffort, |height difference|)` and use the minimum-effort priority queue to always process the smallest effort first.

**Pseudo Code:**

```text
effort[*] = ∞; effort[0][0] = 0

pq = {(0, (0,0))}

while pq not empty:

    (e, cell) = pop min effort

    if cell settled: skip; mark settled

    if cell == target: return e

    for each neighbour:

        cand = max(e, |Δheight|)

        if cand < effort[nbr]: update; push
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(R · C · log(R · C))** | **O(R · C)** |

---

# Find the City With Fewest Reachable

**LeetCode #1334** · [LeetCode](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) · **Medium**

> **Graph · Floyd-Warshall · All-pairs shortest paths · Count reachable cities**

### Approaches

#### 1. Optimized (Floyd-Warshall)
Build the distance matrix, use every city as an intermediate node to find the shortest distance between every pair, then count cities within the threshold. If counts tie, choose the **largest index**.

**Pseudo Code:**

```text id="2k7v9d"
dist[i][j] = w (edges), 0 (i==j), ∞ else

for k in 0..n-1:

    for i, j:

        if dist[i][k] + dist[k][j] < dist[i][j]:

            dist[i][j] = dist[i][k] + dist[k][j]

// score cities

for each city i:

    count j with dist[i][j] <= threshold

pick fewest count; tie → largest index
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(V³)** | **O(V²)** |

---

# Union-Find (DSU)

**Concept** · Disjoint Set Union

> **Graph · DSU · Find + Union · Connected components**

### Approaches

#### 1. Optimized (DSU)
Maintain a `parent` array where each node starts as its own set. `find(x)` returns the set representative, while `union(a,b)` merges two different sets. With **path compression + union by rank/size**, each operation is approximately **O(α(n))**.

**Pseudo Code:**

```text id="q4x7mv"
parent[i] ← i           // each node its own set

find(x): while parent[x] != x: x = parent[x]; return x

union(a, b):

    ra, rb ← find(a), find(b)

    if ra != rb: parent[rb] = ra

// same set? ⇔ find(a) == find(b);  #sets = distinct roots
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **~O(α(n)) per operation** | **O(n)** |

---

# Number of Connected Components

**LeetCode #323** · [LeetCode](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) · **Medium**

> **Graph · Union-Find (DSU) · Count components · Merge decreases count**

### Approaches

#### 1. Optimized (Union-Find)
Start with `count = n` because every node is initially its own component. For each edge, if the two nodes have different roots, union them and decrement `count`. At the end, `count` is the number of connected components.

**Pseudo Code:**

```text id="7k3mqp"
parent[i] ← i; count ← n          // each node its own component

find(x): while parent[x] != x: x = parent[x]; return x

for (a, b) in edges:

    ra, rb ← find(a), find(b)

    if ra != rb: parent[rb] = ra; count--   // merge two components

    // else: same component → edge redundant, count unchanged

return count                            // # connected components
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O((n + E) · α(n))** | **O(n)** |

---

# Redundant Connection

**LeetCode #684** · [LeetCode](https://leetcode.com/problems/redundant-connection/) · **Medium**

> **Graph · Union-Find (DSU) · Cycle detection · First cycle-closing edge**

### Approaches

#### 1. Optimized (Union-Find)
For each edge, check whether both nodes already have the same root. If yes, the edge creates a cycle, so return it; otherwise, union the two sets.

**Pseudo Code:**

```text id="v8q2lm"
parent[i] ← i                       // each node its own set

find(x): while parent[x] != x: x = parent[x]; return x

for (a, b) in edges:

    if find(a) == find(b): return [a, b]   // same set → cycle

    parent[find(b)] = find(a)              // union

// first same-set edge is the redundant one
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n · α(n))** | **O(n)** |

---

# Word Ladder

**LeetCode #127** · [LeetCode](https://leetcode.com/problems/word-ladder/) · **Hard**

> **Graph · BFS · Shortest transformation sequence · One-letter changes**

### Approaches

#### 1. Optimized (BFS)
Treat each valid word as a graph node, where two words are connected if they differ by one letter. Start BFS from `beginWord`; the first time a word is reached gives the shortest transformation length.

**Pseudo Code:**

```text id="x5n8qp"
build graph: edge(a, b) if a, b differ by one letter

queue = [beginWord]; dist[beginWord] = 1; visited = {beginWord}

while queue: u = queue.pop_front()          // dequeue (level order)

    for v in neighbours(u):

        if v in visited: continue           // already shortest

        dist[v] = dist[u] + 1; enqueue v    // first time = shortest

    if u == endWord: return dist[u]         // BFS ⇒ shortest ladder

return 0                                    // unreachable
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(N · L · 26)** | **O(N · L)** |
