---
title: "Fenwick Tree / BIT"
parent: Topics
nav_order: 27
---

## ELI5 (Explain Like I'm 5)

A Fenwick Tree (Binary Indexed Tree / BIT) solves the same problem as a segment tree — fast prefix sums with updates — but with a clever bit manipulation trick. Instead of a full tree, it stores partial sums in an array where each position is responsible for a range determined by the lowest set bit in its index. It's more compact and faster in practice, but only handles prefix sum queries (not arbitrary range aggregates like min/max).

## Explanation

- A BIT supports two operations in O(log n): **prefix sum query** and **point update**
- The key insight: `i` is responsible for the range `[i - lowbit(i) + 1, i]` where `lowbit(i) = i & (-i)`
- To query prefix sum up to `i`: sum `tree[i] + tree[i - lowbit(i)] + ...` until index reaches 0
- To update index `i`: update `tree[i], tree[i + lowbit(i)], ...` until index exceeds n
- 1-indexed (shift array by 1)

> Prefer BIT over Segment Tree when you only need **prefix sums + point updates** — simpler code, faster constant factor.

## When to use?

- Prefix sum queries with point updates
- Counting inversions (merge sort alternative)
- "How many elements to the left of i are smaller than arr[i]?"
- Coordinate compression + BIT for rank queries
- When you want the simplest possible O(log n) update + query structure

## Approach

```python
class BIT:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)  # 1-indexed

    def update(self, i, delta):
        # add delta to position i, propagate up
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)       # next position: add lowest set bit

    def query(self, i):
        # prefix sum from 1 to i
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)       # parent: remove lowest set bit
        return total

    def range_query(self, l, r):
        return self.query(r) - self.query(l - 1)
```

### Key Bit Trick
`i & (-i)` = lowest set bit of i:
- `6 = 110₂` → `6 & (-6) = 010₂ = 2`
- `8 = 1000₂` → `8 & (-8) = 1000₂ = 8`

## Notes

- Time Complexity: O(n) build, O(log n) update, O(log n) query
- Space Complexity: O(n)
- BIT is **simpler to code** than a segment tree — memorize the 10-line template
- Only handles **prefix queries** naturally — for range min/max, use a segment tree
- Always 1-indexed — shift your input array by 1 when using BIT

## Data structures

- **Array of size n+1** — the BIT itself (1-indexed)

## How to Master This

### Step-by-step
1. Understand `i & (-i)` — trace through examples with binary representations
2. Memorize the update and query functions — they're symmetric (one adds, one subtracts the lowbit)
3. Solve #307 with BIT instead of segment tree — compare implementations
4. Solve #315 (count of smaller numbers) — BIT + coordinate compression

### Key Resources
- 📹 [William Fiset — Binary Indexed Tree](https://www.youtube.com/watch?v=RgITNht_f4Q)
- 📹 [Errichto — BIT / Fenwick Tree](https://www.youtube.com/watch?v=v_wj_mOAlig)
- 📝 [CP-Algorithms — Fenwick Tree](https://cp-algorithms.com/data_structures/fenwick.html)
- 🔁 Drill: [#307](https://leetcode.com/problems/range-sum-query-mutable) → [#315](https://leetcode.com/problems/count-of-smaller-numbers-after-self) → [#493](https://leetcode.com/problems/reverse-pairs)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 307 | [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable) | Medium | High | ✅ |
| 315 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self) | Hard | Medium | ✅ |
| 493 | [Reverse Pairs](https://leetcode.com/problems/reverse-pairs) | Hard | Low | 📖 |
| 327 | [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum) | Hard | Low | 📖 |

## Code Samples

1. Range Sum Query Mutable — BIT implementation

```python
class NumArray:
    def __init__(self, nums: list[int]):
        self.n = len(nums)
        self.tree = [0] * (self.n + 1)  # 1-indexed
        self.nums = [0] * self.n

        for i, val in enumerate(nums):
            self._update(i + 1, val)  # build BIT
            self.nums[i] = val

    def _update(self, i: int, delta: int) -> None:
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)  # move to next responsible node

    def _prefix_sum(self, i: int) -> int:
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)  # move to parent node
        return total

    def update(self, index: int, val: int) -> None:
        delta = val - self.nums[index]
        self.nums[index] = val
        self._update(index + 1, delta)  # BIT is 1-indexed

    def sumRange(self, left: int, right: int) -> int:
        return self._prefix_sum(right + 1) - self._prefix_sum(left)
```

2. Count of Smaller Numbers After Self — BIT + coordinate compression

```python
def countSmaller(nums: list[int]) -> list[int]:
    # coordinate compression: map values to 1..n range
    sorted_unique = sorted(set(nums))
    rank = {v: i + 1 for i, v in enumerate(sorted_unique)}
    m = len(sorted_unique)

    tree = [0] * (m + 1)

    def update(i: int) -> None:
        while i <= m:
            tree[i] += 1
            i += i & (-i)

    def query(i: int) -> int:
        total = 0
        while i > 0:
            total += tree[i]
            i -= i & (-i)
        return total

    result = []
    # process right to left — query how many smaller are already in BIT
    for num in reversed(nums):
        r = rank[num]
        result.append(query(r - 1))  # count of elements with rank < r
        update(r)

    return result[::-1]
```
