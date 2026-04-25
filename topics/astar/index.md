---
title: "A* Search"
parent: Topics
nav_order: 22
---

## ELI5 (Explain Like I'm 5)

Dijkstra explores in all directions equally — like a ripple spreading out from a stone in water. A* is smarter: it uses a compass (a heuristic) that points toward the destination. It explores nodes that are both close to the start AND close to the goal first. This makes it much faster when you know roughly where you're going.

## Explanation

- A* is an **informed shortest path algorithm** — it uses a heuristic function to guide the search toward the target
- It maintains two costs per node: `g(n)` = actual cost from start, `h(n)` = estimated cost to goal
- Evaluates nodes by `f(n) = g(n) + h(n)` and always processes the lowest `f(n)` first
- With an **admissible heuristic** (never overestimates), A* always finds the optimal path
- A* = Dijkstra + heuristic guidance

> Use A* when: you have a single target, the search space is large, and you can define a good heuristic (e.g., Manhattan distance on a grid).

## When to use?

- Grid pathfinding (games, robotics)
- Single-source single-target shortest path (not all-pairs)
- When Dijkstra is too slow and you have domain knowledge for a heuristic
- Puzzle solving (8-puzzle, 15-puzzle)

## Approach

```
f(n) = g(n) + h(n)
  g(n) = actual cost from start to n
  h(n) = heuristic estimate from n to goal

open_set = min-heap by f(n)
push (f(start), 0, start)    # (f, g, node)
g_score = {start: 0}

while open_set:
    f, g, node = heappop(open_set)
    if node == goal: return g
    if g > g_score[node]: continue   # stale entry

    for neighbor, weight in graph[node]:
        new_g = g + weight
        if new_g < g_score.get(neighbor, inf):
            g_score[neighbor] = new_g
            h = heuristic(neighbor, goal)
            heappush(open_set, (new_g + h, new_g, neighbor))
```

### Common Heuristics
- **Manhattan distance** (grid, 4-directional): `|r1-r2| + |c1-c2|`
- **Euclidean distance** (grid, any direction): `sqrt((r1-r2)² + (c1-c2)²)`
- **Chebyshev distance** (grid, 8-directional): `max(|r1-r2|, |c1-c2|)`
- **0** (no heuristic) → degenerates to Dijkstra

## Notes

- Time Complexity: O(E log V) — same as Dijkstra in the worst case, but typically much faster in practice
- A* is **optimal** only if `h(n)` is admissible (never overestimates true cost)
- A* is rare in LeetCode but common in system design, robotics, and game dev interviews
- For most LeetCode grid problems, Dijkstra or BFS is sufficient — A* shines at scale

## Data structures

- **Min-heap** — priority queue ordered by `f(n)`
- **g_score dict** — best known actual cost to each node
- **Heuristic function** — domain-specific estimate to goal

## How to Master This

### Step-by-step
1. Master Dijkstra first — A* is a natural extension
2. Understand what makes a heuristic admissible
3. Solve #1091 (shortest path in binary matrix) with BFS first, then try A*
4. Solve #505 (the maze II) — weighted grid, good A* candidate

### Key Resources
- 📹 [Sebastian Lague — A* Pathfinding](https://www.youtube.com/watch?v=-L-WgKMFuhE)
- 📝 [Red Blob Games — A* Introduction](https://www.redblobgames.com/pathfinding/a-star/introduction.html) (best visual explanation on the internet)
- 🔁 Drill: [#1091](https://leetcode.com/problems/shortest-path-in-binary-matrix) → [#505](https://leetcode.com/problems/the-maze-ii)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 1091 | [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix) | Medium | Medium | ⚡ |
| 505 | [The Maze II](https://leetcode.com/problems/the-maze-ii) | Medium | Low | 📖 |
| 1263 | [Minimum Moves to Move a Box to Target](https://leetcode.com/problems/minimum-moves-to-move-a-box-to-their-target-location) | Hard | Low | 📖 |

## Code Samples

1. Shortest Path in Binary Matrix — A* with Chebyshev heuristic

```python
import heapq

def shortestPathBinaryMatrix(grid: list[list[int]]) -> int:
    n = len(grid)
    if grid[0][0] == 1 or grid[n-1][n-1] == 1:
        return -1
    if n == 1:
        return 1

    def heuristic(r, c):
        # Chebyshev distance — 8-directional movement
        return max(n - 1 - r, n - 1 - c)

    # (f_score, g_score, row, col)
    heap = [(1 + heuristic(0, 0), 1, 0, 0)]
    g_score = [[float('inf')] * n for _ in range(n)]
    g_score[0][0] = 1

    directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]

    while heap:
        f, g, r, c = heapq.heappop(heap)

        if r == n - 1 and c == n - 1:
            return g

        if g > g_score[r][c]:
            continue  # stale entry

        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and grid[nr][nc] == 0:
                new_g = g + 1
                if new_g < g_score[nr][nc]:
                    g_score[nr][nc] = new_g
                    heapq.heappush(heap, (new_g + heuristic(nr, nc), new_g, nr, nc))

    return -1
```
