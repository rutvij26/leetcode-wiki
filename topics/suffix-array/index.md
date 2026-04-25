---
title: "Suffix Array"
parent: Topics
nav_order: 28
---

## ELI5 (Explain Like I'm 5)

A suffix array is a sorted list of all suffixes of a string. For "banana", the suffixes are "banana", "anana", "nana", "ana", "na", "a" — sort these alphabetically and you get the suffix array. It's a compact structure that lets you answer "does substring X appear in this string?" incredibly fast using binary search.

## Explanation

- A **suffix array** is an array of integers representing the starting indices of all suffixes of a string, sorted in lexicographical order
- Combined with an **LCP (Longest Common Prefix) array**, it can answer most string problems efficiently
- Building: naive O(n² log n), SA-IS algorithm O(n) — in interviews, naive construction is usually acceptable
- Key use: binary search on the suffix array to find pattern occurrences in O(m log n)

> Keyword trigger: _"longest repeated substring"_, _"number of distinct substrings"_, _"longest common substring of two strings"_ → Suffix Array.

## When to use?

- Finding all occurrences of a pattern in a text (alternative to KMP / Z-algorithm)
- Longest repeated substring
- Number of distinct substrings
- Longest common substring between two or more strings
- Competitive programming — rare in general LeetCode, but appears in string-heavy hard problems

## Approach

### Construction (O(n log² n) — acceptable for interviews)
```python
def build_suffix_array(s: str) -> list[int]:
    n = len(s)
    # all suffixes as (suffix_string, start_index)
    suffixes = sorted(range(n), key=lambda i: s[i:])
    return suffixes
```

### LCP Array
```python
def build_lcp(s: str, sa: list[int]) -> list[int]:
    n = len(s)
    rank = [0] * n
    for i, suffix_idx in enumerate(sa):
        rank[suffix_idx] = i  # inverse of suffix array

    lcp = [0] * n
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i]] = h
            if h > 0:
                h -= 1
    return lcp
```

### Pattern Search
```python
def search(s: str, sa: list[int], pattern: str) -> int:
    # binary search on the suffix array
    lo, hi = 0, len(sa)
    while lo < hi:
        mid = (lo + hi) // 2
        if s[sa[mid]:sa[mid] + len(pattern)] < pattern:
            lo = mid + 1
        else:
            hi = mid
    return lo  # first occurrence index in suffix array
```

## Notes

- Time Complexity: O(n log² n) for naive SA construction, O(n) for SA-IS
- Space Complexity: O(n)
- In Python, the naive `sorted(range(n), key=lambda i: s[i:])` is O(n² log n) due to string comparisons — acceptable for n ≤ 10⁴
- For practical competitive programming in Python, use the O(n log² n) doubling algorithm
- The **Z-algorithm** or **KMP** are simpler alternatives for pattern matching — use suffix arrays when you need multiple queries or more complex string operations

## Data structures

- **Suffix array** — sorted indices of suffixes
- **LCP array** — `lcp[i]` = longest common prefix between `sa[i]` and `sa[i-1]`
- **Rank array** — inverse of suffix array, `rank[i]` = position of suffix starting at i in sorted order

## How to Master This

### Step-by-step
1. Understand what a suffix is — enumerate all suffixes of "banana" manually
2. Implement naive suffix array construction and verify on small inputs
3. Build the LCP array and understand what it tells you
4. Solve #1044 (longest duplicate substring) — suffix array + LCP is the intended O(n log n) solution
5. Solve #1062 (longest repeating substring)

### Key Resources
- 📹 [William Fiset — Suffix Array](https://www.youtube.com/watch?v=zqKlL3ZpTqs)
- 📝 [CP-Algorithms — Suffix Array](https://cp-algorithms.com/string/suffix-array.html)
- 📝 [Stanford ICPC — Suffix Array](https://web.stanford.edu/class/cs97si/suffix-array.pdf)
- 🔁 Drill: [#1044](https://leetcode.com/problems/longest-duplicate-substring) → [#1062](https://leetcode.com/problems/longest-repeating-substring)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 1044 | [Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring) | Hard | Low | 📖 |
| 1062 | [Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring) | Medium | Low | 📖 |
| 718 | [Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray) | Medium | Medium | ⚡ |

## Code Samples

1. Longest Duplicate Substring — suffix array + LCP

```python
def longestDupSubstring(s: str) -> str:
    n = len(s)

    # build suffix array (naive O(n^2 log n) — acceptable here)
    sa = sorted(range(n), key=lambda i: s[i:])

    # build LCP array using Kasai's algorithm
    rank = [0] * n
    for i, idx in enumerate(sa):
        rank[idx] = i

    lcp = [0] * n
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i]] = h
            if h > 0:
                h -= 1

    # find maximum LCP and the corresponding substring
    max_len = max(lcp)
    if max_len == 0:
        return ''

    # find which suffix has this LCP length
    for i, length in enumerate(lcp):
        if length == max_len:
            return s[sa[i]: sa[i] + max_len]

    return ''
```

2. Count Distinct Substrings — using suffix array + LCP

```python
def countDistinctSubstrings(s: str) -> int:
    """
    Total substrings = n*(n+1)/2
    Subtract sum of LCP array (shared prefixes = duplicates)
    """
    n = len(s)
    sa = sorted(range(n), key=lambda i: s[i:])

    # Kasai's LCP
    rank = [0] * n
    for i, idx in enumerate(sa):
        rank[idx] = i

    lcp = [0] * n
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i]] = h
            if h > 0:
                h -= 1

    total_substrings = n * (n + 1) // 2
    return total_substrings - sum(lcp)
```
