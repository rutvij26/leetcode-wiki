---
title: "Binary Search"
parent: Topics
nav_order: 16
---

## ELI5 (Explain Like I'm 5)

Imagine you're trying to find a word in a dictionary. You don't start from page 1 — you open to the middle. If your word comes before that page, you throw away the right half. If it comes after, you throw away the left half. You keep halving the book until you find the word. That's binary search — eliminate half the search space at every step.

## Explanation

- Binary search works on **sorted** input
- At each step, check the midpoint — if it's your target, done; if the target is smaller, search left half; if larger, search right half
- Each step cuts the problem in half → O(log n) instead of O(n)
- The tricky part is getting the **boundary conditions** right (`left <= right` vs `left < right`, `mid+1` vs `mid`)

> Keyword trigger: _"sorted array"_, _"find target"_, _"minimize/maximize something"_ with a monotonic condition → binary search.

## When to use?

- Input is sorted (or can be mapped to a sorted/monotonic condition)
- Searching for a value, an insertion point, or a boundary (first/last occurrence)
- "Find the minimum X such that condition(X) is true" — binary search on the answer
- Time limit would be exceeded with a linear scan

## Approach

There are two main templates:

### Template 1 — Exact Search
Find a target value in a sorted array.

```
left = 0, right = n - 1
while left <= right:
    mid = left + (right - left) // 2   ← avoids overflow
    if arr[mid] == target: return mid
    if arr[mid] < target:  left = mid + 1
    else:                  right = mid - 1
return -1
```

### Template 2 — Left Boundary (first position where condition is true)
```
left = 0, right = n   ← note: n not n-1
while left < right:
    mid = left + (right - left) // 2
    if condition(mid): right = mid    ← keep mid in range
    else:              left = mid + 1
return left
```

Use Template 2 for: "first bad version", "leftmost target", "minimum feasible value".

## Notes

- Time Complexity: O(log n)
- Space Complexity: O(1)
- Always use `mid = left + (right - left) / 2` to avoid integer overflow
- Off-by-one bugs are the #1 mistake — decide upfront: is `right` inclusive or exclusive?
- When binary searching on an **answer** (not an index), define `left`/`right` as the answer range, not array indices

## Data structures

- **Two pointers** (`left`, `right`) — define the current search range
- The input array itself (must be sorted, or your condition must be monotonic)

## How to Master This

### Step-by-step
1. Memorize **Template 1** (exact search) — get it right without thinking
2. Solve #704 (exact search, the template in pure form)
3. Solve #374 (first bad version) — forces you to use Template 2
4. Solve #33 (rotated sorted array) — teaches you how to identify which half is sorted
5. Solve #153 (min in rotated array) — boundary-finding variant
6. Solve #875 (binary search on answer, not index) — the hardest conceptual jump

### Key Resources
- 📹 [NeetCode — Binary Search playlist](https://www.youtube.com/playlist?list=PLot-Xpze53leU0Ec48pgtylmgwOrczKNC)
- 📹 [NeetCode — Find Minimum in Rotated Sorted Array](https://www.youtube.com/watch?v=nIVW4P8b1VA)
- 📝 [NeetCode.io — Binary Search problems](https://neetcode.io/roadmap) (see "Binary Search" section)
- 🔁 Drill in this order: [#704](https://leetcode.com/problems/binary-search) → [#374](https://leetcode.com/problems/guess-number-higher-or-lower) → [#33](https://leetcode.com/problems/search-in-rotated-sorted-array) → [#153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array) → [#875](https://leetcode.com/problems/koko-eating-bananas)

## Sample LeetCode problems

- [#704 Binary Search](https://leetcode.com/problems/binary-search)
- [#374 Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower)
- [#33 Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array)
- [#153 Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array)
- [#875 Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas)

## Code Samples

1. Binary Search — exact search (Template 1)

```typescript
/**
 * Search for target in a sorted array. Return its index or -1.
 * Input: nums = [-1,0,3,5,9,12], target = 9
 * Output: 4
 */
function search(nums: number[], target: number): number {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] === target) return mid;
    if (nums[mid] < target) left = mid + 1;
    else right = mid - 1;
  }

  return -1;
}
```

2. Find Minimum in Rotated Sorted Array — boundary search (Template 2)

```typescript
/**
 * Find the minimum element in a rotated sorted array (no duplicates).
 * Input: nums = [3,4,5,1,2]
 * Output: 1
 *
 * Key insight: the minimum is always in the "unsorted" half.
 * If nums[mid] > nums[right], minimum is to the RIGHT of mid.
 * Otherwise it's at mid or to the LEFT.
 */
function findMin(nums: number[]): number {
  let left = 0;
  let right = nums.length - 1;

  while (left < right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] > nums[right]) {
      // min is in the right half (not including mid)
      left = mid + 1;
    } else {
      // min is at mid or in the left half
      right = mid;
    }
  }

  return nums[left];
}
```

3. Koko Eating Bananas — binary search on the answer

```typescript
/**
 * Find the minimum eating speed k such that Koko can eat all bananas within h hours.
 * Each pile: eat min(pile, k) bananas per hour.
 * Input: piles = [3,6,7,11], h = 8
 * Output: 4
 *
 * Key insight: binary search on k (1 to max(piles)).
 * For a given k, check if sum of ceil(pile/k) <= h.
 */
function minEatingSpeed(piles: number[], h: number): number {
  let left = 1;
  let right = Math.max(...piles);

  while (left < right) {
    const mid = left + Math.floor((right - left) / 2);

    // hours needed to eat all piles at speed mid
    const hoursNeeded = piles.reduce((sum, pile) => sum + Math.ceil(pile / mid), 0);

    if (hoursNeeded <= h) {
      // mid is fast enough — try slower
      right = mid;
    } else {
      // too slow — need to eat faster
      left = mid + 1;
    }
  }

  return left;
}
```
