---
title: "AVL Trees"
parent: Topics
nav_order: 25
---

## ELI5 (Explain Like I'm 5)

A regular BST can get lopsided — if you insert 1, 2, 3, 4, 5 in order, it becomes a straight line and search takes O(n). An AVL tree is a BST that checks its balance after every insert or delete, and if one side gets too heavy, it rotates to fix itself. It's a self-balancing BST that guarantees O(log n) always.

## Explanation

- AVL trees are **height-balanced BSTs** — at every node, the heights of left and right subtrees differ by at most 1
- **Balance factor** = `height(left) - height(right)` ∈ {-1, 0, 1}
- When a rotation is needed (balance factor becomes ±2), one of four rotation cases applies
- Four rotation types: Left, Right, Left-Right, Right-Left

> AVL trees rarely appear as direct LeetCode problems, but the concepts (rotations, balance factors) appear in system design and "design a data structure" questions.

## When to use?

- When you need guaranteed O(log n) insert/delete/search (BSTs can degrade to O(n))
- Implementing an ordered set or map from scratch
- Understanding how `std::map` (C++), `TreeMap` (Java) work internally
- Interview questions about self-balancing BSTs

## Approach

### Balance Factor
```
height(node) = 1 + max(height(left), height(right))
balance(node) = height(left) - height(right)

balanced: -1 <= balance <= 1
left-heavy: balance > 1  → need right rotation (or left-right)
right-heavy: balance < -1 → need left rotation (or right-left)
```

### Four Rotation Cases

| Case | Condition | Fix |
|------|-----------|-----|
| Left-Left | balance > 1, left child balanced or left-heavy | Right rotate |
| Right-Right | balance < -1, right child balanced or right-heavy | Left rotate |
| Left-Right | balance > 1, left child right-heavy | Left rotate left child, then right rotate |
| Right-Left | balance < -1, right child left-heavy | Right rotate right child, then left rotate |

### Right Rotation (Left-Left case)
```
    z                y
   / \              / \
  y   T4    →     x    z
 / \             / \  / \
x   T3          T1 T2 T3 T4
```

### Left Rotation (Right-Right case)
```
  z                  y
 / \                / \
T1   y      →      z    x
    / \            / \  / \
   T2   x         T1 T2 T3 T4
```

## Notes

- Time Complexity: O(log n) for insert, delete, search — guaranteed
- Space Complexity: O(n) for n nodes
- Python does not have a built-in AVL tree — use `SortedList` from `sortedcontainers` library in practice
- Red-Black trees are preferred in most language standard libraries (slightly fewer rotations on average)
- For interviews, knowing the concept and rotation cases is usually enough — full implementation is rarely asked

## Data structures

- **AVL Node** — stores value, left/right children, and height
- **Height field** — must be updated after every insert/delete/rotation

## How to Master This

### Step-by-step
1. Master BST insert/delete/search first
2. Understand why BSTs can degrade and why balancing helps
3. Learn the 4 rotation cases — draw them on paper
4. Implement a full AVL tree insert with rotations
5. Use `sortedcontainers.SortedList` in Python interviews where you'd need an AVL tree

### Key Resources
- 📹 [Abdul Bari — AVL Tree](https://www.youtube.com/watch?v=jDM6_TnYIqE)
- 📹 [MIT 6.006 — AVL Trees](https://www.youtube.com/watch?v=FNeL18KsWPc)
- 📝 [Visualgo — AVL Tree Visualization](https://visualgo.net/en/bst)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 1382 | [Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree) | Medium | Low | 📖 |
| 699 | [Falling Squares](https://leetcode.com/problems/falling-squares) | Hard | Low | 📖 |
| 315 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self) | Hard | Medium | ⚡ |

## Code Samples

1. AVL Tree — full implementation

```python
class AVLNode:
    def __init__(self, val: int):
        self.val = val
        self.left: 'AVLNode | None' = None
        self.right: 'AVLNode | None' = None
        self.height = 1


class AVLTree:
    def _height(self, node: AVLNode | None) -> int:
        return node.height if node else 0

    def _balance_factor(self, node: AVLNode | None) -> int:
        return self._height(node.left) - self._height(node.right) if node else 0

    def _update_height(self, node: AVLNode) -> None:
        node.height = 1 + max(self._height(node.left), self._height(node.right))

    def _rotate_right(self, z: AVLNode) -> AVLNode:
        y = z.left
        T3 = y.right
        y.right = z
        z.left = T3
        self._update_height(z)
        self._update_height(y)
        return y  # new root

    def _rotate_left(self, z: AVLNode) -> AVLNode:
        y = z.right
        T2 = y.left
        y.left = z
        z.right = T2
        self._update_height(z)
        self._update_height(y)
        return y  # new root

    def _rebalance(self, node: AVLNode) -> AVLNode:
        self._update_height(node)
        bf = self._balance_factor(node)

        # Left-Left
        if bf > 1 and self._balance_factor(node.left) >= 0:
            return self._rotate_right(node)

        # Left-Right
        if bf > 1 and self._balance_factor(node.left) < 0:
            node.left = self._rotate_left(node.left)
            return self._rotate_right(node)

        # Right-Right
        if bf < -1 and self._balance_factor(node.right) <= 0:
            return self._rotate_left(node)

        # Right-Left
        if bf < -1 and self._balance_factor(node.right) > 0:
            node.right = self._rotate_right(node.right)
            return self._rotate_left(node)

        return node  # already balanced

    def insert(self, node: AVLNode | None, val: int) -> AVLNode:
        if not node:
            return AVLNode(val)
        if val < node.val:
            node.left = self.insert(node.left, val)
        elif val > node.val:
            node.right = self.insert(node.right, val)
        else:
            return node  # duplicate — no insert

        return self._rebalance(node)
```
