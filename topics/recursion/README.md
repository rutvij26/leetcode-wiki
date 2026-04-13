---
title: "Recursion"
---

## ELI5 (Explain Like I'm 5)

Imagine you want to count how many people are in a long line. You ask the person in front of you "how many people are behind you?" — and they ask the person behind them the same question. This keeps going until the last person says "just me, so 1!" Then the answers come back: 1, 2, 3... until you get the total. You never counted the whole line yourself — you trusted each person to handle their bit.

## Explanation

- Recursion solves a problem by breaking it into a **smaller version of the same problem**
- Two parts every recursive function must have:
  - **Base case** — the smallest input with an obvious answer (stops the recursion)
  - **Recursive case** — assume the smaller problem is already solved, combine the results
- The key mindset: **you don't trace the call stack** — you trust that the recursive call returns the correct answer for a smaller input

> Ask yourself: _"If I already had the answer for `n-1`, how would I get the answer for `n`?"_

## When to use?

- Problems involving trees or graphs (DFS naturally uses recursion)
- Divide-and-conquer problems (merge sort, binary search)
- Generating all combinations or permutations
- When the problem definition is self-referential (e.g. "a tree is a node with two subtrees")

## Approach

1. **Define what the function does** — write it in one sentence before coding
2. **Identify the base case** — what's the smallest input? What's the obvious answer?
3. **Write the recursive case** — assume `f(smaller)` works, use it to compute `f(current)`
4. **Trust the recursion** — do not trace every call; verify base case + recursive step are correct

### Algorithm Steps (max depth of a tree):
1. Base case: if `node === null` → return `0`
2. Recursive case: `1 + max(maxDepth(node.left), maxDepth(node.right))`
3. The `1 +` accounts for the current node; the recursion handles all subtrees

## Notes

- Time Complexity: usually O(n) for tree problems (visit each node once)
- Space Complexity: O(h) where h is the call stack depth (h = tree height, O(log n) balanced, O(n) skewed)
- Recursion uses the call stack — very deep recursion can cause a stack overflow
- For trees: recursion is almost always the cleanest solution — embrace it

## Data structures

- **Call stack** — implicit; each recursive call adds a frame
- **Tree / linked structure** — the most natural fit for recursion

## How to Master This

### Step-by-step
1. Read this page — focus on the two-part mental model (base case + trust)
2. Watch the video below — seeing it explained on trees makes it click
3. Solve #104 Max Depth of Binary Tree (2–3 lines once you get it)
4. Solve #226 Invert Binary Tree — same structure, different operation
5. Solve #509 Fibonacci — understand why naive recursion is O(2ⁿ) and how memoization fixes it

### Key Resources
- 📹 [NeetCode — Max Depth of Binary Tree](https://www.youtube.com/watch?v=hTM3phVI6YQ)
- 📹 [CS Dojo — Introduction to Recursion](https://www.youtube.com/watch?v=B0NtAFf4bvU)
- 📝 [NeetCode.io — Trees problems](https://neetcode.io/roadmap) (see "Trees" section, all use recursion)
- 🔁 Drill in this order: [#509](https://leetcode.com/problems/fibonacci-number) → [#104](https://leetcode.com/problems/maximum-depth-of-binary-tree) → [#226](https://leetcode.com/problems/invert-binary-tree) → [#344](https://leetcode.com/problems/reverse-string)

## Sample LeetCode problems

- [#509 Fibonacci Number](https://leetcode.com/problems/fibonacci-number)
- [#104 Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)
- [#226 Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree)
- [#344 Reverse String](https://leetcode.com/problems/reverse-string)

## Code Samples

1. Fibonacci ( base case + recursive case )

```typescript
/**
 * Return the nth Fibonacci number
 * fib(0)=0, fib(1)=1, fib(2)=1, fib(3)=2, fib(4)=3...
 */
function fib(n: number): number {
  // base cases: we know these answers directly
  if (n <= 1) return n;

  // recursive case: trust that fib(n-1) and fib(n-2) are correct
  return fib(n - 1) + fib(n - 2);
}
// Note: this is O(2^n) — see memoized version below for O(n)
```

2. Maximum Depth of Binary Tree ( tree recursion )

```typescript
/**
 * Return the maximum depth (number of levels) of a binary tree
 * Tree:   3
 *        / \
 *       9  20
 *          / \
 *         15   7
 * Output: 3
 */
function maxDepth(root: TreeNode | null): number {
  // base case: empty tree has depth 0
  if (root === null) return 0;

  // recursive case: depth = 1 (current node) + deepest subtree
  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);

  return 1 + Math.max(leftDepth, rightDepth);
}
```

3. Invert Binary Tree ( same structure, different operation )

```typescript
/**
 * Flip a binary tree (mirror it)
 * Input:  4       Output:  4
 *        / \              / \
 *       2   7            7   2
 *      / \ / \          / \ / \
 *     1  3 6  9        9  6 3  1
 */
function invertTree(root: TreeNode | null): TreeNode | null {
  // base case: nothing to invert
  if (root === null) return null;

  // swap left and right children
  [root.left, root.right] = [root.right, root.left];

  // recursively invert both subtrees
  invertTree(root.left);
  invertTree(root.right);

  return root;
}
```
