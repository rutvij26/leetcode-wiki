---
title: "Graph DFS"
parent: Topics
nav_order: 14
---

## ELI5 (Explain Like I'm 5)

Imagine exploring a cave system. You pick one tunnel, go as deep as you can until you hit a dead end, then backtrack and try the next tunnel. You mark every room you've visited so you don't go in circles. That's Graph DFS — go deep first, backtrack when stuck, mark everything visited.

## Explanation

- Graph DFS explores nodes by going as **deep as possible** before backtracking
- Unlike Tree DFS, graphs can have **cycles** — always track visited nodes
- Can be implemented **recursively** (call stack) or **iteratively** (explicit stack)
- Works on both directed and undirected graphs

> Keyword trigger: _"connected components"_, _"detect cycle"_, _"path exists between"_, _"count islands"_ → Graph DFS.

## When to use?

- Finding connected components in a graph
- Cycle detection in directed or undirected graphs
- Checking if a path exists between two nodes
- Topological sort (DFS-based variant)
- Solving maze/grid problems where you explore all possibilities

## Approach

### Recursive DFS
```
visited = set()

def dfs(node):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor)
```

### Iterative DFS (explicit stack)
```
def dfs(start):
    visited = set()
    stack = [start]
    while stack:
        node = stack.pop()
        if node in visited:
            continue
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)
```

### Connected Components
```
def count_components(n, edges):
    # build adjacency list, then DFS from each unvisited node
    count = 0
    for node in range(n):
        if node not in visited:
            dfs(node)
            count += 1
    return count
```

## Notes

- Time Complexity: O(V + E) — visit each vertex and edge once
- Space Complexity: O(V) — visited set + recursion stack
- **Always mark visited before recursing**, not after — prevents infinite loops
- Recursion depth can hit Python's default limit (~1000) on large graphs — use iterative or `sys.setrecursionlimit`
- For cycle detection in directed graphs, track nodes in the **current path** (not just visited)

## Data structures

- **Adjacency list** (dict or list of lists) — standard graph representation
- **Visited set** — prevents revisiting nodes / infinite loops
- **Stack** — iterative DFS; call stack implicitly used in recursive DFS
- **Path set** — for cycle detection in directed graphs (nodes currently on the DFS path)

## How to Master This

### Step-by-step
1. Make sure you're solid on Tree DFS first — Graph DFS is the same with a visited set added
2. Solve #200 (number of islands) — classic connected components on a grid
3. Solve #133 (clone graph) — DFS on an explicit graph with node objects
4. Solve #207 (course schedule) — cycle detection in a directed graph
5. Solve #417 (Pacific Atlantic water flow) — DFS from multiple sources

### Key Resources
- 📹 [NeetCode — Number of Islands](https://www.youtube.com/watch?v=pV2kpPD66nE)
- 📹 [NeetCode — Course Schedule](https://www.youtube.com/watch?v=EgI5nU9etnU)
- 📝 [NeetCode.io — Graphs section](https://neetcode.io/roadmap)
- 🔁 Drill: [#200](https://leetcode.com/problems/number-of-islands) → [#133](https://leetcode.com/problems/clone-graph) → [#207](https://leetcode.com/problems/course-schedule) → [#417](https://leetcode.com/problems/pacific-atlantic-water-flow)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 200 | [Number of Islands](https://leetcode.com/problems/number-of-islands) | Medium | Very High | ✅ |
| 133 | [Clone Graph](https://leetcode.com/problems/clone-graph) | Medium | High | ✅ |
| 207 | [Course Schedule](https://leetcode.com/problems/course-schedule) | Medium | Very High | ✅ |
| 417 | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow) | Medium | High | ✅ |
| 130 | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions) | Medium | Medium | ⚡ |
| 323 | [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph) | Medium | High | ✅ |

## Code Samples

1. Number of Islands — DFS on a grid

```python
def numIslands(grid: list[list[str]]) -> int:
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    visited = set()
    count = 0

    def dfs(r, c):
        # out of bounds, water, or already visited
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return
        if grid[r][c] == '0' or (r, c) in visited:
            return

        visited.add((r, c))
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1' and (r, c) not in visited:
                dfs(r, c)
                count += 1

    return count
```

2. Course Schedule — cycle detection in directed graph

```python
from collections import defaultdict

def canFinish(numCourses: int, prerequisites: list[list[int]]) -> bool:
    # build adjacency list
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[course].append(prereq)

    # 0 = unvisited, 1 = in current path (cycle!), 2 = fully processed
    state = [0] * numCourses

    def dfs(node: int) -> bool:
        if state[node] == 1:
            return False  # cycle detected
        if state[node] == 2:
            return True   # already verified — no cycle from here

        state[node] = 1  # mark as in-progress
        for neighbor in graph[node]:
            if not dfs(neighbor):
                return False

        state[node] = 2  # mark as fully processed
        return True

    return all(dfs(course) for course in range(numCourses))
```
