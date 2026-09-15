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

**Time:** **O(n)**

**Space:** **O(1)**

----


