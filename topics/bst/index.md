---
title: "BST"
parent: Topics
nav_order: 9
---

## ELI5 (Explain Like I'm 5)

Think of a BST like a filing cabinet where there's a rule for every single drawer: everything smaller goes in the left drawer, everything bigger goes in the right drawer. And every drawer inside also follows the same rule. So to find something, you just keep asking "is it smaller or bigger?" and you'll find it super fast.

## Explanation

- **Binary Search Tree (BST)**: for every node, all values in the left subtree are smaller, all values in the right subtree are larger — recursively
- This property enables O(log n) search, insert, and delete on a balanced tree
- **Common trap**: validating a BST requires passing down `min` and `max` bounds — just checking that `node.left < node` isn't enough

> Example of the trap: `[5, 4, 6, null, null, 3, 7]` — `3` is in the right subtree of `5`, which violates the BST property even though `3 < 6`.

## When to use?

- Efficiently search for a value: O(log n) vs O(n) in a plain array
- Check whether a tree satisfies BST properties
- Insert a value while maintaining sorted order
- Find kth smallest/largest (→ combine with [in-order traversal](../tree-dfs/))

## Approach

1. **Search** — at each node: if `target < node.val` go left, if `target > node.val` go right, if equal → found
2. **Insert** — same as search; when you hit `null`, that's where the new node goes
3. **Validate** — pass `(min, max)` bounds down; each node must satisfy `min < node.val < max`. Initial call: `validate(root, -Infinity, +Infinity)`

### Algorithm Steps (validate BST):
1. Base case: `node === null` → return `true`
2. Check: `node.val <= min || node.val >= max` → return `false`
3. Recurse left with updated max: `validate(node.left, min, node.val)`
4. Recurse right with updated min: `validate(node.right, node.val, max)`
5. Return `left && right`

## Notes

- Time Complexity: O(log n) average for balanced BST, O(n) worst case (skewed tree)
- Space Complexity: O(h) — call stack depth equals tree height
- A balanced BST has height O(log n); a skewed BST degrades to a linked list at O(n)
- In-order traversal of a BST always produces a sorted sequence — use this as a sanity check

## Data structures

- **TreeNode** — `val`, `left`, `right`
- **Recursion** — the natural way to traverse with bounds

## How to Master This

### Step-by-step
1. Read this page — especially the validation trap (bounds vs simple comparison)
2. Draw a small invalid BST that looks valid at first glance (like the example above)
3. Solve #700 (search) — it's the simplest BST operation
4. Solve #98 (validate) — practice passing bounds down
5. Solve #701 (insert) — same idea as search, just place the node when you hit null

### Key Resources
- 📹 [NeetCode — Validate Binary Search Tree](https://www.youtube.com/watch?v=s6ATEkipzow)
- 📹 [NeetCode — Kth Smallest in BST](https://www.youtube.com/watch?v=5LUXSvjmGCw)
- 📝 [NeetCode.io — Trees problems](https://neetcode.io/roadmap) (see "Trees" section)
- 🔁 Drill in this order: [#700](https://leetcode.com/problems/search-in-a-binary-search-tree) → [#701](https://leetcode.com/problems/insert-into-a-binary-search-tree) → [#98](https://leetcode.com/problems/validate-binary-search-tree) → [#235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)

## Sample LeetCode problems

- [#700 Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree)
- [#701 Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree)
- [#98 Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)
- [#235 Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)

## Code Samples

1. Search in a BST

```typescript
/**
 * Find the node with the given value in the BST
 * Input: root = [4,2,7,1,3], val = 2
 * Output: subtree rooted at 2
 */
function searchBST(root: TreeNode | null, val: number): TreeNode | null {
  // base case: not found or exact match
  if (root === null || root.val === val) return root;

  // BST property: go left if smaller, right if larger
  if (val < root.val) {
    return searchBST(root.left, val);
  } else {
    return searchBST(root.right, val);
  }
}
```

2. Validate Binary Search Tree ( bounds approach )

```typescript
/**
 * Check if a binary tree is a valid BST
 * Input: [2,1,3] → true
 * Input: [5,1,4,null,null,3,6] → false (4 is in right subtree of 5 but 4 < 5)
 */
function isValidBST(root: TreeNode | null): boolean {
  function validate(
    node: TreeNode | null,
    min: number,
    max: number
  ): boolean {
    // empty tree is valid
    if (node === null) return true;

    // node must be strictly inside (min, max) bounds
    if (node.val <= min || node.val >= max) return false;

    // left subtree: all values must be < node.val (update max)
    // right subtree: all values must be > node.val (update min)
    return (
      validate(node.left, min, node.val) &&
      validate(node.right, node.val, max)
    );
  }

  return validate(root, -Infinity, Infinity);
}
```
