---
title: "Backtracking"
parent: Topics
nav_order: 15
---

## ELI5 (Explain Like I'm 5)

Imagine you're exploring a maze. At each fork you pick a direction, walk forward, and if you hit a dead end you go back to the last fork and try the other direction. Backtracking is exactly this — you build a solution one piece at a time, and whenever you realize a partial solution can't possibly work, you "backtrack" and undo the last choice to try the next option.

## Explanation

- Backtracking is a **systematic search** over a decision tree: at each step you make a choice, recurse into it, then undo the choice (backtrack) before trying the next one
- It's a DFS on the space of all possible solutions
- The key optimization is **pruning** — detecting early that a partial solution is invalid, so you skip entire subtrees
- Without pruning, it's just brute-force recursion; with pruning, it's often the only feasible approach

> Keyword trigger: _"all combinations/permutations/subsets"_, _"find all valid arrangements"_, _"place N queens"_ → backtracking.

## When to use?

- You need **all** valid solutions (not just one), or need to find if any valid solution exists
- The problem involves choosing from a set and the choices have constraints
- The solution can be built incrementally, with invalid partial solutions prunable early
- Combinations, permutations, subsets, path-finding with constraints

## Approach

The backtracking template is always the same shape:

```
function backtrack(path, choices):
    if path is a complete solution:
        results.push(copy of path)
        return

    for each choice in choices:
        if choice is valid:
            path.push(choice)          // make choice
            backtrack(path, updated choices)
            path.pop()                 // undo choice (backtrack)
```

Three decisions to nail down before coding:
1. **What is "path"?** — the partial solution being built
2. **What are "choices"?** — what can be added at this step
3. **What makes a choice invalid?** — your pruning condition

### For subsets / combinations (no reuse, no permutation order):
- Pass a `start` index — only consider elements from `start` onward to avoid duplicates

### For permutations (order matters):
- Use a `used` set/array — at each position, try all unused elements

## Notes

- Time Complexity: O(2ⁿ) for subsets, O(n!) for permutations — exponential, but pruning helps enormously in practice
- Space Complexity: O(n) for the recursion stack + path
- **Always pass a copy** when appending to results: `results.push([...path])` not `results.push(path)`
- The undo step (pop) must exactly mirror the make step (push)
- Pruning is what separates a working solution from a TLE

## Data structures

- **Array (path)** — the current partial solution, built and unwound as you recurse
- **Set or boolean array (used)** — track which elements are currently in the path (for permutations)
- **Start index** — avoid re-using earlier elements (for combinations/subsets)

## How to Master This

### Step-by-step
1. Read this page and internalize the template (make → recurse → undo)
2. Solve #78 (subsets) — no constraints, pure structure
3. Solve #77 (combinations) — adds the `k` size constraint
4. Solve #46 (permutations) — switches to `used` array tracking
5. Solve #39 (combination sum) — allows reuse, requires a different pruning rule
6. Solve #51 (N-Queens) — hardest; multiple constraints per step

### Key Resources
- 📹 [NeetCode — Backtracking playlist](https://www.youtube.com/playlist?list=PLot-Xpze53leU0Ec48pgtylmgwOrczKNC)
- 📹 [NeetCode — Subsets](https://www.youtube.com/watch?v=REOH22Xwdkk)
- 📹 [NeetCode — Combination Sum](https://www.youtube.com/watch?v=GBKI9VSKdGg)
- 📝 [NeetCode.io — Backtracking problems](https://neetcode.io/roadmap) (see "Backtracking" section)
- 🔁 Drill in this order: [#78](https://leetcode.com/problems/subsets) → [#77](https://leetcode.com/problems/combinations) → [#46](https://leetcode.com/problems/permutations) → [#39](https://leetcode.com/problems/combination-sum) → [#51](https://leetcode.com/problems/n-queens)

## Sample LeetCode problems

- [#78 Subsets](https://leetcode.com/problems/subsets)
- [#77 Combinations](https://leetcode.com/problems/combinations)
- [#46 Permutations](https://leetcode.com/problems/permutations)
- [#39 Combination Sum](https://leetcode.com/problems/combination-sum)
- [#51 N-Queens](https://leetcode.com/problems/n-queens)

## Code Samples

1. Subsets — pure backtracking template

```typescript
/**
 * Return all possible subsets of a distinct integer array.
 * Input: nums = [1,2,3]
 * Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
 */
function subsets(nums: number[]): number[][] {
  const results: number[][] = [];
  const path: number[] = [];

  function backtrack(start: number): void {
    // every path state is a valid subset — add a copy
    results.push([...path]);

    for (let i = start; i < nums.length; i++) {
      path.push(nums[i]);       // make choice
      backtrack(i + 1);         // recurse (i+1 prevents reuse of same element)
      path.pop();               // undo choice
    }
  }

  backtrack(0);
  return results;
}
```

2. Permutations — track used elements

```typescript
/**
 * Return all permutations of a distinct integer array.
 * Input: nums = [1,2,3]
 * Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
 */
function permute(nums: number[]): number[][] {
  const results: number[][] = [];
  const path: number[] = [];
  const used = new Array(nums.length).fill(false);

  function backtrack(): void {
    if (path.length === nums.length) {
      results.push([...path]); // complete permutation — save a copy
      return;
    }

    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue;   // skip elements already in path

      used[i] = true;
      path.push(nums[i]);      // make choice
      backtrack();
      path.pop();              // undo choice
      used[i] = false;
    }
  }

  backtrack();
  return results;
}
```

3. Combination Sum — allow reuse, prune when sum exceeds target

```typescript
/**
 * Find all combinations of candidates that sum to target.
 * Candidates may be reused unlimited times.
 * Input: candidates = [2,3,6,7], target = 7
 * Output: [[2,2,3],[7]]
 */
function combinationSum(candidates: number[], target: number): number[][] {
  const results: number[][] = [];
  const path: number[] = [];

  function backtrack(start: number, remaining: number): void {
    if (remaining === 0) {
      results.push([...path]); // exact match — save solution
      return;
    }

    for (let i = start; i < candidates.length; i++) {
      if (candidates[i] > remaining) continue; // pruning: can't exceed target

      path.push(candidates[i]);                // make choice
      backtrack(i, remaining - candidates[i]); // i (not i+1) allows reuse
      path.pop();                              // undo choice
    }
  }

  backtrack(0, target);
  return results;
}
```
