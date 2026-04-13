---
title: "Greedy"
---

## ELI5 (Explain Like I'm 5)

Imagine you have a bag with limited space and a pile of snacks — each snack has a different size and tastiness. Greedy says: always grab the tastiest snack per bite first. You never plan ahead or reconsider — just always pick the best available option right now. It sounds reckless, but for certain problems it actually gets you the best total outcome.

## Explanation

- A greedy algorithm makes the **locally optimal choice at each step**, hoping the result is globally optimal
- It works when you can prove that a greedy choice never needs to be undone
- The typical setup: **sort by some key**, then iterate and always pick the "best" next item
- The proof technique: **exchange argument** — if you swapped any two choices in the result, the outcome wouldn't improve

> If you're unsure whether greedy works: ask "what if I swapped two adjacent choices? Would the result get better or stay the same?" If swapping never helps, greedy is valid.

## When to use?

- Optimization problems asking for max/min of something (units, profit, meetings)
- Interval scheduling (meeting rooms, overlapping intervals)
- The input can be sorted by a key that naturally drives the optimal order
- Dynamic programming feels too slow and the problem has a clear "take the best now" structure

## Approach

1. **Identify the sorting key** — what property determines priority? (value/unit, end time, weight, etc.)
2. **Sort** the input by that key
3. **Iterate and take greedily** — at each step, take the best available item that still satisfies constraints
4. Repeat until done or constraint is met

### Algorithm Steps (Non-overlapping Intervals — remove fewest):
1. Sort intervals by **end time** (not start time — this is the key insight)
2. Track the end time of the last interval we kept
3. For each interval: if it starts after the last kept end → keep it (update last end). Otherwise → remove it (increment counter)

## Notes

- Time Complexity: O(n log n) — usually dominated by the sort
- Space Complexity: O(1) — just a few tracking variables (excluding sort overhead)
- Greedy does **not** always work — if unsure, think about a counterexample
- When greedy doesn't work → try Dynamic Programming

## Data structures

- **Array** — after sorting
- **Variables** — to track running state (capacity used, last interval end, current sum)

## How to Master This

### Step-by-step
1. Read this page and understand the exchange argument
2. Watch the NeetCode video — the interval scheduling insight (sort by end time, not start time) is non-obvious
3. Solve #1710 (straightforward: sort by value, take greedily until capacity runs out)
4. Solve #435 (interval scheduling — the key is sorting by end time)
5. Solve #55 (jump game — track the furthest reachable index)

### Key Resources
- 📹 [NeetCode — Non-overlapping Intervals](https://www.youtube.com/watch?v=nONCGxWoUfM)
- 📹 [NeetCode — Jump Game](https://www.youtube.com/watch?v=Yan0cv2cLy8)
- 📝 [NeetCode.io — Greedy problems](https://neetcode.io/roadmap) (see "Greedy" section)
- 🔁 Drill in this order: [#1710](https://leetcode.com/problems/maximum-units-on-a-truck) → [#55](https://leetcode.com/problems/jump-game) → [#435](https://leetcode.com/problems/non-overlapping-intervals) → [#452](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons)

## Sample LeetCode problems

- [#1710 Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck)
- [#55 Jump Game](https://leetcode.com/problems/jump-game)
- [#435 Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals)
- [#452 Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons)

## Code Samples

1. Maximum Units on a Truck ( sort by value, take greedily )

```typescript
/**
 * Pack boxes onto a truck to maximize total units.
 * Each box type: [numberOfBoxes, unitsPerBox]
 * Truck capacity: truckSize boxes total.
 * Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
 * Output: 8  (take 1 box of type 0 + 2 of type 1 + 1 of type 2 = 3+4+1 = 8)
 */
function maximumUnits(boxTypes: number[][], truckSize: number): number {
  // sort by units per box descending — take most valuable first
  boxTypes.sort((a, b) => b[1] - a[1]);

  let totalUnits = 0;
  let remaining = truckSize;

  for (const [numBoxes, unitsPerBox] of boxTypes) {
    // take as many of this box type as we can
    const taken = Math.min(numBoxes, remaining);
    totalUnits += taken * unitsPerBox;
    remaining -= taken;

    if (remaining === 0) break; // truck is full
  }

  return totalUnits;
}
```

2. Non-overlapping Intervals ( sort by end time )

```typescript
/**
 * Find the minimum number of intervals to remove so none overlap.
 * Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
 * Output: 1  (remove [1,3])
 *
 * Key insight: sort by END time, not start time.
 * Greedily keep each interval that doesn't overlap the last kept one.
 */
function eraseOverlapIntervals(intervals: number[][]): number {
  // sort by end time
  intervals.sort((a, b) => a[1] - b[1]);

  let removed = 0;
  let lastEnd = -Infinity;

  for (const [start, end] of intervals) {
    if (start >= lastEnd) {
      // no overlap — keep this interval
      lastEnd = end;
    } else {
      // overlap — remove this interval (it ends later, so removing it is always at least as good)
      removed++;
    }
  }

  return removed;
}
```
