# Table of Contents

- [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)

---

# Best Time to Buy and Sell Stock

**LeetCode #121** · [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) · **Easy**

**Array · Greedy · One pass · Track minimum price so far**

### Tip

**Optimized · Greedy:** Track the cheapest price seen so far. For each day, calculate the profit if selling today and update the maximum profit.

**Pseudo Code:**

```text
given prices

min_so_far ← prices[0]

max_profit ← 0

for i ← 1 to n − 1:

    max_profit = max(max_profit, prices[i] − min_so_far)

    min_so_far = min(min_so_far, prices[i])

return max_profit
```

### Dry Run
Sure. This is the basic greedy approach for **Best Time to Buy and Sell Stock**.

Given:

```text
prices = [1,2,3,0,2]
```

The goal is to find the **maximum profit from one buy and one sell**, where you must buy before selling.

---

## 1. `min_so_far ← prices[0]`

```text
min_so_far = prices[0]
```

`prices[0] = 1`

So:

```text
min_so_far = 1
```

Meaning:

> So far, the cheapest price at which we could have bought is `1`.

---

## 2. `max_profit ← 0`

```text
max_profit = 0
```

Initially, we haven't made any profit.

---

## 3. Loop

```text
for i ← 1 to n − 1:
```

We start from index `1` because index `0` was already used to initialize `min_so_far`.

Our array:

```text
Index:    0  1  2  3  4
Price:    1  2  3  0  2
```

---

### 🔹 Iteration 1 — `i = 1`

```text
prices[1] = 2
```

First:

```text
max_profit = max(max_profit, prices[i] - min_so_far)
```

Substitute:

```text
max_profit = max(0, 2 - 1)
```

```text
max_profit = max(0, 1)
           = 1
```

So our current maximum profit is:

```text
max_profit = 1
```

Now update the minimum price:

```text
min_so_far = min(min_so_far, prices[i])
```

```text
min_so_far = min(1,2)
            = 1
```

So:

```text
min_so_far = 1
max_profit = 1
```

---

### 🔹 Iteration 2 — `i = 2`

```text
prices[2] = 3
```

Calculate possible profit:

```text
3 - 1 = 2
```

So:

```text
max_profit = max(1,2)
           = 2
```

Update minimum:

```text
min_so_far = min(1,3)
           = 1
```

State:

```text
min_so_far = 1
max_profit = 2
```

This corresponds to:

```text
Buy at 1
Sell at 3
Profit = 3 - 1 = 2
```

---

### 🔹 Iteration 3 — `i = 3`

Now:

```text
prices[3] = 0
```

Calculate possible profit:

```text
0 - 1 = -1
```

So:

```text
max_profit = max(2,-1)
           = 2
```

We don't want a negative profit.

Now update minimum price:

```text
min_so_far = min(1,0)
           = 0
```

So:

```text
min_so_far = 0
max_profit = 2
```

This is important.

We discovered a cheaper buying price:

```text
Buy at 0
```

---

### 🔹 Iteration 4 — `i = 4`

```text
prices[4] = 2
```

Calculate:

```text
2 - min_so_far
= 2 - 0
= 2
```

Then:

```text
max_profit = max(2,2)
           = 2
```

Update minimum:

```text
min_so_far = min(0,2)
           = 0
```

Final state:

```text
min_so_far = 0
max_profit = 2
```

---

## Complete dry run

| `i` | Price | `min_so_far` before | Profit `price - min` | `max_profit` | `min_so_far` after |
| --: | ----: | ------------------: | -------------------: | -----------: | -----------------: |
|   — |   `1` |                   — |                    — |          `0` |                `1` |
|   1 |   `2` |                 `1` |                  `1` |          `1` |                `1` |
|   2 |   `3` |                 `1` |                  `2` |          `2` |                `1` |
|   3 |   `0` |                 `1` |                 `-1` |          `2` |                `0` |
|   4 |   `2` |                 `0` |                  `2` |          `2` |                `0` |

### ✅ Final output

```text
2
```

The best transaction is:

```text
Buy  → 0
Sell → 2

Profit = 2 - 0 = 2
```

### 🧠 Easy way to remember the logic

At every price, ask two questions:

```text
1. What is the cheapest price I've seen so far?
2. If I sell TODAY, how much profit can I make?
```

That's exactly what these two lines do:

```text
max_profit = max(max_profit, prices[i] - min_so_far)

min_so_far = min(min_so_far, prices[i])
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Greedy** | **O(n)** | **O(1)** |

----


