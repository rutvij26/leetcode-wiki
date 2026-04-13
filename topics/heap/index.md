---
title: "Heap / Priority Queue"
parent: Topics
nav_order: 10
---

## ELI5 (Explain Like I'm 5)

Imagine a doctor's waiting room where the sickest patient always gets called first, no matter when they arrived. Every time a new patient comes in, they're slotted into the right position. You always know who's at the front without sorting everyone every single time. That's a heap — it always gives you the highest-priority item instantly.

## Explanation

- A **heap** (or priority queue) is a data structure that gives you the min or max element in O(1), and insert/remove in O(log n)
- **Min-heap**: smallest element is always at the top
- **Max-heap**: largest element is always at the top
- The killer use case: **Top-K problems** — "find the K largest/smallest elements from n items"

> Top-K trick: to find **K largest** → use a **min-heap of size K** (pop the smallest when size > K, so only the K largest remain). To find **K smallest** → use a **max-heap of size K**.

## When to use?

- Find the K largest or K smallest elements
- Continuously retrieve the minimum/maximum as new items arrive (streaming data)
- Merge K sorted lists
- When you need O(log n) insert and O(1) peek — faster than re-sorting each time

## Approach

1. **Build a heap** from the input — O(n) with heapify
2. **Push / pop as you scan** — for Top-K: push each element, pop when size exceeds K; what remains is your answer
3. **For K largest**: min-heap — the heap evicts the smallest, keeping the K biggest
4. **For K smallest**: max-heap — the heap evicts the largest, keeping the K smallest

### Algorithm Steps (Top K Frequent Elements):
1. Count frequencies using a HashMap
2. Use a min-heap of size K on (frequency, element) pairs
3. For each element: push to heap; if heap size > K, pop the smallest frequency
4. The heap now contains the K most frequent elements

## Notes

- Time Complexity: O(n log k) — n insertions, each O(log k) since heap size is capped at K
- Space Complexity: O(k) for the heap + O(n) for the frequency map
- Beats full sort O(n log n) when K << N or on streaming data
- JavaScript has no built-in heap — in interviews you can use a sorted array for small K, or implement a MinHeap class

## Data structures

- **Min-heap** — underlying array where `arr[i] <= arr[2i+1]` and `arr[i] <= arr[2i+2]`
- **HashMap** — usually needed alongside a heap to compute frequencies
- **Array** — used as the backing store for heap implementation

## How to Master This

### Step-by-step
1. Read this page and understand the top-K trick (min-heap for K largest, max-heap for K smallest)
2. Watch the NeetCode video — the "why min-heap for K largest" part is the key insight
3. Solve #215 (kth largest) to verify you understand the trick
4. Solve #347 (top K frequent) — it adds a HashMap + heap combination
5. Understand the complexity: why is it O(n log k) and not O(n log n)?

### Key Resources
- 📹 [NeetCode — Kth Largest Element in an Array](https://www.youtube.com/watch?v=XEmy13g1Qxc)
- 📹 [NeetCode — Top K Frequent Elements](https://www.youtube.com/watch?v=YPTqKIgVk-k)
- 📝 [NeetCode.io — Heap / Priority Queue problems](https://neetcode.io/roadmap) (see "Heap / Priority Queue" section)
- 🔁 Drill in this order: [#1046](https://leetcode.com/problems/last-stone-weight) → [#215](https://leetcode.com/problems/kth-largest-element-in-an-array) → [#347](https://leetcode.com/problems/top-k-frequent-elements) → [#703](https://leetcode.com/problems/kth-largest-element-in-a-stream)

## Sample LeetCode problems

- [#1046 Last Stone Weight](https://leetcode.com/problems/last-stone-weight)
- [#215 Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array)
- [#347 Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements)
- [#703 Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream)

## Code Samples

1. Top K Frequent Elements ( HashMap + simulated min-heap )

```typescript
/**
 * Return the k most frequent elements
 * Input: nums = [1,1,1,2,2,3], k = 2
 * Output: [1,2]
 *
 * Note: JS has no built-in heap. This uses a bucket sort approach
 * which is O(n) — even better than a heap for this specific problem.
 */
function topKFrequent(nums: number[], k: number): number[] {
  // step 1: count frequencies
  const freq = new Map<number, number>();
  for (const n of nums) {
    freq.set(n, (freq.get(n) || 0) + 1);
  }

  // step 2: bucket sort — index = frequency, value = list of nums with that freq
  const buckets: number[][] = Array.from({ length: nums.length + 1 }, () => []);
  for (const [num, count] of freq) {
    buckets[count].push(num);
  }

  // step 3: read from highest frequency bucket down until we have k elements
  const result: number[] = [];
  for (let i = buckets.length - 1; i >= 0 && result.length < k; i--) {
    result.push(...buckets[i]);
  }

  return result.slice(0, k);
}
```

2. Kth Largest Element ( simulated min-heap using sorted array )

```typescript
/**
 * Find the kth largest element in an array
 * Input: nums = [3,2,1,5,6,4], k = 2
 * Output: 5
 *
 * Heap approach: maintain a min-heap of size k.
 * The top of the heap (minimum in the heap) is the kth largest.
 */
function findKthLargest(nums: number[], k: number): number {
  // simulate a min-heap with a sorted array (acceptable for interviews)
  const minHeap: number[] = [];

  for (const num of nums) {
    // add to heap (maintain sorted order for simulation)
    minHeap.push(num);
    minHeap.sort((a, b) => a - b); // O(k log k)

    // keep heap size at k — evict the smallest
    if (minHeap.length > k) {
      minHeap.shift(); // remove the minimum
    }
  }

  // the top of the min-heap is the kth largest
  return minHeap[0];
}
```
