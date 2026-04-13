---
title: "Sliding Window"
parent: Topics
nav_order: 3
---

## ELI5 (Explain Like I'm 5)

Imagine you're looking at a long street through the window of a moving car. You only ever see a chunk of the street at one time, and as you drive forward, old things leave your view and new things appear. That's sliding window — you maintain a "view" over part of the array that moves and resizes as needed.

## Explanation

- A sliding window is a subarray or substring that you move across the input
- Instead of checking every possible subarray (O(n²)), you grow the window from the right and shrink it from the left — O(n) total
- The window expands by moving the right pointer forward, and shrinks by moving the left pointer forward when a constraint is violated

> Keyword trigger: _"longest/shortest subarray or substring with property X"_ → sliding window.

## When to use?

- Find the longest/shortest subarray or substring matching some condition
- The input is a contiguous sequence (array or string)
- The constraint is about what's _inside_ the current window (sum, unique chars, frequency, etc.)

## Approach

1. **Fixed-size window** — window size `k` never changes; just add the new right element, subtract the exiting left element
2. **Variable window (shrink on violation)** — expand right freely; when the window violates the constraint, shrink from the left until it's valid again

### Algorithm Steps (variable window)
1. Initialize `left = 0`, result variable, and a data structure to track window state (Map, counter, running sum)
2. Move `right` from `0` to `n-1`
3. Add `arr[right]` to window state
4. While window is invalid → remove `arr[left]` from state, increment `left`
5. Update result with current window size (`right - left + 1`)

## Notes

- Time Complexity: O(n) — each element enters and exits the window at most once
- Space Complexity: O(k) where k is the size of the window or alphabet
- Don't use nested loops — that defeats the purpose
- The "shrink" step is a `while` loop, not an `if` — keep shrinking until valid

## Data structures

- **Two pointers** (`left`, `right`) — define the window boundaries
- **Map / counter** — track character/element frequencies inside the window
- **Running sum** — for numeric subarrays

## How to Master This

### Step-by-step
1. Read this page and understand the two window types (fixed vs variable)
2. Watch the video below — the template becomes obvious after seeing it once
3. Solve #643 (fixed size, warm-up) then #3 (variable, classic)
4. Try #76 only after you can solve #3 without hints — it's the hard version of the same idea
5. Write the variable window template from memory

### Key Resources
- 📹 [NeetCode — Sliding Window playlist](https://www.youtube.com/playlist?list=PLot-Xpze53leU0Ec48pgtylmgwOrczKNC)
- 📹 [NeetCode — Longest Substring Without Repeating Characters](https://www.youtube.com/watch?v=wiGpQwVHdE0)
- 📝 [NeetCode.io — Sliding Window problems](https://neetcode.io/roadmap) (see "Sliding Window" section)
- 🔁 Drill in this order: [#643](https://leetcode.com/problems/maximum-average-subarray-i) → [#3](https://leetcode.com/problems/longest-substring-without-repeating-characters) → [#567](https://leetcode.com/problems/permutation-in-string) → [#76](https://leetcode.com/problems/minimum-window-substring)

## Sample LeetCode problems

- [#643 Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i)
- [#3 Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)
- [#567 Permutation in String](https://leetcode.com/problems/permutation-in-string)
- [#76 Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)

## Code Samples

1. Maximum Average Subarray I ( fixed-size window )

```typescript
/**
 * Find max average of any contiguous subarray of length k
 * Input: nums = [1,12,-5,-6,50,3], k = 4
 * Output: 12.75  (subarray [12,-5,-6,50] has avg 12.75)
 */
function findMaxAverage(nums: number[], k: number): number {
  // compute sum of first window
  let windowSum = nums.slice(0, k).reduce((a, b) => a + b, 0);
  let maxSum = windowSum;

  for (let i = k; i < nums.length; i++) {
    // slide: add new right element, remove old left element
    windowSum += nums[i] - nums[i - k];
    maxSum = Math.max(maxSum, windowSum);
  }

  return maxSum / k;
}
```

2. Longest Substring Without Repeating Characters ( variable window )

```typescript
/**
 * Find the length of the longest substring without repeating characters
 * Input: s = "abcabcbb"
 * Output: 3  ("abc")
 */
function lengthOfLongestSubstring(s: string): number {
  // tracks last seen index of each character
  const lastSeen = new Map<string, number>();
  let left = 0;
  let maxLen = 0;

  for (let right = 0; right < s.length; right++) {
    const char = s[right];

    // if char was seen inside the current window, shrink left past it
    if (lastSeen.has(char) && lastSeen.get(char)! >= left) {
      left = lastSeen.get(char)! + 1;
    }

    lastSeen.set(char, right);
    maxLen = Math.max(maxLen, right - left + 1);
  }

  return maxLen;
}
```
