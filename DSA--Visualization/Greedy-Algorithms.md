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


---
## Doubt 1:
Yes. Actually, **your existing pseudocode already satisfies that condition**.

Your condition is:

> Choose one day to buy and a **different day in the future** to sell.

Your pseudocode:

```text
min_so_far ← prices[0]
max_profit ← 0

for i ← 1 to n − 1:
    max_profit = max(max_profit, prices[i] − min_so_far)
    min_so_far = min(min_so_far, prices[i])

return max_profit
```

### Why does it already guarantee a future selling day?

The key is the **order of these two lines**:

```text
max_profit = max(max_profit, prices[i] − min_so_far)
min_so_far = min(min_so_far, prices[i])
```

At day `i`:

1. First, calculate the profit using `min_so_far`.
2. Only **after calculating profit**, add today's price to `min_so_far`.

So today's price cannot become the buying price for today's sell.

### Example

```text
prices = [5, 1, 4]
```

At `i = 1`:

```text
min_so_far = 5

profit = 1 - 5 = -4
```

Then:

```text
min_so_far = min(5,1)
           = 1
```

At `i = 2`:

```text
profit = 4 - 1
       = 3
```

So:

```text
Buy  → day 1 (price 1)
Sell → day 2 (price 4)
Profit = 3
```

Different days, and selling is in the future. ✅

---

### ⚠️ Don't change the order

If you wrote:

```text
for i ← 1 to n − 1:
    min_so_far = min(min_so_far, prices[i])
    max_profit = max(max_profit, prices[i] − min_so_far)
```

then today's price could become `min_so_far` **before** calculating today's profit.

For example:

```text
prices = [5, 1]
```

You would get:

```text
min_so_far = 1
profit = 1 - 1 = 0
```

That represents buying and selling on the same day, which violates your condition.

### So the answer is:

**Change nothing.** Your original pseudocode is already correct for:

```text
Buy on one day
        ↓
Sell on a different day
        ↓
Selling day must be later
```

The critical rule is:

```text
✅ calculate profit FIRST
✅ update min_so_far SECOND
```

That ordering is what guarantees the buy happens before the sell.

------
---
Sure. Let's dry-run your exact pseudocode:

```text id="zv2m9s"
min_so_far ← prices[0]
max_profit ← 0

for i ← 1 to n − 1:
    max_profit = max(max_profit, prices[i] − min_so_far)
    min_so_far = min(min_so_far, prices[i])

return max_profit
```

Input:

```text id="p8kq5v"
prices = [7,1,5,3,6,4]
```

### Initial values

```text id="w5qg7c"
min_so_far = 7
max_profit = 0
```

---

### i = 1

Price:

```text id="l9b0v8"
prices[1] = 1
```

Calculate profit:

```text id="g6r2xq"
1 - 7 = -6
```

```text id="5x9p3a"
max_profit = max(0,-6) = 0
```

Update minimum:

```text id="f8k2lw"
min_so_far = min(7,1) = 1
```

State:

```text id="7q1n4c"
min_so_far = 1
max_profit = 0
```

---

### i = 2

Price:

```text id="b4c7m2"
prices[2] = 5
```

Profit:

```text id="0z8s6k"
5 - 1 = 4
```

```text id="5e2j9x"
max_profit = max(0,4) = 4
```

Update minimum:

```text id="r3k6v1"
min_so_far = min(1,5) = 1
```

State:

```text id="s8q2m4"
min_so_far = 1
max_profit = 4
```

This means:

```text id="1x4q7m"
Buy at 1
Sell at 5
Profit = 4
```

---

### i = 3

Price:

```text id="n7c3p9"
prices[3] = 3
```

Profit:

```text id="j5w8r2"
3 - 1 = 2
```

```text id="a9k1v6"
max_profit = max(4,2) = 4
```

Update minimum:

```text id="d2f7m8"
min_so_far = min(1,3) = 1
```

State:

```text id="q4s8n1"
min_so_far = 1
max_profit = 4
```

---

### i = 4

Price:

```text id="c6m2x8"
prices[4] = 6
```

Profit:

```text id="w1r5k9"
6 - 1 = 5
```

Update:

```text id="e7p3v4"
max_profit = max(4,5) = 5
```

Minimum:

```text id="t8n2q6"
min_so_far = min(1,6) = 1
```

State:

```text id="h3k7s1"
min_so_far = 1
max_profit = 5
```

This is currently the best:

```text id="x6p2m9"
Buy at 1
Sell at 6
Profit = 5
```

---

### i = 5

Price:

```text id="u4c8z2"
prices[5] = 4
```

Profit:

```text id="k9m3r7"
4 - 1 = 3
```

```text id="v2q6s8"
max_profit = max(5,3) = 5
```

Update minimum:

```text id="f1n5x9"
min_so_far = min(1,4) = 1
```

Final:

```text id="m7c2p4"
min_so_far = 1
max_profit = 5
```

---

## Dry-run table

|  i | Price | `min_so_far` before | Profit | `max_profit` | `min_so_far` after |
| -: | ----: | ------------------: | -----: | -----------: | -----------------: |
|  — |     7 |                   — |      — |            0 |                  7 |
|  1 |     1 |                   7 |     -6 |            0 |                  1 |
|  2 |     5 |                   1 |      4 |            4 |                  1 |
|  3 |     3 |                   1 |      2 |            4 |                  1 |
|  4 |     6 |                   1 |  **5** |        **5** |                  1 |
|  5 |     4 |                   1 |      3 |            5 |                  1 |

# ✅ Final output

```text id="z7k3p1"
5
```

The best transaction is:

```text id="b2n8v4"
Buy  → day 1, price = 1
Sell → day 4, price = 6

Profit = 6 - 1 = 5
```

Notice that the buy day (`1`) comes **before** the sell day (`4`), so it satisfies your condition of buying on one day and selling on a **different future day**.
