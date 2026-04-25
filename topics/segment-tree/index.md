---
title: "Segment Tree"
parent: Topics
nav_order: 26
---

## ELI5 (Explain Like I'm 5)

You have a row of 1000 numbers and someone keeps asking "what's the sum from index 3 to index 500?" and also keeps changing values. Recalculating from scratch each time is slow. A segment tree precomputes partial sums in a tree structure — each node covers a range. Answering a range query takes O(log n) instead of O(n).

## Explanation

- A segment tree is a **binary tree over an array** where each node stores an aggregate (sum, min, max) for a contiguous range
- Leaf nodes represent individual elements; internal nodes represent merged ranges
- Supports two operations in O(log n): **range query** and **point update**
- Built in O(n), stores in an array of size 4n (array-based representation)

> Keyword trigger: _"range sum query with updates"_, _"range min/max with updates"_, _"count inversions"_ → Segment Tree (or Fenwick Tree for simpler cases).

## When to use?

- Range queries (sum, min, max) with **point updates** in between
- "How many elements in range [l, r] satisfy condition X?"
- When you need O(log n) updates AND O(log n) queries simultaneously
- If only queries (no updates), prefix sums work (O(1) query); if only updates, just update the array

## Approach

### Array-based Segment Tree
```
For an array of size n, the segment tree has at most 4n nodes.
Node i:
  - left child:  2i
  - right child: 2i + 1
  - parent:      i // 2

Build: fill from leaves up
Query: split range into O(log n) tree nodes, merge their values
Update: update leaf, propagate changes up
```

### Build
```
def build(arr, node, start, end):
    if start == end:
        tree[node] = arr[start]  # leaf
    else:
        mid = (start + end) // 2
        build(arr, 2*node, start, mid)
        build(arr, 2*node+1, mid+1, end)
        tree[node] = tree[2*node] + tree[2*node+1]  # merge (sum example)
```

### Query
```
def query(node, start, end, l, r):
    if r < start or end < l:
        return 0        # out of range
    if l <= start and end <= r:
        return tree[node]   # completely inside range
    mid = (start + end) // 2
    return query(2*node, start, mid, l, r) + query(2*node+1, mid+1, end, l, r)
```

## Notes

- Time Complexity: O(n) build, O(log n) query, O(log n) update
- Space Complexity: O(n) — typically allocate `4 * n` to be safe
- For **lazy propagation** (range updates, not just point updates), you need to store lazy tags and push them down
- Python is slow for large segment trees — consider using a different approach (Fenwick Tree) for simpler range-sum problems

## Data structures

- **Array of size 4n** — the segment tree itself (1-indexed, root at index 1)
- **Lazy array** — optional, for range updates with lazy propagation

## How to Master This

### Step-by-step
1. Implement a range sum segment tree (build, query, update) from scratch
2. Solve #307 (range sum query mutable) — the canonical segment tree problem
3. Solve #315 (count of smaller numbers after self) — segment tree on coordinate-compressed values
4. Learn lazy propagation for range update queries

### Key Resources
- 📹 [William Fiset — Segment Tree](https://www.youtube.com/watch?v=ZBHKZF5w4YU)
- 📹 [Errichto — Segment Trees](https://www.youtube.com/watch?v=CNcHkQfnUV0)
- 📝 [CP-Algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
- 🔁 Drill: [#307](https://leetcode.com/problems/range-sum-query-mutable) → [#315](https://leetcode.com/problems/count-of-smaller-numbers-after-self) → [#493](https://leetcode.com/problems/reverse-pairs)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 307 | [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable) | Medium | High | ✅ |
| 315 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self) | Hard | Medium | ⚡ |
| 493 | [Reverse Pairs](https://leetcode.com/problems/reverse-pairs) | Hard | Low | 📖 |
| 218 | [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem) | Hard | Medium | ⚡ |

## Code Samples

1. Range Sum Segment Tree — build, query, update

```python
class SegmentTree:
    def __init__(self, nums: list[int]):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)
        self._build(nums, 1, 0, self.n - 1)

    def _build(self, nums: list[int], node: int, start: int, end: int) -> None:
        if start == end:
            self.tree[node] = nums[start]
            return
        mid = (start + end) // 2
        self._build(nums, 2 * node, start, mid)
        self._build(nums, 2 * node + 1, mid + 1, end)
        self.tree[node] = self.tree[2 * node] + self.tree[2 * node + 1]

    def update(self, idx: int, val: int, node: int = 1, start: int = 0, end: int = -1) -> None:
        if end == -1:
            end = self.n - 1
        if start == end:
            self.tree[node] = val
            return
        mid = (start + end) // 2
        if idx <= mid:
            self.update(idx, val, 2 * node, start, mid)
        else:
            self.update(idx, val, 2 * node + 1, mid + 1, end)
        self.tree[node] = self.tree[2 * node] + self.tree[2 * node + 1]

    def query(self, l: int, r: int, node: int = 1, start: int = 0, end: int = -1) -> int:
        if end == -1:
            end = self.n - 1
        if r < start or end < l:
            return 0        # out of range
        if l <= start and end <= r:
            return self.tree[node]  # fully inside range
        mid = (start + end) // 2
        left_sum = self.query(l, r, 2 * node, start, mid)
        right_sum = self.query(l, r, 2 * node + 1, mid + 1, end)
        return left_sum + right_sum


# Usage (LeetCode #307)
class NumArray:
    def __init__(self, nums: list[int]):
        self.st = SegmentTree(nums)

    def update(self, index: int, val: int) -> None:
        self.st.update(index, val)

    def sumRange(self, left: int, right: int) -> int:
        return self.st.query(left, right)
```
