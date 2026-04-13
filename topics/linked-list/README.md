---
title: "Linked List"
---

## ELI5 (Explain Like I'm 5)

Imagine a treasure hunt where each clue is a note that tells you _exactly_ where to find the next clue. There's no map — you can only follow clues one at a time, and you can't jump straight to clue #5 without reading clues 1 through 4 first. A linked list works the same way: each node only knows about the next node.

## Explanation

- A linked list is a chain of nodes; each node has a `value` and a `next` pointer
- No random access by index — you must traverse from the start
- Three tricks that solve almost every linked list problem:
  - **Dummy head**: create a fake node before the real head to simplify edge cases
  - **Fast / slow pointers**: two pointers moving at different speeds for cycle detection and finding the middle
  - **Iterative reversal**: flip `next` pointers one by one to reverse in place

> Always draw the list on paper first. Pointer manipulation is easy to get wrong if you're only holding it in your head.

## When to use?

- Reverse a list or portion of it
- Detect or find the start of a cycle
- Find the middle node
- Merge two sorted lists
- Remove the Nth node from the end

## Approach

1. **Dummy head** — create `const dummy = new ListNode(0); dummy.next = head`. Build your result off `dummy.next` so you don't need to special-case the head
2. **Fast / slow pointers** — `slow` moves one step, `fast` moves two. They meet at the cycle point (Floyd's algorithm) or `slow` lands on the middle when `fast` exits
3. **Iterative reversal** — three variables: `prev = null`, `curr = head`. Each step: save `curr.next`, point `curr.next` to `prev`, advance both

### Algorithm Steps (reverse a linked list):
1. `prev = null`, `curr = head`
2. While `curr !== null`:
   - Save `next = curr.next`
   - Point `curr.next = prev`
   - Move `prev = curr`, `curr = next`
3. Return `prev` (new head)

## Notes

- Time Complexity: O(n) for most operations — single pass
- Space Complexity: O(1) for iterative approaches (no extra storage), O(n) for recursive
- Don't use recursion for reversal in interviews unless asked — iterative is safer and O(1) space
- Always check for `null` before accessing `.next` — a common source of runtime errors

## Data structures

- **ListNode** — the node class (`val`, `next`)
- **Two pointers** — for fast/slow and multi-list problems

## How to Master This

### Step-by-step
1. Read this page and understand the three core tricks
2. Draw out the pointer manipulations on paper before coding
3. Solve #206 (reversal) and #876 (fast/slow) back to back — they build the two key techniques
4. Then solve #21 (merge) using a dummy head
5. Try #141 last — it's fast/slow but with a twist

### Key Resources
- 📹 [NeetCode — Reverse Linked List](https://www.youtube.com/watch?v=G0_I-ZF0S38)
- 📹 [NeetCode — Linked List Cycle](https://www.youtube.com/watch?v=gBTe7lFR3vc)
- 📝 [NeetCode.io — Linked List problems](https://neetcode.io/roadmap) (see "Linked List" section)
- 🔁 Drill in this order: [#206](https://leetcode.com/problems/reverse-linked-list) → [#876](https://leetcode.com/problems/middle-of-the-linked-list) → [#21](https://leetcode.com/problems/merge-two-sorted-lists) → [#141](https://leetcode.com/problems/linked-list-cycle)

## Sample LeetCode problems

- [#206 Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)
- [#876 Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list)
- [#21 Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)
- [#141 Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)

## Code Samples

1. Reverse Linked List ( iterative reversal )

```typescript
/**
 * Reverse a singly linked list
 * Input:  1 -> 2 -> 3 -> 4 -> 5 -> null
 * Output: 5 -> 4 -> 3 -> 2 -> 1 -> null
 */
function reverseList(head: ListNode | null): ListNode | null {
  let prev: ListNode | null = null;
  let curr: ListNode | null = head;

  while (curr !== null) {
    const next = curr.next; // save next before we overwrite it
    curr.next = prev;       // flip the pointer backward
    prev = curr;            // advance prev
    curr = next;            // advance curr
  }

  return prev; // prev is the new head
}
```

2. Middle of the Linked List ( fast / slow pointers )

```typescript
/**
 * Return the middle node of a linked list
 * Input:  1 -> 2 -> 3 -> 4 -> 5
 * Output: node with value 3
 * If even length: return second middle (1->2->3->4 → node 3)
 */
function middleNode(head: ListNode | null): ListNode | null {
  let slow: ListNode | null = head;
  let fast: ListNode | null = head;

  // fast moves twice as fast — when fast reaches the end, slow is at the middle
  while (fast !== null && fast.next !== null) {
    slow = slow!.next;
    fast = fast.next.next;
  }

  return slow;
}
```

3. Merge Two Sorted Lists ( dummy head )

```typescript
/**
 * Merge two sorted linked lists into one sorted list
 * Input:  list1 = 1->2->4,  list2 = 1->3->4
 * Output: 1->1->2->3->4->4
 */
function mergeTwoLists(
  list1: ListNode | null,
  list2: ListNode | null
): ListNode | null {
  // dummy node eliminates the need to special-case the head
  const dummy = new ListNode(0);
  let curr = dummy;

  while (list1 !== null && list2 !== null) {
    if (list1.val <= list2.val) {
      curr.next = list1;
      list1 = list1.next;
    } else {
      curr.next = list2;
      list2 = list2.next;
    }
    curr = curr.next;
  }

  // attach the remaining non-empty list
  curr.next = list1 ?? list2;

  return dummy.next;
}
```
