---
title: "Dynamic Programming"
parent: Topics
nav_order: 18
---

## ELI5 (Explain Like I'm 5)

You're climbing stairs and want to know how many ways you can reach step 10. Instead of figuring out all paths from scratch, you realize: "the number of ways to reach step 10 = ways to reach step 9 + ways to reach step 8." You already solved those smaller problems, so you look up the answers instead of recomputing them. That's dynamic programming — solve small subproblems, save the answers, build up to the big answer.

## Explanation

- Dynamic programming (DP) solves problems by breaking them into **overlapping subproblems**, solving each once, and storing the result
- Two properties that tell you DP applies:
  1. **Optimal substructure** — the optimal solution contains optimal solutions to subproblems
  2. **Overlapping subproblems** — the same subproblems are solved repeatedly (unlike divide-and-conquer)
- Two implementation styles:
  - **Top-down (memoization):** recursive, cache results in a map/array
  - **Bottom-up (tabulation):** iterative, fill a DP table from base cases up

> Keyword trigger: _"number of ways"_, _"minimum/maximum cost/path"_, _"can you reach X"_, _"longest subsequence"_ → think DP.

## When to use?

- Counting problems ("how many ways to…")
- Optimization problems ("minimum cost to…", "maximum profit from…")
- Feasibility problems ("is it possible to…")
- String problems involving subsequences or substrings
- The naive recursive solution has exponential time due to repeated subproblems

## Approach

### Identifying the DP state
Ask: "what information do I need to uniquely describe a subproblem?"
- 1D: `dp[i]` = answer for the first `i` elements
- 2D: `dp[i][j]` = answer considering first `i` items with capacity/budget `j`

### Recurrence relation
Express `dp[i]` in terms of smaller states. This is the heart of every DP solution.

### Base cases
Define `dp[0]` (and `dp[1]` if needed) directly — these are the seeds of the computation.

### Algorithm Steps (bottom-up)
1. Define what `dp[i]` means in plain English
2. Write the recurrence relation
3. Initialize base cases
4. Fill the table in the right order (smaller → larger)
5. Return `dp[n]` (or wherever the final answer lives)

## Notes

- Time Complexity: typically O(n), O(n²), or O(n × capacity) — depends on state dimensions
- Space Complexity: O(n) or O(n²) for the table; often reducible to O(1) or O(n) with rolling array
- **Don't jump to code** — write the recurrence in English first
- If you can only move forward through states (no going back), bottom-up is usually cleaner
- Space optimization: if `dp[i]` only depends on `dp[i-1]`, you only need two variables

## Data structures

- **1D array** — single-variable DP (Fibonacci, house robber, climbing stairs)
- **2D array** — two-variable DP (knapsack, edit distance, LCS)
- **HashMap** — top-down memoization when state isn't naturally integer-indexed

## How to Master This

### Step-by-step
1. Start with 1D DP — it's just Fibonacci with a different story
2. Solve #70 (climbing stairs) — the simplest possible DP
3. Solve #198 (house robber) — introduces the "skip or take" decision
4. Solve #322 (coin change) — unbounded knapsack, classic recurrence
5. Solve #300 (LIS) — O(n²) DP on pairs of indices
6. Solve #1143 (LCS) — 2D DP, the foundation for edit distance
7. Solve #416 (partition equal subset sum) — DP on booleans, knapsack variant

