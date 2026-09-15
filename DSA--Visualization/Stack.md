# Valid Parentheses

**LeetCode #20** · [LeetCode](https://leetcode.com/problems/valid-parentheses/) · **Easy**

> **Stack · LIFO · The top bracket must close next**

### Approaches

#### 1. Brute force (delete adjacent pairs)
Repeatedly find and remove adjacent matching pairs `()`, `[]`, or `{}`. If the string becomes empty, it is valid.

**Pseudo Code:**

```text id="a7m3qx"
given s

repeat:

    find an adjacent pair "()", "[]" or "{}"

    if found: delete it

    else: break

return s is empty
```

#### 2. Optimized (stack of open brackets)
Push opening brackets onto a stack. For every closing bracket, check whether it matches the stack top; at the end, the stack must be empty.

**Pseudo Code:**

```text id="k9v2pd"
given s

st = []

for each ch in s:

    if ch opens: push ch

    else:

        if st empty or top mismatches: return false

        pop

return st is empty
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n²)** | **O(n)** |
| **Optimized** | **O(n)** | **O(n)** |

---

# Baseball Game

**LeetCode #682** · [LeetCode](https://leetcode.com/problems/baseball-game/) · **Easy**

> **Stack · LIFO · Every operation works with the latest scores**

### Approaches

#### 1. Brute force (list with index maths)
Maintain a list of valid scores and use the last one or two elements to handle `+`, `D`, and `C`.

**Pseudo Code:**

```text id="h4n8qs"
record ← []

for op in ops: update record using record[len − 1], record[len − 2]

return sum(record)
```

#### 2. Optimized (stack)
Use a stack of scores; push new scores, use the top two for `+`, double the top for `D`, and pop for `C`.

**Pseudo Code:**

```text id="v6p2mt"
stack ← []

for op in ops:

    if op == "C":  stack.pop()

    if op == "D":  stack.push(2 × stack.top)

    if op == "+":  stack.push(stack.top + stack.second)

    else:          stack.push(int(op))

return sum(stack)
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n)** | **O(n)** |
| **Optimized** | **O(n)** | **O(n)** |

---

# Decode String

**LeetCode #394** · [LeetCode](https://leetcode.com/problems/decode-string/) · **Medium**

> **Stack · Nested Strings · `]` completes the top encoding**

### Approaches

#### 1. Brute force (expand innermost repeatedly)
Find the innermost `k[body]`, replace it with `body` repeated `k` times, and repeat until no brackets remain.

**Pseudo Code:**

```text id="q5m8zr"
given s

while s contains "[":

    find the innermost k[body]

    replace it with body repeated k times

return s
```

#### 2. Optimized (one pass with a stack)
Use a stack to store previous strings and repeat counts; when `]` appears, pop the current segment and count, decode it, and attach it to the previous string.

**Pseudo Code:**

```text id="x3n7kp"
given s

st = []

for each ch in s:

    if ch ≠ "]": push ch

    else:

        body = pop chars until "[" (drop the "[")

        k = pop the number

        push body × k

return the single remaining item
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n²)** | **O(n)** |
| **Optimized** | **O(output)** | **O(output)** |

---

# Longest Valid Parentheses

**LeetCode #32** · [LeetCode](https://leetcode.com/problems/longest-valid-parentheses/) · **Hard**

> **Stack · Store indices · Keep a base index for each valid run**

### Approaches

#### 1. Brute force (balance from every start)
Start from each index, maintain the parenthesis balance, stop when it becomes negative, and update the longest length whenever the balance becomes `0`.

**Pseudo Code:**

```text id="m8q2vd"
given s

best ← 0

for i ← 0 to n − 1:

    bal ← 0

    for j ← i to n − 1:

        bal += (s[j] == "(" ? +1 : −1)

        if bal < 0: break

        if bal == 0: best = max(best, j − i + 1)

