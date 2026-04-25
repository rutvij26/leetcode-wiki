---
title: "Union Find"
parent: Topics
nav_order: 20
---

## ELI5 (Explain Like I'm 5)

Imagine a school where students form friend groups. At first, everyone is their own group. When two students become friends, you merge their groups. Later, if someone asks "are these two students in the same group?", you can answer instantly. Union Find is exactly this — a data structure for tracking who's connected to whom, with super-fast merge and lookup.

## Explanation

- Union Find (Disjoint Set Union / DSU) maintains a collection of disjoint sets
- Two operations: **find(x)** — which group is x in? **union(x, y)** — merge x's and y's groups
- Two optimizations make both operations nearly O(1):
  1. **Path compression** — when finding root, flatten the tree so everyone points directly to root
  2. **Union by rank** — always attach smaller tree under larger tree

> Keyword trigger: _"connected components"_, _"detect cycle in undirected graph"_, _"minimum spanning tree"_, _"number of groups"_ → Union Find.

## When to use?

- Dynamically connecting nodes and checking connectivity
- Detecting cycles in undirected graphs
- Minimum spanning tree (Kruskal's algorithm)
- Counting connected components as edges are added
- Grouping problems where merges happen over time

## Approach

### Template

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))  # each node is its own parent
        self.rank = [0] * n           # tree height (for union by rank)

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False  # already connected
        # union by rank: attach smaller tree under larger
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True
```

### Cycle Detection
If `union(x, y)` returns False, x and y were already in the same component → adding this edge creates a cycle.

### Count Components
Start with `n` components. Each successful `union` (returns True) decreases count by 1.

## Notes

- Time Complexity: O(α(n)) per operation — α is the inverse Ackermann function, effectively O(1)
- Space Complexity: O(n) for parent and rank arrays
- Path compression + union by rank together give the O(α(n)) bound — use both
- For grid problems, you can map (row, col) → index with `row * cols + col`

## Data structures

- **Parent array** — `parent[i]` = parent of node i (root points to itself)
- **Rank array** — approximate tree height, used to keep trees shallow

## How to Master This

### Step-by-step
1. Write the template from memory — it's short and always the same
2. Solve #547 (number of provinces) — direct connected components
3. Solve #684 (redundant connection) — cycle detection
4. Solve #1584 (min cost to connect all points) — Kruskal's with Union Find
5. Solve #128 (longest consecutive sequence) — Union Find on numbers

### Key Resources
- 📹 [NeetCode — Union Find](https://www.youtube.com/watch?v=ayW5B2W9hfo)
- 📹 [NeetCode — Redundant Connection](https://www.youtube.com/watch?v=FXWRE9PXlAQ)
- 📝 [NeetCode.io — Advanced Graphs section](https://neetcode.io/roadmap)
- 🔁 Drill: [#547](https://leetcode.com/problems/number-of-provinces) → [#684](https://leetcode.com/problems/redundant-connection) → [#128](https://leetcode.com/problems/longest-consecutive-sequence) → [#1584](https://leetcode.com/problems/min-cost-to-connect-all-points)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 547 | [Number of Provinces](https://leetcode.com/problems/number-of-provinces) | Medium | High | ✅ |
| 684 | [Redundant Connection](https://leetcode.com/problems/redundant-connection) | Medium | High | ✅ |
| 128 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence) | Medium | Very High | ✅ |
| 1584 | [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points) | Medium | Medium | ⚡ |
| 990 | [Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations) | Medium | Medium | ⚡ |
| 721 | [Accounts Merge](https://leetcode.com/problems/accounts-merge) | Medium | High | ✅ |

## Code Samples

1. Number of Provinces — count connected components

```python
def findCircleNum(isConnected: list[list[int]]) -> int:
    n = len(isConnected)
    parent = list(range(n))
    rank = [0] * n

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])  # path compression
        return parent[x]

    def union(x, y):
        px, py = find(x), find(y)
        if px == py:
            return False
        if rank[px] < rank[py]:
            px, py = py, px
        parent[py] = px
        if rank[px] == rank[py]:
            rank[px] += 1
        return True

    components = n
    for i in range(n):
        for j in range(i + 1, n):
            if isConnected[i][j] == 1:
                if union(i, j):
                    components -= 1  # merged two components

    return components
```

2. Redundant Connection — find edge that creates a cycle

```python
def findRedundantConnection(edges: list[list[int]]) -> list[int]:
    n = len(edges)
    parent = list(range(n + 1))
    rank = [0] * (n + 1)

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        px, py = find(x), find(y)
        if px == py:
            return False  # already connected — this edge is redundant
        if rank[px] < rank[py]:
            px, py = py, px
        parent[py] = px
        if rank[px] == rank[py]:
            rank[px] += 1
        return True

    for u, v in edges:
        if not union(u, v):
            return [u, v]  # this edge created a cycle

    return []
```
