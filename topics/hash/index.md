---
title: "HashMap"
parent: Topics
nav_order: 1
---

## ELI5 (Explain Like I'm 5)

Imagine a coat check at a restaurant — you hand in your coat and get a ticket number. Later you use that ticket to find your coat instantly, instead of searching through every single coat. A HashMap works the same way: store something with a label, retrieve it in one step.

## Explanation

- A HashMap stores `key → value` pairs with O(1) average lookup
- The core question it answers: _"Have I seen this before?"_ or _"How many times have I seen X?"_
- Turns an O(n²) nested loop into O(n) by checking the map instead of re-scanning the array

> The Two Sum archetype: store `{value: index}` as you scan. For each number, check if its complement already exists in the map.

## When to use?

- You need to count frequencies of elements
- You need to check if a value was seen before (existence check)
- You need to look up a value by key in O(1)
- You want to avoid a nested loop on an array

## Approach

1. **Existence check** — store values you've seen, check before adding: `if map.has(x) → found it`
2. **Frequency count** — `map[x] = (map[x] || 0) + 1` to count occurrences
3. **Value → index map** — store `{value: index}` to find complement indices (Two Sum pattern)
4. **Grouping** — use a computed key (e.g. sorted chars) to group items into buckets (Group Anagrams)

## Notes

- Time Complexity: O(n) — one pass through the array
- Space Complexity: O(n) — worst case, everything goes into the map
- Hash collisions are handled internally — treat lookup as O(1) in interviews
- In JavaScript use `Map` (preserves insertion order, any key type) or plain objects `{}`

## Data structures

- **Map / Object** — the core structure
- **Array** — usually the input you're scanning

## How to Master This

### Step-by-step
1. Read this page and understand the 4 approaches
2. Watch the video below
3. Solve #1 Two Sum and #242 Valid Anagram without looking at solutions
4. Wait 24 hours, then attempt #49 Group Anagrams
5. Write the Two Sum solution from memory — that template covers 80% of HashMap problems

### Key Resources
- 📹 [NeetCode — Arrays & Hashing playlist](https://www.youtube.com/playlist?list=PLot-Xpze53ldVwtstag2TL4HQhAnC8ATf)
- 📹 [NeetCode — Two Sum walkthrough](https://www.youtube.com/watch?v=KLlXCFG5TnA)
- 📝 [NeetCode.io — Arrays & Hashing problems](https://neetcode.io/roadmap) (see "Arrays & Hashing" section)
- 🔁 Drill in this order: [#242](https://leetcode.com/problems/valid-anagram) → [#1](https://leetcode.com/problems/two-sum) → [#383](https://leetcode.com/problems/ransom-note) → [#49](https://leetcode.com/problems/group-anagrams)

## Sample LeetCode problems

- [#1 Two Sum](https://leetcode.com/problems/two-sum)
- [#242 Valid Anagram](https://leetcode.com/problems/valid-anagram)
- [#383 Ransom Note](https://leetcode.com/problems/ransom-note)
- [#49 Group Anagrams](https://leetcode.com/problems/group-anagrams)

## Code Samples

1. Two Sum ( value → index map )

```typescript
/**
 * Find two indices that add up to target
 * Input: nums = [2,7,11,15], target = 9
 * Output: [0,1]  (because nums[0] + nums[1] = 2 + 7 = 9)
 */
function twoSum(nums: number[], target: number): number[] {
  // map stores { value: index } for O(1) complement lookup
  const map = new Map<number, number>();

  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];

    // if complement already in map, we found our pair
    if (map.has(complement)) {
      return [map.get(complement)!, i];
    }

    // otherwise store current value → index
    map.set(nums[i], i);
  }

  return [];
}
```

2. Valid Anagram ( frequency count )

```typescript
/**
 * Check if two strings are anagrams of each other
 * Input: s = "anagram", t = "nagaram"
 * Output: true
 */
function isAnagram(s: string, t: string): boolean {
  if (s.length !== t.length) return false;

  const count = new Map<string, number>();

  // increment count for each char in s
  for (const char of s) {
    count.set(char, (count.get(char) || 0) + 1);
  }

  // decrement count for each char in t
  for (const char of t) {
    if (!count.has(char)) return false;
    count.set(char, count.get(char)! - 1);
    if (count.get(char)! < 0) return false;
  }

  return true;
}
```
