---
title: "Bellman-Ford"
parent: Topics
nav_order: 23
---

## ELI5 (Explain Like I'm 5)

Imagine you're finding the cheapest flight from city A to city B. Some routes have "discount" edges that actually subtract cost (negative weights). Dijkstra's breaks with these discounts. Bellman-Ford works differently: it relaxes ALL edges, over and over, n-1 times. Each round it finds paths one hop longer than before. After n-1 rounds, it's found all shortest paths — and if prices keep dropping after that, it means there's an infinite discount loop (negative cycle).

## Explanation

- Bellman-Ford computes **shortest paths from a source to all nodes**, even with **negative edge weights**
- Runs **V-1 iterations** of relaxing all edges — after k iterations, it has found all shortest paths using at most k edges
- On the V-th iteration, if any distance still improves → **negative cycle detected**
- Slower than Dijkstra (O(VE) vs O((V+E)logV)) but handles negative weights

> Keyword trigger: _"negative weights"_, _"detect negative cycle"_, _"cheapest flights with k stops"_ → Bellman-Ford.

## When to use?

- Shortest path with **negative edge weights** (Dijkstra breaks here)
- Detecting **negative cycles** in a graph
- Shortest path with a constraint on **number of edges** (e.g., at most k hops)
- Currency arbitrage detection

## Approach

```
dist = [infinity] * n
dist[source] = 0

for i in range(n - 1):          # V-1 relaxation rounds
    for each edge (u, v, w):
        if dist[u] + w < dist[v]:
            dist[v] = dist[u] + w

# check for negative cycles (V-th round)
for each edge (u, v, w):
    if dist[u] + w < dist[v]:
        # negative cycle exists
```

### K-stops variant
Limit relaxation to k rounds (not V-1). This gives shortest paths using at most k edges.

## Notes

- Time Complexity: O(V × E)
- Space Complexity: O(V)
- For the k-stops variant, make a **copy of dist at the start of each round** — don't use updated values within the same round (prevents using more than k edges)
- If the graph has no negative weights, use Dijkstra's (faster)
- SPFA (Shortest Path Faster Algorithm) is an optimized Bellman-Ford with a queue, average O(E) but same worst case

## Data structures

- **Distance array** — `dist[i]` = shortest distance from source to node i
- **Edge list** — Bellman-Ford iterates over all edges, so an edge list is more natural than an adjacency list

## How to Master This

### Step-by-step
1. Implement the basic template — 3 loops (rounds, edges, relax)
2. Solve #743 with Bellman-Ford to see the difference vs Dijkstra
3. Solve #787 (cheapest flights within k stops) — the k-stops variant
4. Understand negative cycle detection as a follow-up

### Key Resources
- 📹 [NeetCode — Cheapest Flights Within K Stops](https://www.youtube.com/watch?v=5eIK3zUdYmE)
- 📝 [CP-Algorithms — Bellman-Ford](https://cp-algorithms.com/graph/bellman_ford.html)
- 🔁 Drill: [#787](https://leetcode.com/problems/cheapest-flights-within-k-stops) → [#743](https://leetcode.com/problems/network-delay-time)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 787 | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops) | Medium | High | ✅ |
| 743 | [Network Delay Time](https://leetcode.com/problems/network-delay-time) | Medium | High | ⚡ |
| 1334 | [Find the City With Smallest Number of Neighbors](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance) | Medium | Medium | ⚡ |

## Code Samples

1. Cheapest Flights Within K Stops — Bellman-Ford with k-round limit

```python
def findCheapestPrice(n: int, flights: list[list[int]], src: int, dst: int, k: int) -> int:
    # dist[i] = cheapest price from src to city i
    dist = [float('inf')] * n
    dist[src] = 0

    # relax edges k+1 times (k stops = k+1 edges)
    for _ in range(k + 1):
        # IMPORTANT: use a copy to not allow more than one edge per round
        temp = dist[:]
        for u, v, price in flights:
            if dist[u] != float('inf') and dist[u] + price < temp[v]:
                temp[v] = dist[u] + price
        dist = temp

    return dist[dst] if dist[dst] != float('inf') else -1
```

2. Basic Bellman-Ford — shortest paths with negative edge support

```python
def bellman_ford(n: int, edges: list[tuple[int, int, int]], source: int) -> list[float]:
    """
    Returns shortest distances from source to all nodes.
    Returns None if a negative cycle is detected.
    edges: list of (u, v, weight)
    """
    dist = [float('inf')] * n
    dist[source] = 0

    # V-1 relaxation rounds
    for _ in range(n - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                updated = True
        if not updated:
            break  # early termination if no updates

    # V-th round: detect negative cycle
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None  # negative cycle exists

    return dist
```
