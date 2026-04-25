---
title: Patterns Cheat Sheet
nav_order: 4
---

# Patterns Cheat Sheet

When you see a keyword in a problem, map it to a pattern. Use this as a quick decision tool before writing any code.

---

## Keyword → Pattern Map

| Keywords / Problem Shape | Pattern | Complexity |
|--------------------------|---------|------------|
| Contiguous subarray, longest/shortest with condition | [Sliding Window](./topics/sliding-window/) | O(n) |
| Sorted array, find pair summing to X | [Two Pointers](./topics/two-pointers/) | O(n) |
| Sorted array, find target or position | [Binary Search](./topics/binary-search/) | O(log n) |
| All combinations / permutations / subsets | [Backtracking](./topics/combinatorial-dfs/) | O(2ⁿ) / O(n!) |
| Number of ways to do X, min/max cost, can you reach X | [Dynamic Programming](./topics/dynamic-programming/) | O(n) – O(n²) |
| Connected components, path exists, cycle in graph | [Graph DFS](./topics/graph-dfs/) | O(V+E) |
| Shortest path, BFS on grid | [Graph BFS](./topics/graph-bfs/) | O(V+E) |
| Shortest path in weighted graph (no neg weights) | [Dijkstra](./topics/dijkstra/) | O((V+E)logV) |
| Shortest path with negative weights | [Bellman-Ford](./topics/bellman-ford/) | O(VE) |
| Task ordering, prerequisites, dependencies | [Topological Sort](./topics/topological-sort/) | O(V+E) |
| Merge groups, detect cycle in undirected graph | [Union Find](./topics/union-find/) | O(α(n)) |
| Prefix search, autocomplete, starts with | [Trie](./topics/trie/) | O(m) |
| Range sum/min/max with updates | [Segment Tree](./topics/segment-tree/) / [Fenwick Tree](./topics/fenwick-tree/) | O(log n) |
| Repeated substring, distinct substrings | [Suffix Array](./topics/suffix-array/) | O(n log n) |
| Sort, find kth largest, merge sorted lists | [Divide & Conquer](./topics/divide-and-conquer/) | O(n log n) |
| Best local choice leads to global optimum | [Greedy](./topics/greedy/) | O(n log n) |
| Top K elements, kth largest/smallest | [Heap](./topics/heap/) | O(n log k) |
| Next greater element, monotonic structure | [Stack](./topics/stack/) | O(n) |
| Tree traversal in-order/pre/post | [Tree DFS](./topics/tree-dfs/) | O(n) |
| Level-order tree traversal, shortest path in tree | [Tree BFS](./topics/tree-bfs/) | O(n) |
| Insert/delete/search in sorted order | [BST](./topics/bst/) | O(log n) |
| Fast key-value lookup, counting frequencies | [HashMap](./topics/hash/) | O(1) avg |
| Reverse linked list, detect cycle, find middle | [Linked List](./topics/linked-list/) | O(n) |
| Base case + same structure on smaller input | [Recursion](./topics/recursion/) | Varies |

---

## Decision Flowchart

```
Is input sorted?
  YES → Binary Search or Two Pointers
  NO  → continue

Is it a graph/tree problem?
  YES, unweighted shortest path → BFS
  YES, weighted shortest path   → Dijkstra (no neg) / Bellman-Ford (neg)
  YES, dependency/ordering      → Topological Sort
  YES, connectivity             → DFS or Union Find
  NO  → continue

Does it involve substrings/prefixes?
  YES, prefix lookup            → Trie
  YES, repeated substrings      → Suffix Array
  NO  → continue

Does it ask for ALL solutions?
  YES → Backtracking
  NO  → continue

Does it ask for count/min/max and has overlapping subproblems?
  YES → Dynamic Programming
  NO  → continue

Contiguous subarray?
  YES → Sliding Window
  NO  → continue

Top K elements?
  YES → Heap
  NO  → continue

Local best choice = global best?
  YES → Greedy
```

---

## Common Mistake Patterns

| Mistake | Correct Approach |
|---------|-----------------|
| Using O(n²) nested loops on a subarray | Sliding window |
| BFS on a weighted graph | Dijkstra |
| DFS without visited set (infinite loop) | Always track visited |
| DP without defining state first | Write recurrence in English first |
| Backtracking without pruning (TLE) | Add validity check before recursing |
| Greedy when DP is needed | Verify exchange argument — if greedy doesn't provably work, use DP |

---

## Must-Do Rating Legend

Used in problem tables across all topic pages:

| Rating | Meaning |
|--------|---------|
| ✅ | Must-do — extremely common in interviews, foundational pattern |
| ⚡ | High value — good ROI, appears in top-tier interviews |
| 📖 | Learning problem — builds intuition, less common in interviews |
