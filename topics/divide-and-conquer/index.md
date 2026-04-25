---
title: "Divide and Conquer"
parent: Topics
nav_order: 17
---

## ELI5 (Explain Like I'm 5)

You have 1000 papers to sort. Instead of sorting all 1000 at once, you split them into two piles of 500, sort each pile separately (by splitting again, and again…), then merge the two sorted piles together. That's divide and conquer — split the problem in half, solve each half, combine the results.

## Explanation

- Divide and conquer splits a problem into **independent subproblems**, solves them recursively, then **merges** the results
- Unlike DP, the subproblems don't overlap — no memoization needed
- Three phases: **Divide** (split), **Conquer** (recurse), **Combine** (merge)
- Classic examples: merge sort, quick sort, binary search, closest pair of points

> Keyword trigger: _"sort"_, _"find kth largest"_, _"inversion count"_, _"closest pair"_ → consider divide and conquer.

## When to use?

- Problem can be split into non-overlapping subproblems of the same type
- Merging solutions from subproblems is straightforward
- Sorting-related problems
- Finding kth element (quick select)

## Approach

### Merge Sort Template
```
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])    # conquer left
    right = merge_sort(arr[mid:])   # conquer right
    return merge(left, right)        # combine
```

### Quick Select Template (find kth smallest)
```
def quick_select(arr, k):
    pivot = arr[random index]
    left = [x for x in arr if x < pivot]
    mid = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    if k <= len(left):
        return quick_select(left, k)
    elif k <= len(left) + len(mid):
        return pivot
    else:
        return quick_select(right, k - len(left) - len(mid))
```

## Notes

- Merge Sort: Time O(n log n), Space O(n)
- Quick Select: Average O(n), Worst O(n²) — use random pivot to avoid worst case
- Divide and conquer ≠ DP — D&C subproblems are independent; DP subproblems overlap
- Python's built-in `sort()` uses Timsort (hybrid merge sort) — you rarely need to implement merge sort in interviews; quick select is more common

## Data structures

- **Recursion** — the natural mechanism for D&C
- **Auxiliary array** — merge sort needs temp space during the merge step

## How to Master This

### Step-by-step
1. Implement merge sort from scratch — understand the merge step
2. Solve #215 (kth largest) — quick select is the key skill
3. Solve #148 (sort list) — merge sort on a linked list (common interview problem)
4. Solve #23 (merge k sorted lists) — D&C applied to k-way merging

### Key Resources
- 📹 [NeetCode — Kth Largest Element](https://www.youtube.com/watch?v=XEmy13g1Qxc)
- 📹 [NeetCode — Sort List](https://www.youtube.com/watch?v=TGveA1oFglI)
- 📝 [NeetCode.io — Sorting section](https://neetcode.io/roadmap)
- 🔁 Drill: [#215](https://leetcode.com/problems/kth-largest-element-in-an-array) → [#148](https://leetcode.com/problems/sort-list) → [#23](https://leetcode.com/problems/merge-k-sorted-lists) → [#315](https://leetcode.com/problems/count-of-smaller-numbers-after-self)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 215 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array) | Medium | Very High | ✅ |
| 148 | [Sort List](https://leetcode.com/problems/sort-list) | Medium | High | ✅ |
| 23 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists) | Hard | Very High | ✅ |
| 315 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self) | Hard | Medium | ⚡ |
| 240 | [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii) | Medium | Medium | ⚡ |

## Code Samples

1. Kth Largest Element — Quick Select

```python
import random

def findKthLargest(nums: list[int], k: int) -> int:
    # quick select: partition around pivot, recurse on the right side
    def quick_select(left: int, right: int) -> int:
        pivot = nums[random.randint(left, right)]
        lo, hi = left, right
        mid = left

        while mid <= hi:
            if nums[mid] < pivot:
                nums[lo], nums[mid] = nums[mid], nums[lo]
                lo += 1
                mid += 1
            elif nums[mid] > pivot:
                nums[mid], nums[hi] = nums[hi], nums[mid]
                hi -= 1
            else:
                mid += 1

        # after partition: nums[left..lo-1] < pivot, nums[lo..hi] == pivot, nums[hi+1..right] > pivot
        # kth largest is at index n-k
        target = len(nums) - k
        if target < lo:
            return quick_select(left, lo - 1)
        elif target > hi:
            return quick_select(hi + 1, right)
        else:
            return pivot

    return quick_select(0, len(nums) - 1)
```

2. Sort List — Merge Sort on Linked List

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def sortList(head: Optional[ListNode]) -> Optional[ListNode]:
    if not head or not head.next:
        return head

    # find middle using slow/fast pointers
    slow, fast = head, head.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    # split list into two halves
    mid = slow.next
    slow.next = None

    left = sortList(head)
    right = sortList(mid)

    # merge two sorted lists
    dummy = ListNode()
    curr = dummy
    while left and right:
        if left.val <= right.val:
            curr.next = left
            left = left.next
        else:
            curr.next = right
            right = right.next
        curr = curr.next

    curr.next = left or right
    return dummy.next
```
