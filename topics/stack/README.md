---
title: "Stack"
---

## ELI5 (Explain Like I'm 5)

Think of a stack of pancakes. You always add new pancakes on top, and you always eat from the top too. The most recent pancake you added is the first one you'll eat. A stack in code works exactly the same — last in, first out.

## Explanation

- A stack is a LIFO (Last In, First Out) data structure
- Core mental model: _"the most recent unresolved thing"_
- Push when you open something (e.g. `(`), pop when you close it (e.g. `)`)
- **Monotonic stack**: before pushing, pop elements that violate a sorted order — this lets you efficiently find "next greater" or "next smaller" elements

> When you see "next greater element", "previous smaller element", or "span" problems → monotonic stack, always.

## When to use?

- Matching/balancing brackets or tags
- "Next greater / smaller element" problems
- Undo/redo, backtracking, or call-stack simulation
- Evaluating expressions
- Keeping a running history of "last seen" items

## Approach

1. **Balanced brackets** — push open brackets, pop and compare when you see a closing bracket
2. **Monotonic decreasing stack** (next greater element) — pop all elements smaller than the current before pushing; popped elements now have their "next greater" answer
3. **Monotonic increasing stack** (next smaller element) — pop all elements greater than the current before pushing

### Algorithm Steps (monotonic — next greater):
1. Initialize an empty stack and a result array
2. For each element `nums[i]`:
   - While stack is not empty and `nums[i] > nums[stack.top()]` → pop index, set `result[popped] = nums[i]`
   - Push current index onto stack
3. Remaining elements in stack have no next greater element → set to `-1`

## Notes

- Time Complexity: O(n) — each element is pushed and popped at most once
- Space Complexity: O(n) — worst case all elements on the stack
- Use an array as a stack in JS/TS: `push()` to add, `pop()` to remove, `arr[arr.length - 1]` to peek
- Monotonic stack stores indices, not values (so you can update the result array)

## Data structures

- **Array (used as stack)** — `push` / `pop` / peek at `arr[arr.length - 1]`
- **Stack of indices** — for monotonic variants where you need to update results by position

## How to Master This

### Step-by-step
1. Read this page and understand the difference between a plain stack and a monotonic stack
2. Watch the NeetCode videos below — the monotonic stack clicks fast once you see it animated
3. Solve #20 Valid Parentheses first (plain stack, easy warm-up)
4. Then solve #739 Daily Temperatures (monotonic stack, the canonical example)
5. Write the monotonic stack template from memory — it applies to a whole family of problems

### Key Resources
- 📹 [NeetCode — Valid Parentheses](https://www.youtube.com/watch?v=WTzjTskDFMg)
- 📹 [NeetCode — Daily Temperatures](https://www.youtube.com/watch?v=cTBiBSnjO3c)
- 📝 [NeetCode.io — Stack problems](https://neetcode.io/roadmap) (see "Stack" section)
- 🔁 Drill in this order: [#20](https://leetcode.com/problems/valid-parentheses) → [#739](https://leetcode.com/problems/daily-temperatures) → [#496](https://leetcode.com/problems/next-greater-element-i) → [#84](https://leetcode.com/problems/largest-rectangle-in-histogram)

## Sample LeetCode problems

- [#20 Valid Parentheses](https://leetcode.com/problems/valid-parentheses)
- [#739 Daily Temperatures](https://leetcode.com/problems/daily-temperatures)
- [#496 Next Greater Element I](https://leetcode.com/problems/next-greater-element-i)
- [#84 Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)

## Code Samples

1. Valid Parentheses ( plain stack )

```typescript
/**
 * Check if brackets are correctly opened and closed
 * Input: s = "()[]{}"  → Output: true
 * Input: s = "([)]"    → Output: false
 */
function isValid(s: string): boolean {
  const stack: string[] = [];

  // map each closing bracket to its expected opening bracket
  const pairs: Record<string, string> = {
    ')': '(',
    ']': '[',
    '}': '{',
  };

  for (const char of s) {
    if (!pairs[char]) {
      // it's an opening bracket — push it
      stack.push(char);
    } else {
      // it's a closing bracket — top of stack must match
      if (stack.pop() !== pairs[char]) return false;
    }
  }

  // valid only if all brackets were matched
  return stack.length === 0;
}
```

2. Daily Temperatures ( monotonic decreasing stack )

```typescript
/**
 * For each day, how many days until a warmer temperature?
 * Input: temperatures = [73,74,75,71,69,72,76,73]
 * Output: [1,1,4,2,1,1,0,0]
 */
function dailyTemperatures(temperatures: number[]): number[] {
  const result = new Array(temperatures.length).fill(0);
  // stack stores indices of days waiting for a warmer day
  const stack: number[] = [];

  for (let i = 0; i < temperatures.length; i++) {
    // pop all days cooler than today — today is their answer
    while (stack.length > 0 && temperatures[i] > temperatures[stack[stack.length - 1]]) {
      const prevDay = stack.pop()!;
      result[prevDay] = i - prevDay;
    }
    stack.push(i);
  }

  // remaining indices in stack have no warmer future day → stay 0
  return result;
}
```
