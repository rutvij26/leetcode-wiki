---
title: "Complexity"
parent: Topics
nav_order: 13
---

## ELI5 (Explain Like I'm 5)

Before you give someone directions, you should say "this will take about 10 minutes." Complexity is the same idea for your code — before you finish explaining your solution to an interviewer, you should say "this runs in O(n) time and uses O(1) extra space." Amazon explicitly tests this. Always state it.

## Explanation

- **Time complexity** — how the number of operations grows as input size `n` grows
- **Space complexity** — how much extra memory your algorithm uses as `n` grows
- Big O notation ignores constants and lower-order terms: `3n + 5` → `O(n)`
- You analyze the **worst case** unless told otherwise

> In interviews: state complexity proactively. Don't wait to be asked. "This is O(n log n) time and O(1) space."

## Big O Cheat Sheet

| Operation | Time | Space |
| --------- | ---- | ----- |
| Array scan (one loop) | O(n) | O(1) |
| Nested loops over array | O(n²) | O(1) |
| Sort an array | O(n log n) | O(log n) |
| HashMap insert / lookup | O(1) amortized | O(n) total |
| Binary search | O(log n) | O(1) |
| Two pointers / sliding window | O(n) | O(1) |

## Pattern → Complexity Reference

| Pattern | Time | Space |
| ------- | ---- | ----- |
| HashMap counting | O(n) | O(n) |
| Two pointers (sorted array) | O(n) | O(1) |
| Sliding window | O(n) | O(k) — window size |
| Stack (monotonic) | O(n) | O(n) |
| Linked list traversal | O(n) | O(1) |
| Recursion (tree) | O(n) | O(h) — tree height |
| Tree DFS | O(n) | O(h) |
| Tree BFS | O(n) | O(w) — max width |
| BST search/insert | O(log n) avg, O(n) worst | O(h) |
| Heap — n inserts, size K | O(n log k) | O(k) |
| Grid BFS / DFS | O(rows × cols) | O(rows × cols) |
| Greedy (sort + scan) | O(n log n) | O(1) |

## Space Complexity Notes

- **O(1)** — constant: a few variables, no data structures that grow with input
- **O(log n)** — logarithmic: the call stack of a balanced tree recursion or binary search
- **O(n)** — linear: a HashMap, a result array, a queue for BFS
- **O(h)** — tree height: DFS call stack (h = log n for balanced, n for skewed)
- **O(rows × cols)** — grid size: BFS queue or visited array for a full grid traversal

## Complexity for Common Trees

| Tree type | Height | DFS time | DFS space |
| --------- | ------ | -------- | --------- |
| Balanced BST | O(log n) | O(n) | O(log n) |
| Skewed (linked list) | O(n) | O(n) | O(n) |
| Complete binary tree | O(log n) | O(n) | O(log n) |

## How to Master This

### Step-by-step
1. After every solution you write, practice saying the complexity out loud
2. Learn to recognize patterns: one loop = O(n), nested loops = O(n²), divide in half = O(log n)
3. For recursive solutions: draw the call tree and count total calls
4. For space: ask "what data structures did I create? How big do they get?"

### Key Resources
- 📹 [CS Dojo — Introduction to Big O Notation](https://www.youtube.com/watch?v=v4cd1O4zkGw)
- 📹 [NeetCode — Big O Notation](https://www.youtube.com/watch?v=BgLTDT03QtU)
- 📝 [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- 📝 [NeetCode.io — all problems show expected complexity](https://neetcode.io/roadmap)

## Common Mistakes

- **Forgetting the sort**: greedy solutions often look O(n) but are O(n log n) because of the sort
- **Recursion space**: a recursive tree solution uses O(h) call stack space — not O(1)
- **HashMap space**: if you use a map, your space complexity is at least O(n) — easy to miss
- **BFS queue**: the queue in BFS can hold up to O(n) nodes at once (widest level of the tree)
