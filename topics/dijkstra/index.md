---
title: "Dijkstra's Algorithm"
parent: Topics
nav_order: 21
---

## ELI5 (Explain Like I'm 5)

You're using Google Maps to find the fastest route. At each step, you always expand the nearest unvisited city first — never jumping to a far city when there's still a closer one to explore. You update travel times as you discover shortcuts. That's Dijkstra's — always process the cheapest known node next, and update neighbors if you found a shorter path.

## Explanation

- Dijkstra's finds the **shortest path from a source node to all other nodes** in a weighted graph (non-negative weights only)
- Uses a **min-heap** to always process the cheapest unvisited node next
- Maintains a `dist` array: `dist[node]` = shortest known distance from source
- When you pop a node from the heap, its distance is finalized (greedy property)

> Keyword trigger: _"shortest path"_, _"minimum cost to reach"_, _"weighted graph"_ → Dijkstra's (if no negative weights).

## When to use?

- Shortest path in a weighted graph with **non-negative** edge weights
- Network routing, GPS navigation problems
- "Minimum cost to travel from A to B" on a grid or graph
- When BFS alone isn't enough because edges have different weights

## Approach

```
def dijkstra(graph, source):
    dist = {node: infinity for all nodes}
    dist[source] = 0
    min_heap = [(0, source)]   # (distance, node)

    while min_heap:
        d, node = heappop(min_heap)
        if d > dist[node]:
            continue  # stale entry — skip
        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heappush(min_heap, (new_dist, neighbor))

    return dist
```

### Key Details
- Use a **lazy deletion** approach: push updated distances into the heap, skip stale entries when popped
- No explicit "visited" set needed — the `if d > dist[node]: continue` check handles it
- For single-target shortest path, stop early when the target is popped

## Notes

- Time Complexity: O((V + E) log V) with a binary heap
- Space Complexity: O(V + E) for the graph + heap
- **Does NOT work with negative edge weights** — use Bellman-Ford instead
- For unweighted graphs, use BFS (equivalent to Dijkstra with all weights = 1, but O(V + E))

## Data structures

- **Min-heap** (`heapq`) — always process the cheapest known node first
- **Distance array / dict** — tracks shortest known distance from source to each node
- **Adjacency list** — `graph[node]` = list of `(neighbor, weight)` pairs

## How to Master This

### Step-by-step
1. Understand why BFS doesn't work for weighted graphs — trace through a counterexample
2. Implement Dijkstra with a min-heap from scratch
3. Solve #743 (network delay time) — pure Dijkstra
4. Solve #1631 (minimum effort path) — Dijkstra on a grid
5. Solve #787 (cheapest flights within k stops) — constrained Dijkstra

### Key Resources
- 📹 [NeetCode — Dijkstra's Algorithm](https://www.youtube.com/watch?v=XB4MIexjvY0)
- 📹 [NeetCode — Network Delay Time](https://www.youtube.com/watch?v=EaphyqKU4PQ)
- 📝 [NeetCode.io — Advanced Graphs](https://neetcode.io/roadmap)
- 🔁 Drill: [#743](https://leetcode.com/problems/network-delay-time) → [#1631](https://leetcode.com/problems/path-with-minimum-effort) → [#787](https://leetcode.com/problems/cheapest-flights-within-k-stops) → [#1514](https://leetcode.com/problems/path-with-maximum-probability)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 743 | [Network Delay Time](https://leetcode.com/problems/network-delay-time) | Medium | High | ✅ |
| 1631 | [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort) | Medium | High | ✅ |
| 787 | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops) | Medium | High | ✅ |
| 1514 | [Path With Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability) | Medium | Medium | ⚡ |
| 778 | [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water) | Hard | Medium | ⚡ |

## Code Samples

1. Network Delay Time — classic Dijkstra

```python
import heapq
from collections import defaultdict

def networkDelayTime(times: list[list[int]], n: int, k: int) -> int:
    # build adjacency list: source → [(weight, dest)]
    graph = defaultdict(list)
    for u, v, w in times:
        graph[u].append((w, v))

    dist = {i: float('inf') for i in range(1, n + 1)}
    dist[k] = 0
    min_heap = [(0, k)]  # (distance, node)

    while min_heap:
        d, node = heapq.heappop(min_heap)

        if d > dist[node]:
            continue  # stale entry — already found a shorter path

        for weight, neighbor in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(min_heap, (new_dist, neighbor))

    max_dist = max(dist.values())
    return max_dist if max_dist < float('inf') else -1
```

2. Path With Minimum Effort — Dijkstra on a grid

```python
import heapq

def minimumEffortPath(heights: list[list[int]]) -> int:
    rows, cols = len(heights), len(heights[0])
    # effort[r][c] = min max-difference to reach (r,c) from (0,0)
    effort = [[float('inf')] * cols for _ in range(rows)]
    effort[0][0] = 0
    min_heap = [(0, 0, 0)]  # (effort, row, col)

    while min_heap:
        e, r, c = heapq.heappop(min_heap)

        if e > effort[r][c]:
            continue

        if r == rows - 1 and c == cols - 1:
            return e  # reached destination

        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                new_effort = max(e, abs(heights[nr][nc] - heights[r][c]))
                if new_effort < effort[nr][nc]:
                    effort[nr][nc] = new_effort
                    heapq.heappush(min_heap, (new_effort, nr, nc))

    return effort[rows-1][cols-1]
```
