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

## Code Samples
