# Table of Contents

- [Reverse Linked List](#reverse-linked-list)
- [Merge Two Sorted Lists](#merge-two-sorted-lists)
- [Add Two Numbers](#add-two-numbers)
- [Linked List Cycle](#linked-list-cycle)
- [Palindrome Linked List](#palindrome-linked-list)
- [Remove Nth Node From End](#remove-nth-node-from-end)
- [Reorder List](#reorder-list)
- [Swap Nodes in Pairs](#swap-nodes-in-pairs)
- [LRU Cache](#lru-cache)

---

# Reverse Linked List

**LeetCode #206** · [LeetCode](https://leetcode.com/problems/reverse-linked-list/) · **Easy**

> **Linked List · Three Pointers · Flip Every `next` Arrow**

### Approaches

#### 1. Iterative (Three Pointers)
Keep `prev`, `curr`, and `next`; save the next node, reverse `curr.next`, then move all pointers forward.

**Pseudo Code:**

```text
prev ← ∅;  curr ← head

while curr:

    next ← curr.next      // save the rest

    curr.next ← prev      // flip the arrow

    prev ← curr; curr ← next

return prev               // new head
```
### Dry Run

We need to reverse a singly linked list using the given pseudocode.

### Given input

```text
head = [1, 2, 3, 4, 5]
```

Original linked list:

# `1 → 2 → 3 → 4 → 5 → null`

### 1. Explain the pseudocode line by line

Line 1: `prev ← ∅`

Initialize `prev` as `null`. It represents the previous node when reversing the links.

Line 2: `curr ← head`

Set `curr` to the first node of the list, which contains `1`.

Line 3: `while curr:`

Keep looping while `curr` is not `null`. This means there is still a node to process.

Line 4: `next ← curr.next`

Save the next node before changing the link. Otherwise, we could lose access to the remaining list.

Line 5: `curr.next ← prev`

Reverse the current node's arrow so it points backward to `prev`.

Line 6: `prev ← curr; curr ← next`

* `prev ← curr`: Move `prev` to the node just processed.
* `curr ← next`: Move `curr` to the next unprocessed node.

Line 7: `return prev`

When `curr` becomes `null`, all links have been reversed. `prev` points to the new head of the list.

### 2. Dry run step by step

#### Initial state

* `prev = null`
* `curr = 1`

#### Iteration 1

* `next = 2`
* Reverse the link: `1 → null`
* Move `prev = 1`, `curr = 2`

Reversed part: `1 → null`

#### Iteration 2

* `next = 3`
* Reverse the link: `2 → 1`
* Move `prev = 2`, `curr = 3`

Reversed part: `2 → 1 → null`

#### Iteration 3

* `next = 4`
* Reverse the link: `3 → 2`
* Move `prev = 3`, `curr = 4`

Reversed part: `3 → 2 → 1 → null`

#### Iteration 4

* `next = 5`
* Reverse the link: `4 → 3`
* Move `prev = 4`, `curr = 5`

Reversed part: `4 → 3 → 2 → 1 → null`

#### Iteration 5

* `next = null`
* Reverse the link: `5 → 4`
* Move `prev = 5`, `curr = null`

Reversed list: `5 → 4 → 3 → 2 → 1 → null`

#### Loop ends

`curr = null`, so the loop stops. Return `prev`, which points to node `5`.

### 3. Dry-run summary

| Iteration | `curr` | `next` | `prev` after update |
| :---: | :---: | :---: | :---: |
| **1** | 1 | 2 | 1 |
| **2** | 2 | 3 | 2 |
| **3** | 3 | 4 | 3 |
| **4** | 4 | 5 | 4 |
| **5** | 5 | null | 5 |

### ✅ Final Output

Reversed linked list:

# `[5, 4, 3, 2, 1]`

Final answer: `[5, 4, 3, 2, 1]`

### 🧠 Key idea

Remember the three key steps: save the next node, reverse the current link, then move `prev` and `curr` forward.

#### 2. Recursive
Recursively reverse the remaining list, then make `head.next.next = head` and set `head.next = null`.

**Pseudo Code:**

```text
reverse(head):

    if head is ∅ or head.next is ∅: return head

    newHead ← reverse(head.next)

    head.next.next ← head   // node after head points back

    head.next ← ∅

    return newHead
```

### Dry Run

This pseudocode reverses a linked list using recursion. Unlike the iterative method, which uses a `while` loop, this method calls the same function again and again until it reaches the last node.

Input: `head = [1, 2, 3, 4, 5]`

### 1. Understand the pseudocode line by line

```text
reverse(head):
    if head is ∅ or head.next is ∅:
        return head

    newHead ← reverse(head.next)

    head.next.next ← head
    head.next ← ∅

    return newHead
```

#### Line 1: Base condition

```text
if head is ∅ or head.next is ∅:
    return head
```

* `head is ∅` means the linked list is empty.
* `head.next is ∅` means there is only one node left.
* In either case, the list is already reversed, so return `head`.

For our input, the recursion continues until it reaches node `5`, because `5.next = ∅`.

#### Line 2: Recursive call

```text
newHead ← reverse(head.next)
```

This calls `reverse()` on the next node.

For example:

* `reverse(1)` calls `reverse(2)`.
* `reverse(2)` calls `reverse(3)`.
* `reverse(3)` calls `reverse(4)`.
* `reverse(4)` calls `reverse(5)`.

At node `5`, the base condition is satisfied, so `5` is returned as `newHead`.

Important: The recursive calls now start returning in reverse order, from node `5` back toward node `1`.

#### Line 3: Reverse the link

```text
head.next.next ← head
```

This is the most important line.

Suppose `head = 4`, and `head.next = 5`.

Originally:

`4 → 5 → NULL`

The statement `head.next.next ← head` means:

`5.next = 4`

Now the link points backward:

`5 → 4`

#### Line 4: Remove the old forward link

```text
head.next ← ∅
```

This sets the next pointer of the current node to `NULL`.

For example, when `head = 4`, it sets `4.next = NULL`, so the old forward connection from `4` to `5` is removed.

This prevents the list from keeping the old link in the wrong direction.

#### Line 5: Return the new head

```text
return newHead
```

Return node `5`, which became the first node of the reversed linked list. Every recursive call returns the same `newHead`.

### 2. Dry run with input `[1, 2, 3, 4, 5]`

#### Step A: Recursive calls go down

The function keeps calling the next node.

* `reverse(1)`
* `reverse(2)`
* `reverse(3)`
* `reverse(4)`
* `reverse(5)` — base condition reached

At node `5`, the function returns `5`. Now the remaining calls execute their statements while returning.

#### Step B: Recursive calls return and reverse links

| Current `head` | Statement | Result |
| :---: | :--- | :--- |
| **4** | `5.next = 4`, then `4.next = NULL` | `5 → 4` |
| **3** | `4.next = 3`, then `3.next = NULL` | `5 → 4 → 3` |
| **2** | `3.next = 2`, then `2.next = NULL` | `5 → 4 → 3 → 2` |
| **1** | `2.next = 1`, then `1.next = NULL` | `5 → 4 → 3 → 2 → 1` |

Notice that `newHead` remains node `5` throughout the process.

### 3. Final output

Original linked list:

# `1 → 2 → 3 → 4 → 5 → NULL`

Reversed linked list:

# `[5, 4, 3, 2, 1]`

Output: `[5, 4, 3, 2, 1]`

### 🧠 Key idea

1. `reverse(head.next)` goes all the way to the last node first.
2. `head.next.next = head` flips the link backward.
3. `head.next = NULL` removes the old forward link.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Iterative · Three Pointers** | **O(n)** | **O(1)** |
| **Recursive** | **O(n)** | **O(n)** |

---

# Merge Two Sorted Lists

**LeetCode #21** · [LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/) · **Easy**

> **Linked List · Two Pointers · Dummy Node · Splice the Smaller Head**

### Approaches

#### 1. Naive (Collect + Sort)
Collect all values from both lists, sort them, and rebuild the linked list.

**Pseudo Code:**

```text
vals ← all values of both lists

sort(vals)

rebuild a list from vals
```

#### 2. Optimized (Two Pointers)
Use a dummy node; compare `l1` and `l2`, attach the smaller node, and move that pointer forward. Finally, attach the remaining list.

**Pseudo Code:**

```text
dummy ← node; tail ← dummy

while l1 and l2:

    if l1.val <= l2.val: tail.next ← l1; l1 ← l1.next

    else:                 tail.next ← l2; l2 ← l2.next

    tail ← tail.next

tail.next ← l1 or l2          // attach remainder

return dummy.next
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Naive · Collect + Sort** | **O((n + m) log(n + m))** | **O(n + m)** |
| **Optimized · Two Pointers** | **O(n + m)** | **O(1)** |

---

# Add Two Numbers

**LeetCode #2** · [LeetCode](https://leetcode.com/problems/add-two-numbers/) · **Medium**

> **Linked List · Elementary Addition · Carry**

### Approaches

#### 1. Optimized (Elementary Addition)
Traverse both lists together, add corresponding digits with `carry`, store `sum % 10`, and continue while either list or `carry` remains.

**Pseudo Code:**

```text
dummy ← node; tail ← dummy; carry ← 0

while l1 or l2 or carry:

    sum ← (l1.val or 0) + (l2.val or 0) + carry

    carry ← sum / 10

    tail.next ← node(sum % 10); tail ← tail.next

    advance l1 and l2 if present

return dummy.next
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Optimized · Elementary Addition** | **O(max(n, m))** | **O(max(n, m))** |

---

# Linked List Cycle

**LeetCode #141** · [LeetCode](https://leetcode.com/problems/linked-list-cycle/) · **Easy**

> **Linked List · Floyd’s Tortoise & Hare · Two-Speed Pointers**

### Approaches

#### 1. Brute Force (Visited Set)
Store each visited node in a set; if the same node appears again, a cycle exists.

**Pseudo Code:**

```text
given head

visited = {}

cur = head; while cur ≠ ∅:

    if cur in visited: return true

    add cur; cur = cur.next

return false
```

### Dry Run

This pseudocode detects whether a linked list contains a cycle (loop). It uses a set called `visited` to remember which nodes it has already visited.

Input: `head = [3, 2, 0, -4]`, `pos = 1`

Here, `pos = 1` means the last node (`-4`) points back to the node at index `1`, which contains `2`. Therefore, the linked list has a cycle.

### 1. Explain the pseudocode line by line

```text
visited = {}
cur = head

while cur ≠ ∅:
    if cur in visited:
        return true
    add cur to visited
    cur = cur.next

return false
```

#### Line 1: Create an empty set

```text
visited = {}
```

* `visited` stores the nodes we have already visited.
* Initially, it is empty: `{}`.

#### Line 2: Start at the head

```text
cur = head
```

`cur` is a pointer to the current node.

Initially, `cur` points to node `3`.

#### Line 3: Check whether the current node exists

```text
while cur ≠ ∅:
```

This loop continues as long as `cur` is not `NULL` (∅).

If `cur` becomes `NULL`, we reach the end of the list, meaning there is no cycle.

#### Line 4: Check whether we have seen this node before

```text
if cur in visited:
    return true
```

If the current node is already in `visited`, we have reached the same node again.

That means the linked list contains a cycle, so return `true`.

Important: We check the node itself, not just its value. Two different nodes could contain the same value.

#### Line 5: Remember the current node

```text
add cur to visited
```

Store the current node in the set so we can detect it if we encounter it again.

#### Line 6: Move to the next node

```text
cur = cur.next
```

Move `cur` forward by one node.

#### Line 7: Return false

```text
return false
```

This executes only if the loop reaches `NULL` without finding a previously visited node. It means there is no cycle.

### 2. Dry run for the given input

The linked list looks like this:

Because `pos = 1`, the last node points back to node `2`:

# `3 → 2 → 0 → -4 ──┐`
# `     ^           │`
# `     └───────────┘`

Now execute the code step by step:

| Step | `cur` value | Is it in `visited`? | Action | `visited` after action |
| :---: | :---: | :---: | :--- | :---: |
| **1** | 3 | No | Add node 3; move to next | `{3}` |
| **2** | 2 | No | Add node 2; move to next | `{3, 2}` |
| **3** | 0 | No | Add node 0; move to next | `{3, 2, 0}` |
| **4** | -4 | No | Add node -4; move to next | `{3, 2, 0, -4}` |
| **5** | 2 | Yes! | Node 2 was visited before, so return `true` | No change |

Notice the sequence of nodes visited:

`3 → 2 → 0 → -4 → 2 → 0 → -4 → ...`

The node containing `2` is encountered again, so the loop never reaches `NULL`.

### ✅ Final Output

Output:

# `true`

The linked list contains a cycle because the last node points back to the node at index `1`.

### 🧠 Key idea

This approach uses extra space:
* Time: O(n) — each distinct node is checked once before the cycle is detected.
* Extra space: O(n) — the `visited` set stores the nodes.

Interview tip: This approach uses extra space. Another common solution is Floyd’s Cycle Detection algorithm, which uses two pointers (`slow` and `fast`) and requires O(1) extra space.

#### 2. Optimized (Floyd’s Two-Speed Pointers)
Use `slow` moving one step and `fast` moving two steps; if they meet, there is a cycle. If `fast` reaches `null`, there is no cycle.

**Pseudo Code:**

```text
given head

slow = fast = head

while fast and fast.next:

    slow = slow.next; fast = fast.next.next

    if slow == fast: return true

return false

// in a loop, fast gains 1 per step → must catch slow
```

### Dry Run

This pseudocode detects whether a linked list contains a cycle (loop) using Floyd’s Cycle Detection Algorithm, also called the Slow and Fast Pointer method.

Input: `head = [1, 2]`, `pos = 0`

Here, `pos = 0` means the last node (`2`) points back to the node at index `0` (`1`). So the linked list contains a cycle.

### 1. Explain the pseudocode line by line

```text
slow = fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next

    if slow == fast:
        return true

return false
```

#### Line 1: Initialize both pointers

```text
slow = fast = head
```

Both pointers start at the first node.

* `slow` moves one node at a time.
* `fast` moves two nodes at a time.

Initially, both point to node `1`.

#### Line 2: Check whether fast can move

```text
while fast and fast.next:
```

The loop continues as long as:

* `fast` is not `NULL`.
* `fast.next` is not `NULL`.

This ensures that `fast` can safely move two nodes forward.

#### Line 3: Move the slow pointer

```text
slow = slow.next
```

Move `slow` forward by one node.

For example, if `slow` points to node `1`, it moves to node `2`.

#### Line 4: Move the fast pointer

```text
fast = fast.next.next
```

Move `fast` forward by two nodes.

Because the linked list contains a cycle, `fast` will keep moving around the loop.

#### Line 5: Compare the pointers

```text
if slow == fast:
    return true
```

If both pointers reach the same node, a cycle exists.

Why? In a cycle, `fast` moves faster than `slow`, so eventually it catches up with `slow`.

#### Line 6: Return false

```text
return false
```

If `fast` reaches `NULL`, there is no cycle.

### 2. Dry run for `head = [1, 2], pos = 0`

Because `pos = 0`, node `2` points back to node `1`.

Linked list with a cycle:

# `1 ⇄ 2` (Node 2 points back to node 1)

### Step-by-step execution

| Step | `slow` | `fast` | What happens? |
| :---: | :---: | :---: | :--- |
| **Initial** | 1 | 1 | Both start at node 1 |
| **Iteration 1** | 2 | 1 | `slow` moves to 2; `fast` moves two steps: 1 → 2 → 1 |
| **After iteration 1** | 2 | 1 | Pointers are different, continue |
| **Iteration 2** | 1 | 1 | `slow` moves to 1; `fast` moves two steps and returns to 1 |
| **After iteration 2** | 1 | 1 | Both point to the same node, return `true` |

Notice that `fast` can move two steps even with only two nodes because the list loops back to node `1`.

### ✅ Final Output

Output:

# `true`

A cycle exists because `slow` and `fast` eventually meet at the same node.

### 🧠 Key idea

* `slow = slow.next` → moves 1 step.
* `fast = fast.next.next` → moves 2 steps.
* `slow == fast` → cycle detected.
* `fast == NULL` or `fast.next == NULL` → no cycle.

Complexity:
* Time: O(n)
* Extra space: O(1), because only two pointers are used and no set is needed.
### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Visited Set** | **O(n)** | **O(n)** |
| **Optimized · Floyd’s Two-Speed Pointers** | **O(n)** | **O(1)** |

---

# Palindrome Linked List

**LeetCode #234** · [LeetCode](https://leetcode.com/problems/palindrome-linked-list/) · **Easy**

> **Linked List · Find Middle · Reverse Second Half · Two Pointers**

### Approaches

#### 1. Brute Force (Copy to Array)
Copy all node values into an array, then compare values from both ends using two pointers.

**Pseudo Code:**

```text
given head

copy all values into arr

L = 0, R = n − 1

while L < R: if arr[L++] ≠ arr[R−−]: return false

return true
```
### Dry Run

This pseudocode checks whether a linked list is a palindrome. A palindrome reads the same forward and backward, such as `[1, 2, 2, 1]`.

Input: `head = [1, 2, 2, 1]`

### 1. Explain the pseudocode line by line

```text
copy all values into arr

L = 0, R = n − 1

while L < R:
    if arr[L++] ≠ arr[R−−]:
        return false

return true
```

#### Line 1: Copy linked-list values into an array

```text
copy all values into arr
```

We copy each node's value into an array so we can easily compare values from both ends.

For our input:

`arr = [1, 2, 2, 1]`

The array has `n = 4` elements.

#### Line 2: Initialize two pointers

```text
L = 0, R = n − 1
```

* `L` points to the first element (index `0`).
* `R` points to the last element (index `3`).

So:
* `L = 0` → `1`
* Index 1 → `2`
* Index 2 → `2`
* `R = 3` → `1`

#### Line 3: Continue while the pointers have not met

```text
while L < R:
```

The loop runs while `L` is smaller than `R`.

We compare the first and last elements, then move inward. Once the pointers meet or cross, all necessary comparisons are complete.

#### Line 4: Compare both ends

```text
if arr[L++] ≠ arr[R−−]:
    return false
```

This line compares the left and right values.

* `arr[L++]` means use `arr[L]` first, then increase `L` by 1.
* `arr[R−−]` means use `arr[R]` first, then decrease `R` by 1.
* `≠` means “not equal.”

If the values differ, the list is not a palindrome, so return `false` immediately.

#### Line 5: Return true

```text
return true
```

If every pair matches, the linked list is a palindrome, so return `true`.

### 2. Dry run for `[1, 2, 2, 1]`

| Step | `L` before | `R` before | Values compared | Result |
| :---: | :---: | :---: | :---: | :--- |
| **1** | 0 | 3 | `arr[0] = 1`, `arr[3] = 1` | Equal; move inward |
| **2** | 1 | 2 | `arr[1] = 2`, `arr[2] = 2` | Equal; move inward |
| **3** | 2 | 1 | `L < R` is false | Stop loop |

Both comparisons match, so the code reaches `return true`.

### ✅ Final Output

Output:

# `true`

`[1, 2, 2, 1]` is a palindrome because it reads the same forward and backward.

### 🧠 Key idea

Complexity:
* Time: O(n) — copy the values and compare pairs.
* Extra space: O(n) — the array stores all linked-list values.

Interview tip: The array approach is simple to understand. You can also check a linked-list palindrome with O(1) extra space by reversing its second half.

#### 2. Optimized (Reverse the Back Half)
Find the middle using slow/fast pointers, reverse the second half, then compare the first and reversed second halves.

**Pseudo Code:**

```text
given head

slow/fast walk → slow = middle

reverse the list after middle

p1 = head, p2 = head of reversed half

while p2: if p1.val ≠ p2.val: return false; advance both

return true
```

### Dry Run

This pseudocode checks whether a linked list is a palindrome without copying all values into an array. It uses slow/fast pointers and reverses the second half of the list.

Input: `head = [1, 3, 2, 1, 2, 3, 1]`

### 1. Understand the pseudocode line by line

#### Step 1: Find the middle of the linked list

```text
slow/fast walk → slow = middle
```

* `slow` moves one node at a time.
* `fast` moves two nodes at a time.
* When `fast` reaches the end, `slow` is around the middle.

For our input, there are 7 nodes, so the middle value is `1` at index `3`.

```text
1 → 3 → 2 → [1] → 2 → 3 → 1
                  middle
```

#### Step 2: Reverse the list after the middle

```text
reverse the list after middle
```

The second half originally is:

`2 → 3 → 1`

After reversing it:

`1 → 3 → 2`

The list is now conceptually split into two parts:

* First half (including middle): `1 → 3 → 2 → 1`
* Reversed second half: `1 → 3 → 2`

#### Step 3: Initialize two pointers

```text
p1 = head
p2 = head of reversed half
```

* `p1` starts at the beginning of the original list.
* `p2` starts at the beginning of the reversed second half.

For our example:

* `p1` points to `1 → 3 → 2 → 1 ...`
* `p2` points to `1 → 3 → 2`

#### Step 4: Compare the values

```text
while p2:
    if p1.val ≠ p2.val:
        return false
    advance both
```

As long as `p2` is not `NULL`:

1. Compare `p1.val` with `p2.val`.
2. If they differ, return `false`.
3. Otherwise, move both pointers forward by one node.

We only need to compare the length of the reversed second half.

#### Step 5: Return true

```text
return true
```

If every comparison matches, the list is a palindrome.

### 2. Dry run for `[1, 3, 2, 1, 2, 3, 1]`

After reversing the second half, we compare these values:

| Step | `p1.val` | `p2.val` | Comparison |
| :---: | :---: | :---: | :--- |
| **1** | 1 | 1 | Equal |
| **2** | 3 | 3 | Equal |
| **3** | 2 | 2 | Equal |
| **4** | 1 | — | Stop; `p2` is `NULL` |

Every comparison matches, so the code reaches `return true`.

### ✅ Final Output

Output:

# `true`

The linked list `[1, 3, 2, 1, 2, 3, 1]` is a palindrome because its values read the same forward and backward.

### 🧠 Key idea

Complexity:
* Time: O(n) — finding the middle, reversing the second half, and comparing take linear time.
* Extra space: O(1) — only pointers are used.

Important: This is conceptual pseudocode. In an actual implementation, for an odd-length list, you skip the middle node when comparing the two halves. Also, reversing the second half modifies the list, so you may reverse it again afterward to restore the original list.

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Copy to Array** | **O(n)** | **O(n)** |
| **Optimized · Reverse the Back Half** | **O(n)** | **O(1)** |

---

# Remove Nth Node From End

**LeetCode #19** · [LeetCode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) · **Medium**

> **Linked List · Two Pointers · Keep a Gap of `n`**

### Approaches

#### 1. Brute Force (Two Passes)
First count the nodes, then walk to the node before the target and skip the nth node from the end.

**Pseudo Code:**

```text id="j8k3wm"
given head, n

L = count the nodes          // pass 1

walk to node (L − n − 1)     // pass 2

cur.next = cur.next.next     // splice

return head
```

#### 2. Optimized (One Pass, Gap of `n`)
Move `fast` `n` steps ahead, then move `slow` and `fast` together; when `fast` reaches the end, `slow` is at the node before the target. Use a dummy node to handle removing the head.

**Pseudo Code:**

```text id="x4v7np"
given head, n

fast = head advanced n hops; slow = head

while fast is not the last node:

    slow++, fast++            // gap stays n

slow.next = slow.next.next    // splice

return head
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Two Passes** | **O(n)** | **O(1)** |
| **Optimized · One Pass, Gap of `n`** | **O(n)** | **O(1)** |

---

# Reorder List

**LeetCode #143** · [LeetCode](https://leetcode.com/problems/reorder-list/) · **Medium**

> **Linked List · Find Middle · Reverse Second Half · Alternate Merge**

### Approaches

#### 1. Brute Force (Array of Node References)
Store all node references in an array, then use two pointers from both ends to relink nodes in alternating order.

**Pseudo Code:**

```text id="a7k2qp"
given head

collect node refs into nodes[]

L = 0, R = n − 1

while L < R: append nodes[L++]

             append nodes[R−−]

relink in that order
```

#### 2. Optimized (Middle + Reverse + Merge)
Find the middle with slow/fast pointers, reverse the second half, then merge the two halves alternately.

**Pseudo Code:**

```text id="m9x4ct"
given head

slow/fast → middle; split there

reverse the back half

A = front, B = reversed back

weave: A₀ → B₀ → A₁ → B₁ → …

return head
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Brute Force · Array of Node References** | **O(n)** | **O(n)** |
| **Optimized · Middle + Reverse + Merge** | **O(n)** | **O(1)** |

---

# Swap Nodes in Pairs

**LeetCode #24** · [LeetCode](https://leetcode.com/problems/swap-nodes-in-pairs/) · **Medium**

> **Linked List · Pointer Manipulation · Dummy Head · Swap Adjacent Nodes**

### Approaches

#### 1. Shortcut (Swap Values)
Swap the values of every adjacent pair, but this is **not allowed** because the problem requires swapping the actual nodes.

**Pseudo Code:**

```text id="n5q2vk"
given head

cur = head

while cur and cur.next:

    swap(cur.val, cur.next.val); cur = cur.next.next

return head    // usually not allowed!
```

#### 2. Optimized (Rewire the Pointers)
Use a dummy node and, for each pair, rewire the three key links to swap the two nodes without changing their values.

**Pseudo Code:**

```text id="k8w4rp"
dummy → head; prev = dummy

while prev.next and prev.next.next:

    a = prev.next; b = a.next

    a.next = b.next; b.next = a; prev.next = b

    prev = a

return dummy.next
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Shortcut · Swap Values** | **O(n)** | **O(1)** |
| **Optimized · Rewire the Pointers** | **O(n)** | **O(1)** |

---

# LRU Cache

**LeetCode #146** · [LeetCode](https://leetcode.com/problems/lru-cache/) · **Medium**

> **Hash Map + Doubly Linked List · O(1) `get` / `put`**

### Approaches

#### 1. Optimized (Hash Map + Doubly Linked List)
Use a hash map for **O(1)** node lookup and a doubly linked list to maintain usage order. Keep the **most recently used** node at the front and the **least recently used** node at the back.

**Pseudo Code:**

```text id="q7m3vx"
map: key → node;  DLL ordered newest → oldest

get(key):

    if key not in map: return -1       // MISS

    node ← map[key]                    // O(1) lookup

    moveToFront(node)                  // splice + relink, O(1)

    return node.value

put(key, value):

    if key in map:

        node.value ← value; moveToFront(node)    // UPDATE

    else:

        if size == capacity: evict(tail)         // drop LRU, O(1)

        insert new node at FRONT; map[key] ← node
```

### Complexity

| Approach | Time | Space |
| :--- | :---: | :---: |
| **Hash Map + Doubly Linked List** | **O(1) average** | **O(capacity)** |