return best
```

#### 2. Optimized (index stack with base)
Store indices of unmatched parentheses; keep a base index below each valid run. On `(` push its index; on `)` pop, and if the stack becomes empty update the base, otherwise calculate the current valid length.

**Pseudo Code:**

```text id="t5k9rx"
given s

st = [−1]               // base index

for i ← 0 to n − 1:

    if s[i] == "(": push i

    else:

        pop

        if st empty: push i    // new base

        else: best = max(best, i − st.top)

return best
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n²)** | **O(1)** |
| **Optimized** | **O(n)** | **O(n)** |

---

# Monotonic Stack

**Concept**

> **Stack · Monotonic Decreasing Stack · Next Greater Element**

### Approaches

#### 1. Concept
Maintain a stack of elements that are **waiting for a greater element**. Keep the stack in decreasing order. When a new value `x` is greater than the stack top, pop elements because `x` is their next greater element. Then push `x`.

**Pseudo Code:**

```text id="n6r3vk"
// next greater element, via a decreasing stack

st = []                      // values waiting

for each x in arr:

    while st not empty and st.top < x:

        pop — x is its next greater

    push x

// leftovers have no next greater
```

**Key idea:** Each element is pushed and popped at most once, giving **O(n)** time.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Concept** | **O(n)** | **O(n)** |

----

# Daily Temperatures

**LeetCode #739** · [LeetCode](https://leetcode.com/problems/daily-temperatures/) · **Medium**

> **Monotonic Stack · Decreasing stack · New warmer days resolve waiting days**

### Approaches

#### 1. Brute force (scan right from every day)
For each day, scan to the right until you find a warmer temperature; store the number of days waited, or `0` if none exists.

**Pseudo Code:**

```text
given temps

for i ← 0 to n − 1:

    scan j = i+1, i+2, … until temps[j] > temps[i]

    answer[i] = found ? j − i : 0

return answer
```

#### 2. Optimized (monotonic stack of waiting days)
Keep indices of days waiting for a warmer temperature in a decreasing stack. When the current temperature is warmer than the stack top, pop that index and set its answer to the current index minus the popped index.

**Pseudo Code:**

```text
given temps

st = []                       // indices, temps decreasing

for i ← 0 to n − 1:

    while st not empty and temps[st.top] < temps[i]:

        pop j; answer[j] = i − j

    push i

// leftovers: answer = 0
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute force** | **O(n²)** | **O(1)** |
| **Optimized** | **O(n)** | **O(n)** |

---

# Min Stack

**LeetCode #155** · [LeetCode](https://leetcode.com/problems/min-stack/) · **Medium**

> **Stack · Twin stacks · Track minimum in O(1)**

### Approaches

#### 1. Brute Force
Use one stack and scan all elements to find the minimum whenever `getMin()` is called.

#### 2. Optimized (Twin Min-Stack)
Use `main` stack for values and `mins` stack to store the minimum value at each level. This makes `getMin()` simply `mins.top()`.

**Pseudo Code:**

```text
// keep stack `main` and stack `mins` in lockstep

push(x):  main.push(x); mins.push(min(x, mins.top))

pop():    main.pop();   mins.pop()

top():    return main.top()

getMin(): return mins.top()
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(1) per operation** | **O(n)** |

---

# Evaluate Reverse Polish Notation

**LeetCode #150** · [LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/) · **Medium**

> **Stack · Operand stack · Postfix evaluation**

### Approaches

#### 1. Optimized (Operand Stack)
Push numbers onto the stack. When an operator appears, pop `right` first, then `left`, calculate `left OP right`, and push the result back.

**Pseudo Code:**

```text
stack = []

for token in tokens:

    if token is a number:

        stack.push(number(token))

    else:                       // operator

        right = stack.pop()

        left = stack.pop()

        stack.push(left OP right)

return stack.top()              // the only element
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized** | **O(n)** | **O(n)** |
