---
title: "Tree BFS"
---

## Explanation

- Algorithm for searching in Graph/Tree data structure
- Breadth = broad/wide ie goes horizontally before going vertically
- Mostly queue is used to store the visited nodes in a FIFO pattern

## When to use?

BFS is often used in problems where:

- _We need to find the shortest path in an **unweighted** graph or tree_.
- We want to process nodes level by level (e.g., printing nodes level-wise, finding the deepest node).
- The tree/graph is relatively wide, and you need to explore all possible nodes in the shortest time.
- You need to explore all nodes at a given depth level before moving to the next depth level.

## Approach

- Visited nodes are marked as Black and to be Visited nodes are marked as Gray
  - In Each iteration, all nodes at same level are taken into consideration
  - We pop one node out of queue, mark it as visited and add all of its children into the queue

### Algorithm Steps:

1. Start with the root node and add it to the queue.
2. While the queue is not empty:
   - Dequeue a node from the front of the queue.
   - Process the node.
   - Add its children to the queue.
3. Continue this process until the queue is empty

## Notes

- Time Complexity: O(|#Vertices| + |#Edges|)
- Space complexity is O(n) due to the storage of nodes in the queue.
- BFS is not suitable for trees or graphs with a large branching factor or when a deep search is needed, as it can use a lot of memory.

## Data structures

- **Queue**: A queue is essential for BFS because it ensures nodes are processed in the correct order, starting from the root and proceeding level by level.
- **Tree**: The BFS algorithm operates on trees and graphs. In a tree, each node has at most one parent, and potentially many children.

## Sample Leet code problems

1. [Leetcode 199](https://leetcode.com/problems/binary-tree-right-side-view/description/)
2. [Leetcode 1161](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree)

## ELI5 (Explain Like I'm 5)

Imagine you're searching for your friend in a building. BFS means you check every room on floor 1 first, then every room on floor 2, then floor 3 — you never go deeper before finishing the current floor. This guarantees you find your friend in the fewest floors possible.

## How to Master This

### Step-by-step
1. Read this page and understand the sentinel-null technique used here (or use a level-size counter as an alternative)
2. Watch the NeetCode video on Binary Tree Level Order Traversal
3. Solve #199 (right side view) — one value per level
4. Solve #1161 (max level sum) — aggregate per level
5. Understand when to use BFS vs DFS: if the problem mentions "level", "shortest path", or "closest" → BFS

### Key Resources
- 📹 [NeetCode — Binary Tree Level Order Traversal](https://www.youtube.com/watch?v=6ZnyEApgFYg)
- 📹 [NeetCode — Binary Tree Right Side View](https://www.youtube.com/watch?v=d4zLyf32e3I)
- 📝 [NeetCode.io — Trees problems](https://neetcode.io/roadmap) (see "Trees" section)
- 🔁 Drill in this order: [Leetcode 199](https://leetcode.com/problems/binary-tree-right-side-view/description/) → [Leetcode 1161](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree) → [#102 Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)

## Code Samples

```typescript
/**
 * Function that returns all the right side nodes for Leetcode 199
 */
function rightSideView(root: TreeNode | null): number[] {
  // checking if root node is present
  if (!root) return [];

  // null is our sentinal node, which marks as end of a level
  const queue = [root, null];

  // array to store right node values
  const rightSide = [];

  // initializing curr as root
  let curr = root;

  // untill queue has something
  while (queue.length > 0) {
    let prev: TreeNode | null = curr;

    // assigns the first element of queue
    curr = queue.shift()!;

    // untill curr is not null
    while (curr) {
      // add child nodes in the queue
      if (curr.left) queue.push(curr.left);
      if (curr.right) queue.push(curr.right);

      // set the prev as curr
      prev = curr;
      // set the next element in queue as curr
      curr = queue.shift()!;
    }

    // when the while loop exits cause of sentinal node 'null', we push the previous value to return array
    rightSide.push(prev!.val);

    // if queue is empty push null to stop the while loop
    if (queue.length > 0) {
      queue.push(null);
    }
  }
  // return the right side elements
  return rightSide;
}
```

```typescript
/**
 * Function that finds the max Level Sum Leetcode 1161
 * */
function maxLevelSum(root: TreeNode | null): number {
  const queue = [root, null];
  const sumArr = [];

  let curr = root;

  while (queue.length > 0) {
    let sum = 0;
    curr = queue.shift();

    while (curr) {
      if (curr.left) queue.push(curr.left);
      if (curr.right) queue.push(curr.right);
      sum += curr.val;
      curr = queue.shift();
    }

    sumArr.push(sum);
    sum = 0;

    if (queue.length > 0) {
      queue.push(null);
    }
  }

  const sortedNumbers = [...sumArr].sort((a, b) => a - b);
  const maxIndex = sumArr.indexOf(sortedNumbers[sortedNumbers.length - 1]);
  return maxIndex + 1;
}
```
