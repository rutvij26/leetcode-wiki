---
title: "Two Pointers"
---

## Explanation

- Two pointers just the way it sounds uses `two pointers/variables` keeps track of array or string indices
- Saves space and time

> Pointers? : They are reference to an object, bascially memory address of some value

## When to use?

- Analyze each element of the collection compare to other elements

## Approach

1. Two pointers, each starting from `beginning` and the `end` until they both meet

2. One pointer moving at a slow pace, while the other pointer moves at twice the speed

3. If two arrays, start pointer at same position and merge them

4. Partition and split linked list ( Rare ) [#86](https://leetcode.com/problems/partition-list)

## Notes

> Can be made more efficient by iterating through two parts of an object simultaneously.

## Data structures

- Array
- String
- Linked Lists

## Sample Leet code problems

- [#125 Valid Palindrome (Approach #1)](https://leetcode.com/problems/valid-palindrome)
- [#167 Two Sum II Input Array is sorted (Approach #1)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted)
- [#15. 3Sum (Approach #1) Medium Tough Logic](https://leetcode.com/problems/3sum) [Youtube](https://www.youtube.com/watch?v=jzZsG8n2R9A)
- [#11 Container With Most Water (Approach #1)](https://leetcode.com/problems/container-with-most-water)
- [#392 Is Subsequence (Approach #3)](https://leetcode.com/problems/is-subsequence)
- [#86 Partition List (Approach #4)](https://leetcode.com/problems/partition-list)

## Code Samples

1. Two Sum ( Approach #1 )

```javascript
function twoSum(arr, target) {
  // pointer at start of array
  const firstPointer = 0;

  // pointer at end of array
  const secondPointer = arr.length - 1;

  const sum = 0;

  while (firstPointer < secondPointer) {
    // calculate the sum of pair
    sum = arr[firstPointer] + arr[secondPointer];
    // return if sum is found
    if (sum === target) return true;
    // if value is less than target increment the first pointer
    else if (sum < target) pointerOne += 1;
    // if value is greater than target decrement the second pointer
    else pointerTwo -= 1;
  }
  return false;
}
```

2. Detect Cycle ( Approach #2 )

```typescript
/**
 * We are detecting a cycle in linked list
 * 1 -> 2 -> 3 <-> 4 ( 4 points back to 3 )
 * */

// linked list interface
interface Node {
  value: any;
  next: Node | null;
}

function detectCycle(head: Node) {
  // start both the pointers at the same position
  const slow: Node | null = head;
  const fast: Node | null = head;

  while (fast !== null && fast.next !== null) {
    // increment this one by one
    slow = slow.next;
    // increment this one by twice
    fast = fast.next.next;

    // if slow and fast both are same that means there's a cycle
    if (slow === fast) return true;
  }
}
```

3. Merge Sorted Array ( Approach 3 )

```typescript
/**
 Do not return anything, modify nums1 in-place instead.
 nums1 = [1,2,3,4,0,0,0]; m=4; nums2 = [2,5,6]; n =3
 output = [1,2,2,3,4,5,6]
 */
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
  let [i, j, k] = [m - 1, n - 1, nums1.length - 1];

  while (i >= 0 && j >= 0) {
    // i < j
    if (nums1[i] < nums2[j]) {
      nums1[k] = nums2[j];
      k -= 1;
      j -= 1;
    }
    // j < i
    else {
      nums1[k] = nums1[i];
      k -= 1;
      i -= 1;
    }
  }

  while (j >= 0) {
    nums1[k] = nums2[j];
    k -= 1;
    j -= 1;
  }

  while (i >= 0) {
    nums1[k] = nums1[i];
    k -= 1;
    i -= 1;
  }
}
```

4. Partition List

```typescript
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 *
 * Input: head = [1,4,3,2,5,2], x = 3
 * Output: [1,2,2,4,3,5]
 */

function partition(head: ListNode | null, x: number): ListNode | null {
  let bigHead = new ListNode();
  let smallHead = new ListNode();

  let big = bigHead;
  let small = smallHead;

  let n = head;

  while (n) {
    if (n.val < x) {
      small.next = n;
      small = small.next;
      big.next = null;
    } else {
      big.next = n;
      big = big.next;
      small.next = null;
    }

    n = n.next;
  }
  small.next = bigHead.next;
  big.next = null;

  return smallHead.next;
}
```
