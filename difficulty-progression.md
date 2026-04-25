---
title: Difficulty Progression
nav_order: 6
---

# Difficulty Progression

A structured, ordered approach to tackling all topics — from zero to interview-ready. Follow the phases in order. Don't skip ahead.

---

## Phase 0 — Foundations (Week 1–2)

Master these first. Every other topic builds on them.

| Order | Topic | Key Concept | First Problem |
|-------|-------|-------------|---------------|
| 1 | [Complexity](./topics/complexity/) | Big O, time/space analysis | Understand before coding anything |
| 2 | [HashMap](./topics/hash/) | O(1) lookup, frequency counting | [#1 Two Sum](https://leetcode.com/problems/two-sum) |
| 3 | [Two Pointers](./topics/two-pointers/) | Left/right converging | [#125 Valid Palindrome](https://leetcode.com/problems/valid-palindrome) |
| 4 | [Sliding Window](./topics/sliding-window/) | Moving subarray | [#643 Max Avg Subarray I](https://leetcode.com/problems/maximum-average-subarray-i) |
| 5 | [Stack](./topics/stack/) | LIFO, next greater element | [#20 Valid Parentheses](https://leetcode.com/problems/valid-parentheses) |

**Goal:** Solve 2–3 Easy problems per topic. No Hard problems yet.

---

## Phase 1 — Core Data Structures (Week 3–4)

| Order | Topic | Key Concept | First Problem |
|-------|-------|-------------|---------------|
| 6 | [Recursion](./topics/recursion/) | Base case + subproblem | [#509 Fibonacci Number](https://leetcode.com/problems/fibonacci-number) |
| 7 | [Linked List](./topics/linked-list/) | Two pointers on list, dummy node | [#206 Reverse Linked List](https://leetcode.com/problems/reverse-linked-list) |
| 8 | [Tree DFS](./topics/tree-dfs/) | Pre/in/post order | [#104 Max Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree) |
| 9 | [Tree BFS](./topics/tree-bfs/) | Level order, queue | [#102 Binary Tree Level Order](https://leetcode.com/problems/binary-tree-level-order-traversal) |
| 10 | [BST](./topics/bst/) | Search, insert, validate | [#700 Search in a BST](https://leetcode.com/problems/search-in-a-binary-search-tree) |
| 11 | [Heap](./topics/heap/) | Top K elements, priority queue | [#703 Kth Largest in Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream) |

**Goal:** Mix of Easy and Medium. Start attempting Mediums independently.

---

## Phase 2 — Algorithms (Week 5–7)

| Order | Topic | Key Concept | First Problem |
|-------|-------|-------------|---------------|
| 12 | [Binary Search](./topics/binary-search/) | Eliminate half, boundary conditions | [#704 Binary Search](https://leetcode.com/problems/binary-search) |
| 13 | [Graph BFS](./topics/graph-bfs/) | Grid traversal, connected components | [#200 Number of Islands](https://leetcode.com/problems/number-of-islands) |
| 14 | [Graph DFS](./topics/graph-dfs/) | Cycle detection, DFS on explicit graph | [#133 Clone Graph](https://leetcode.com/problems/clone-graph) |
| 15 | [Greedy](./topics/greedy/) | Sort by key, exchange argument | [#455 Assign Cookies](https://leetcode.com/problems/assign-cookies) |
| 16 | [Backtracking](./topics/combinatorial-dfs/) | Make/recurse/undo template | [#78 Subsets](https://leetcode.com/problems/subsets) |
| 17 | [Divide & Conquer](./topics/divide-and-conquer/) | Merge sort, quick select | [#215 Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array) |

**Goal:** All Mediums. Begin attempting Easy Hards.

---

## Phase 3 — Dynamic Programming (Week 8–10)

DP is the hardest topic. Spend 2–3 weeks here.

| Order | Sub-topic | First Problem | Hard version |
|-------|-----------|---------------|--------------|
| 1D DP | Single variable | [#70 Climbing Stairs](https://leetcode.com/problems/climbing-stairs) | [#139 Word Break](https://leetcode.com/problems/word-break) |
| 1D DP | Skip/take decision | [#198 House Robber](https://leetcode.com/problems/house-robber) | [#213 House Robber II](https://leetcode.com/problems/house-robber-ii) |
| 1D DP | Unbounded knapsack | [#322 Coin Change](https://leetcode.com/problems/coin-change) | [#518 Coin Change II](https://leetcode.com/problems/coin-change-ii) |
| 2D DP | Subsequences | [#300 Longest Inc. Subsequence](https://leetcode.com/problems/longest-increasing-subsequence) | [#1143 LCS](https://leetcode.com/problems/longest-common-subsequence) |
| 2D DP | Grid DP | [#62 Unique Paths](https://leetcode.com/problems/unique-paths) | [#64 Min Path Sum](https://leetcode.com/problems/minimum-path-sum) |

**Goal:** 15–20 DP problems minimum. Understand the pattern for each sub-type.

---

## Phase 4 — Advanced Graphs (Week 11–12)

| Order | Topic | Key Concept | First Problem |
|-------|-------|-------------|---------------|
| 18 | [Topological Sort](./topics/topological-sort/) | DAG ordering, cycle detection | [#207 Course Schedule](https://leetcode.com/problems/course-schedule) |
| 19 | [Union Find](./topics/union-find/) | DSU template, path compression | [#547 Number of Provinces](https://leetcode.com/problems/number-of-provinces) |
| 20 | [Dijkstra](./topics/dijkstra/) | Weighted shortest path | [#743 Network Delay Time](https://leetcode.com/problems/network-delay-time) |
| 21 | [Bellman-Ford](./topics/bellman-ford/) | Negative weights, k-stops | [#787 Cheapest Flights K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops) |
| 22 | [A* Search](./topics/astar/) | Heuristic-guided search | [#1091 Shortest Path Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix) |

---

## Phase 5 — Advanced Data Structures (Week 13–14)

| Order | Topic | Use Case | First Problem |
|-------|-------|----------|---------------|
| 23 | [Trie](./topics/trie/) | Prefix matching | [#208 Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree) |
| 24 | [Segment Tree](./topics/segment-tree/) | Range queries + updates | [#307 Range Sum Query Mutable](https://leetcode.com/problems/range-sum-query-mutable) |
| 25 | [Fenwick Tree](./topics/fenwick-tree/) | Prefix sums + updates | [#307 Range Sum Query Mutable](https://leetcode.com/problems/range-sum-query-mutable) |
| 26 | [AVL Trees](./topics/avl-trees/) | Self-balancing BST | Concept + #1382 |
| 27 | [Suffix Array](./topics/suffix-array/) | Advanced string problems | [#1044 Longest Dup Substring](https://leetcode.com/problems/longest-duplicate-substring) |

---

## Recommended Weekly Targets

| Week | Focus | Problems/Week |
|------|-------|---------------|
| 1–2 | Phase 0 (foundations) | 10–15 Easy |
| 3–4 | Phase 1 (data structures) | 15–20 Easy+Medium |
| 5–7 | Phase 2 (algorithms) | 20–25 Medium |
| 8–10 | Phase 3 (DP) | 20–25 Medium+Hard |
| 11–12 | Phase 4 (advanced graphs) | 15–20 Medium+Hard |
| 13–14 | Phase 5 (advanced DS) | 10–15 Hard |

---

## Progress Checkpoints

Before moving to the next phase, make sure you can:

- **After Phase 0:** Solve any Easy problem involving arrays, strings, or hash maps within 15 minutes
- **After Phase 1:** Implement a BST, traverse a tree in all orders, and reverse a linked list from memory
- **After Phase 2:** Solve most LeetCode Mediums in graph, binary search, and backtracking topics
- **After Phase 3:** Recognize DP problems, define the state, and write the recurrence before coding
- **After Phase 4:** Implement Dijkstra and topological sort from memory
- **After Phase 5:** Know when each advanced DS applies and implement the core template

---

## Problem Count Target for Interviews

| Level | Problems Solved | Ready For |
|-------|----------------|-----------|
| 50 | Mostly Easy | Initial screening, startups |
| 100 | Easy + Medium | Most mid-level roles |
| 150 | Mix + some Hard | FAANG / top-tier roles |
| 200+ | All patterns + Hard | Senior / Staff interviews |

Quality > quantity. 75 well-understood problems beat 200 problems you half-remember.
