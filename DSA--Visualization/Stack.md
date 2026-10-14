# Table of Contents

- [Valid Parentheses](#valid-parentheses)
- [Baseball Game](#baseball-game)
- [Decode String](#decode-string)
- [Longest Valid Parentheses](#longest-valid-parentheses)
- [Monotonic Stack](#monotonic-stack)
- [Daily Temperatures](#daily-temperatures)
- [Min Stack](#min-stack)
- [Evaluate Reverse Polish Notation](#evaluate-reverse-polish-notation)

---

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
### Dry Run
We need to check whether the brackets in `s = "([{}])"` are valid and balanced using the repeated deletion method.

### 1. Explain the pseudocode line by line

Line 1: `repeat:`

Start a loop that continues until no more adjacent matching bracket pairs can be found.

Line 2: `find an adjacent pair "()", "[]" or "{}"`

Search the string for any matching pair of brackets that are next to each other.

For example:

* `"()"` is a matching pair.

* `"[]"` is a matching pair.

* `"{}"` is a matching pair.

Line 3: `if found: delete it`

If a matching pair is found, remove both characters from the string.

Line 4: `else: break`

If no matching adjacent pair exists, stop the loop.

Line 5: `return s is empty`

* If the string is empty, return `true` — all brackets are balanced.

* If the string is not empty, return `false` — some brackets remain unmatched.

### 2. Dry run step by step

Given:

```
s = "([{}])"
```

#### Step 1

Original string

# `([{}])`

The adjacent pair `"{}"` is found inside the string.

Delete `"{}"`.

## `([])`

#### Step 2

In `"([])"`, the pair `"[]"` is now adjacent inside the parentheses.

Delete `"[]"`.

## `()`

#### Step 3

The string now contains the adjacent pair `"()"`.

Delete `"()"`.

## `""` (empty string)

#### Step 4

The string is empty. No more pairs can be found, so the loop stops.

### ✅ Final Output

Returned value

# `true`

The brackets are correctly balanced and nested.

Final answer: `true`

### 🧠 Key idea
The order is valid because `{}` closes first, then `[]`, and finally `()`.


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

### Dry Run

We need to find the output of MinStack, a stack that supports `push`, `pop`, `top`, and `getMin` in O(1) time.

### 1. Explain the pseudocode line by line

We maintain two stacks:

* `main`: stores all the values pushed into the stack.
* `mins`: stores the minimum value at each corresponding stack position.

Line 1: `push(x): main.push(x); mins.push(min(x, mins.top))`

When pushing a value `x`:

1. Push `x` into `main`.
2. Push the smaller of `x` and the previous minimum into `mins`.

If `mins` is empty, use `x` as the minimum for the first element.

Line 2: `pop(): main.pop(); mins.pop()`

Remove the top element from both stacks to keep them in sync.

Line 3: `top(): return main.top()`

Return the top value from `main` without removing it.

Line 4: `getMin(): return mins.top()`

Return the top value of `mins`, which represents the minimum of all values currently in `main`.

### 2. Dry run step by step

Given:

```text
Operations: ["MinStack", "push", "push", "push", "getMin", "pop", "top", "getMin"]
Arguments:  [[], [-2], [0], [-3], [], [], [], []]
```

#### Step 1: `MinStack()`

Create two empty stacks.

* `main`: `[]`
* `mins`: `[]`

Returned value: `null`

#### Step 2: `push(-2)`

* Push `-2` into `main`.
* Since `mins` is empty, push `-2` as the current minimum.

* `main`: `[-2]`
* `mins`: `[-2]`

Returned value: `null`

#### Step 3: `push(0)`

* Push `0` into `main`.
* Compare `0` with the previous minimum `-2`.
* `min(0, -2) = -2`, so push `-2` into `mins`.

* `main`: `[-2, 0]`
* `mins`: `[-2, -2]`

Returned value: `null`

#### Step 4: `push(-3)`

* Push `-3` into `main`.
* Compare `-3` with the previous minimum `-2`.
* `min(-3, -2) = -3`, so push `-3` into `mins`.

* `main`: `[-2, 0, -3]`
* `mins`: `[-2, -2, -3]`

Returned value: `null`

#### Step 5: `getMin()`

The top of `mins` is `-3`.

Returned value: `-3`

#### Step 6: `pop()`

Remove the top element from both stacks.

* Remove `-3` from `main`.
* Remove `-3` from `mins`.

* `main`: `[-2, 0]`
* `mins`: `[-2, -2]`

Returned value: `null`

#### Step 7: `top()`

The top of `main` is `0`.

Returned value: `0`

#### Step 8: `getMin()`

The top of `mins` is `-2`.

Returned value: `-2`

### ✅ Final Output

Output array, in operation order:

# `[null, null, null, null, -3, null, 0, -2]`

### 🧠 Key idea

After `pop()` removes `-3`, the top of `mins` returns to `-2`. This allows `getMin()` to instantly return the current minimum in O(1) time without rescanning the stack.

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