### Key Resources
- 📹 [NeetCode — Dynamic Programming playlist](https://www.youtube.com/playlist?list=PLot-Xpze53leU0Ec48pgtylmgwOrczKNC)
- 📹 [NeetCode — Climbing Stairs](https://www.youtube.com/watch?v=Y0lT9Fck7qI)
- 📹 [NeetCode — Coin Change](https://www.youtube.com/watch?v=H9bfqozjoqs)
- 📝 [NeetCode.io — DP problems](https://neetcode.io/roadmap) (see "1D DP" and "2D DP" sections)
- 🔁 Drill in this order: [#70](https://leetcode.com/problems/climbing-stairs) → [#198](https://leetcode.com/problems/house-robber) → [#322](https://leetcode.com/problems/coin-change) → [#300](https://leetcode.com/problems/longest-increasing-subsequence) → [#1143](https://leetcode.com/problems/longest-common-subsequence)

## Sample LeetCode problems

- [#70 Climbing Stairs](https://leetcode.com/problems/climbing-stairs)
- [#198 House Robber](https://leetcode.com/problems/house-robber)
- [#322 Coin Change](https://leetcode.com/problems/coin-change)
- [#300 Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence)
- [#1143 Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence)

## Code Samples

1. Climbing Stairs — 1D DP (Fibonacci variant)

```typescript
/**
 * Count the number of distinct ways to climb n stairs (1 or 2 steps at a time).
 * Input: n = 4
 * Output: 5  (1+1+1+1, 1+1+2, 1+2+1, 2+1+1, 2+2)
 *
 * dp[i] = ways to reach stair i
 * dp[i] = dp[i-1] + dp[i-2]   (came from one step below OR two steps below)
 */
function climbStairs(n: number): number {
  if (n <= 2) return n;

  let prev2 = 1; // dp[1]
  let prev1 = 2; // dp[2]

  for (let i = 3; i <= n; i++) {
    const curr = prev1 + prev2;
    prev2 = prev1;
    prev1 = curr;
  }

  return prev1;
}
```

2. House Robber — skip or take decision

```typescript
/**
 * Max money you can rob from houses in a row without robbing two adjacent houses.
 * Input: nums = [2,7,9,3,1]
 * Output: 12  (rob houses 0, 2, 4 → 2+9+1=12)
 *
 * dp[i] = max money robbing up to house i
 * dp[i] = max(dp[i-1], dp[i-2] + nums[i])
 *           skip house i   OR   rob house i (can't rob i-1)
 */
function rob(nums: number[]): number {
  if (nums.length === 1) return nums[0];

  let prev2 = nums[0];
  let prev1 = Math.max(nums[0], nums[1]);

  for (let i = 2; i < nums.length; i++) {
    const curr = Math.max(prev1, prev2 + nums[i]);
    prev2 = prev1;
    prev1 = curr;
  }

  return prev1;
}
```

3. Coin Change — unbounded knapsack (minimize)

```typescript
/**
 * Find the fewest coins needed to make up the given amount.
 * Return -1 if it's not possible.
 * Input: coins = [1,5,11], amount = 15
 * Output: 3  (5+5+5)
 *
 * dp[i] = min coins to make amount i
 * dp[i] = min over all coins c where c <= i: dp[i - c] + 1
 */
function coinChange(coins: number[], amount: number): number {
  // initialize to amount+1 (impossible sentinel — larger than any real answer)
  const dp = new Array(amount + 1).fill(amount + 1);
  dp[0] = 0; // base case: 0 coins needed for amount 0

  for (let i = 1; i <= amount; i++) {
    for (const coin of coins) {
      if (coin <= i) {
        dp[i] = Math.min(dp[i], dp[i - coin] + 1);
      }
    }
  }

  return dp[amount] > amount ? -1 : dp[amount];
}
```

4. Longest Common Subsequence — 2D DP

```typescript
/**
 * Length of the longest common subsequence of two strings.
 * Input: text1 = "abcde", text2 = "ace"
 * Output: 3  ("ace")
 *
 * dp[i][j] = LCS length of text1[0..i-1] and text2[0..j-1]
 * If text1[i-1] == text2[j-1]: dp[i][j] = dp[i-1][j-1] + 1
 * Else:                         dp[i][j] = max(dp[i-1][j], dp[i][j-1])
 */
function longestCommonSubsequence(text1: string, text2: string): number {
  const m = text1.length;
  const n = text2.length;

  // dp[i][j]: LCS of first i chars of text1 and first j chars of text2
  const dp: number[][] = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (text1[i - 1] === text2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1; // characters match — extend LCS
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]); // skip one character from either string
      }
    }
  }

  return dp[m][n];
}
```
