---
title: Interview Strategy
nav_order: 5
---

# Interview Strategy

The coding interview is not just about solving the problem — it's about demonstrating how you think. Use this guide to handle any problem confidently, even ones you've never seen.

---

## The 5-Step Framework

Follow this every single time, without exception:

### 1. Understand (2–3 min)
- Restate the problem in your own words
- Ask clarifying questions before writing a single line of code:
  - What are the input types and constraints? (integers? strings? could it be empty?)
  - Can there be duplicates?
  - Is the input sorted?
  - What should I return if there's no answer?
- Work through the given example out loud

### 2. Examples (2–3 min)
- Come up with your own examples: a normal case, an edge case, a large input
- Edge cases to always check: empty input, single element, all duplicates, negative numbers, very large n
- The example you work through often reveals the algorithm

### 3. Approach (3–5 min)
- Brainstorm out loud — don't jump to code
- Start with brute force and state its complexity
- Use the [Patterns Cheat Sheet](./patterns-cheatsheet.md) mentally: what keywords does this problem trigger?
- Then optimize: "Can I avoid recomputing X?" / "Can I sort the input first?" / "Is there a greedy choice?"
- Get the interviewer's buy-in before coding: "I'm going to use a sliding window — does that sound reasonable?"

### 4. Code (15–20 min)
- Write clean, readable code — variable names matter
- Narrate what you're doing: "I'm initializing a left pointer at 0..."
- Don't delete and restart — modify in place, explain changes
- Use helper functions if the logic gets complex

### 5. Test (3–5 min)
- Trace through your example manually — don't just say "looks right"
- Run through an edge case
- Check for off-by-one errors, empty inputs, and integer overflow
- Verify your stated complexity

---

## Time Management

| Phase | Easy | Medium | Hard |
|-------|------|--------|------|
| Understand + Examples | 3 min | 4 min | 5 min |
| Approach | 3 min | 5 min | 8 min |
| Code | 10 min | 15 min | 20 min |
| Test | 3 min | 4 min | 5 min |
| **Total** | **~20 min** | **~30 min** | **~40 min** |

If you're stuck after 10 minutes on the approach: ask for a hint. Spending 20 minutes silent and stuck is worse than asking.

---

## When You're Stuck

**Stuck on the approach:**
- Try brute force first — even if it's O(n³), stating it shows you understand the problem
- Ask yourself: "What information do I keep recomputing?" → memoize it
- Ask: "What if I sorted the input?" → often unlocks binary search or greedy
- Ask: "What if I processed from right to left / bottom up?"

**Stuck on the code:**
- Skip the tricky part, write a placeholder, and come back: `# TODO: handle edge case X`
- Think out loud — the interviewer can give you a nudge if you're close

**Got a wrong answer:**
- Don't panic. Say: "Let me trace through this test case step by step"
- Bugs are expected — how you debug matters more than never having a bug

---

## Communication Dos and Don'ts

| Do | Don't |
|----|-------|
| Think out loud continuously | Go silent for more than 30 seconds |
| State complexity proactively | Wait to be asked for complexity |
| Verify your approach before coding | Start coding the first idea you have |
| Ask clarifying questions upfront | Make assumptions about the input |
| Name your variables descriptively | Use `x`, `y`, `z` for everything |
| Say "I'm not sure but here's my thinking" | Pretend to be confident when you're not |

---

## Complexity Cheat Sheet

Always state both **time** and **space** complexity at the end.

| O(...) | Name | When it appears |
|--------|------|-----------------|
| O(1) | Constant | Hash map lookup, arithmetic |
| O(log n) | Logarithmic | Binary search, balanced BST |
| O(n) | Linear | Single pass, two pointers, BFS/DFS |
| O(n log n) | Linearithmic | Sorting, heap operations on n items |
| O(n²) | Quadratic | Nested loops, naive DP |
| O(2ⁿ) | Exponential | Backtracking (subsets) |
| O(n!) | Factorial | Backtracking (permutations) |

---

## After the Problem

Even if you solve it perfectly, the interview isn't over:

1. **Follow-up questions are common.** Be ready for: "How would you handle duplicates?", "What if the input is a stream?", "Can you reduce space complexity?"
2. **Trade-off discussion.** If you used extra space for speed, explain why. Know the alternative.
3. **Behavioral close.** Have your behavioral answer for "tell me about a challenging technical problem" ready.

---

## Common Interview Patterns by Company Tier

| Tier | Common Topics |
|------|--------------|
| Big Tech (FAANG) | DP, Graphs, Trees, System Design |
| Mid-size tech | Arrays, Strings, Trees, Linked Lists |
| Startups | Practical problems, sometimes just take-home |
| Fintech | DP, Probability, sometimes SQL |

---

## Red Flags to Avoid

- Immediately coding without understanding the problem
- Never testing your code
- Saying "I don't know" and stopping — always say what you *do* know and build from there
- Ignoring edge cases (empty array, single element, all duplicates)
- Not knowing the time/space complexity of your own solution
