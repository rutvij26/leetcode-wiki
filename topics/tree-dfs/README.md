---
title: "Tree DFS"
parent: Topics
nav_order: 7
---

## ELI5 (Explain Like I'm 5)

Imagine exploring a cave system. DFS means you always pick one tunnel and go as deep as you possibly can before turning back. There are three ways to "take notes" as you explore: write things down before going deeper (pre-order), write things down in between going left and right (in-order), or wait until you've fully explored both sides before writing (post-order).

## Explanation

- Tree DFS visits every node by going deep before going wide
- Three DFS traversal orders — determined by **when you process the current node**:
  - **Pre-order** (node → left → right): process node first, then recurse. Good for copying a tree
  - **In-order** (left → node → right): recurse left, process, recurse right. On a BST this gives sorted order
  - **Post-order** (left → right → node): recurse both sides first, then process. Good for deletion, computing subtree values
- For BFS (level-order traversal) → see [Tree BFS](../tree-bfs/)

> Rule to memorize: BST + "sorted output" or "kth smallest/largest" → **in-order**

## When to use?

- Compute something about every node (sum, count, depth)
- Validate a tree's structure
- Copy or serialize a tree (pre-order)
- Get sorted values from a BST (in-order)
- Calculate subtree results before combining at parent (post-order)

## Approach

All three traversals share the same recursive structure — only the position of "process the node" changes:

```
pre-order:  process → recurse left → recurse right
in-order:   recurse left → process → recurse right
post-order: recurse left → recurse right → process
```

### Algorithm Steps (in-order):
1. Base case: if `node === null` → return
2. Recurse into `node.left`
3. Process `node` (add to result, compare, etc.)
4. Recurse into `node.right`

## Notes

- Time Complexity: O(n) — every node is visited exactly once
- Space Complexity: O(h) — call stack depth equals tree height (O(log n) balanced, O(n) worst case)
- For a **balanced BST**, height h = O(log n); for a skewed tree, h = O(n)
- Iterative DFS uses an explicit stack — useful when you need to avoid deep recursion

## Data structures

- **Recursive call stack** — implicit storage for DFS
- **Array** — collect results during traversal
- **Stack** — for the iterative version of DFS

## How to Master This

### Step-by-step
1. Read this page and understand that all three traversals are the same code with one line moved
2. Write all three from memory — they're nearly identical
3. Solve #94 (in-order) and #144 (pre-order) back to back to cement the pattern
4. Solve #230 (kth smallest in BST) — it's in-order traversal + a counter
5. Revisit [Recursion](../recursion/) if any of these feel shaky

### Key Resources
- 📹 [NeetCode — Binary Tree Inorder Traversal](https://www.youtube.com/watch?v=g_S5WuasWUE)
- 📹 [NeetCode — Kth Smallest Element in BST](https://www.youtube.com/watch?v=5LUXSvjmGCw)
- 📝 [NeetCode.io — Trees problems](https://neetcode.io/roadmap) (see "Trees" section)
- 🔁 Drill in this order: [#144](https://leetcode.com/problems/binary-tree-preorder-traversal) → [#94](https://leetcode.com/problems/binary-tree-inorder-traversal) → [#145](https://leetcode.com/problems/binary-tree-postorder-traversal) → [#230](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

## Sample LeetCode problems

- [#144 Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal)
- [#94 Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)
- [#145 Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal)
- [#230 Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

## Code Samples

1. All Three Traversals

```typescript
// Pre-order: node → left → right
function preorder(root: TreeNode | null, result: number[] = []): number[] {
  if (root === null) return result;
  result.push(root.val);       // process FIRST
  preorder(root.left, result);
  preorder(root.right, result);
  return result;
}

// In-order: left → node → right (gives sorted order on BST)
function inorder(root: TreeNode | null, result: number[] = []): number[] {
  if (root === null) return result;
  inorder(root.left, result);
  result.push(root.val);       // process IN THE MIDDLE
  inorder(root.right, result);
  return result;
}

// Post-order: left → right → node
function postorder(root: TreeNode | null, result: number[] = []): number[] {
  if (root === null) return result;
  postorder(root.left, result);
  postorder(root.right, result);
  result.push(root.val);       // process LAST
  return result;
}
```

2. Kth Smallest Element in a BST ( in-order + counter )

```typescript
/**
 * Return the kth smallest value in a BST
 * In-order traversal of a BST gives values in sorted order.
 * Just count until we hit the kth node.
 */
function kthSmallest(root: TreeNode | null, k: number): number {
  let count = 0;
  let result = 0;

  function inorder(node: TreeNode | null): void {
    if (node === null) return;

    inorder(node.left);

    // this is the "process" step — increment counter
    count++;
    if (count === k) {
      result = node.val;
      return; // found it, no need to continue
    }

    inorder(node.right);
  }

  inorder(root);
  return result;
}
```
