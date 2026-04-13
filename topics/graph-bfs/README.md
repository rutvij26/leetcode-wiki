---
title: "Graph BFS / DFS on Grids"
parent: Topics
nav_order: 11
---

## ELI5 (Explain Like I'm 5)

Imagine a maze drawn on graph paper. Each square connects to the squares directly above, below, left, and right. **BFS** is like dropping a stone in a pond — ripples spread out in all directions at the same speed, reaching nearby squares first. **DFS** is like following one path as far as it goes, then backtracking when you hit a wall. Both let you explore every square.

## Explanation

- A 2D grid is just a graph where each cell `(r, c)` connects to its 4 neighbors
- **BFS** (queue): explores level by level — finds the shortest path in an unweighted grid
- **DFS** (recursion): goes deep into one region before backtracking — counts connected components, fills regions
- Always need: **bounds checking** + a **visited marker** to avoid revisiting cells

> Directions array — use this template for all grid problems:
> `const dirs = [[0,1],[0,-1],[1,0],[-1,0]];`

## When to use?

- **BFS**: shortest path on a grid, spreading infections, minimum steps
- **DFS**: counting islands/regions, flood fill, detecting connected components
- Any problem where "explore all reachable cells from a starting point" is needed

## Approach

### BFS on a grid (shortest path):
1. Start: add the source cell to a queue, mark it visited
2. While queue is not empty: dequeue a cell, process it, add unvisited neighbors to the queue
3. Track distance by level (increment `steps` after processing each full level)

### DFS on a grid (flood fill / count regions):
1. For each unvisited cell matching the target: call DFS, increment region count
2. In DFS: if out of bounds, already visited, or wrong type → return
3. Mark cell visited, recurse into all 4 neighbors

### Algorithm Steps (DFS — Number of Islands):
1. Scan the grid for `'1'` cells
2. When found: call `dfs(r, c)` and increment island count
3. In `dfs`: mark `grid[r][c] = '0'` (visited), recurse into 4 neighbors
4. Repeat until all cells visited

## Notes

- Time Complexity: O(rows × cols) — each cell is visited at most once
- Space Complexity: O(rows × cols) — visited array or queue in worst case
- You can mark visited by mutating the grid (e.g. set `'1'` to `'0'`) or with a separate `visited` set
- BFS guarantees the **shortest** path; DFS does not

## Data structures

- **Queue** (`array` used as queue with `shift()`) — for BFS
- **Recursion / call stack** — for DFS
- **Visited set or modified grid** — to avoid revisiting

## How to Master This

### Step-by-step
1. Read this page and memorize the directions array — it's the same in every grid problem
2. Watch the NeetCode video on Number of Islands (the canonical grid DFS problem)
3. Solve #200 (DFS, count components) and #733 (DFS, flood fill) back to back
4. Solve #994 (BFS, level-by-level spread) — notice how the queue replaces the recursion
5. After these four, you have 90% of grid problems covered

### Key Resources
- 📹 [NeetCode — Number of Islands](https://www.youtube.com/watch?v=pV2kpPD66nE)
- 📹 [NeetCode — Rotting Oranges](https://www.youtube.com/watch?v=y704fEOx0s0)
- 📝 [NeetCode.io — Graphs problems](https://neetcode.io/roadmap) (see "Graphs" section)
- 🔁 Drill in this order: [#733](https://leetcode.com/problems/flood-fill) → [#200](https://leetcode.com/problems/number-of-islands) → [#695](https://leetcode.com/problems/max-area-of-island) → [#994](https://leetcode.com/problems/rotting-oranges)

## Sample LeetCode problems

- [#733 Flood Fill](https://leetcode.com/problems/flood-fill)
- [#200 Number of Islands](https://leetcode.com/problems/number-of-islands)
- [#695 Max Area of Island](https://leetcode.com/problems/max-area-of-island)
- [#994 Rotting Oranges](https://leetcode.com/problems/rotting-oranges)

## Code Samples

1. Number of Islands ( DFS — count connected components )

```typescript
/**
 * Count the number of islands (connected groups of '1's) in a grid
 * Input:
 *   11110
 *   11010
 *   11000
 *   00000
 * Output: 1
 */
function numIslands(grid: string[][]): number {
  const rows = grid.length;
  const cols = grid[0].length;
  let islands = 0;

  // 4-directional movement: right, left, down, up
  const dirs = [[0, 1], [0, -1], [1, 0], [-1, 0]];

  function dfs(r: number, c: number): void {
    // out of bounds, already visited (marked '0'), or water
    if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] !== '1') {
      return;
    }

    // mark as visited by sinking the land
    grid[r][c] = '0';

    // explore all 4 neighbors
    for (const [dr, dc] of dirs) {
      dfs(r + dr, c + dc);
    }
  }

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c] === '1') {
        dfs(r, c); // sink the entire island
        islands++;
      }
    }
  }

  return islands;
}
```

2. Rotting Oranges ( BFS — level-by-level spread )

```typescript
/**
 * Every minute, fresh oranges adjacent to rotten ones become rotten.
 * Return the minimum minutes until no fresh oranges remain, or -1 if impossible.
 * 0 = empty, 1 = fresh, 2 = rotten
 */
function orangesRotting(grid: number[][]): number {
  const rows = grid.length;
  const cols = grid[0].length;
  const dirs = [[0, 1], [0, -1], [1, 0], [-1, 0]];

  const queue: [number, number][] = [];
  let fresh = 0;

  // find all initially rotten oranges and count fresh ones
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c] === 2) queue.push([r, c]);
      if (grid[r][c] === 1) fresh++;
    }
  }

  let minutes = 0;

  // BFS from all rotten oranges simultaneously
  while (queue.length > 0 && fresh > 0) {
    const levelSize = queue.length;
    for (let i = 0; i < levelSize; i++) {
      const [r, c] = queue.shift()!;
      for (const [dr, dc] of dirs) {
        const nr = r + dr;
        const nc = c + dc;
        // only spread to fresh oranges
        if (nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] === 1) {
          grid[nr][nc] = 2; // rot it
          fresh--;
          queue.push([nr, nc]);
        }
      }
    }
    minutes++;
  }

  // if any fresh oranges remain, they're unreachable
  return fresh === 0 ? minutes : -1;
}
```
