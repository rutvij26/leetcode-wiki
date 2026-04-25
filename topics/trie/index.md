---
title: "Trie"
parent: Topics
nav_order: 19
---

## ELI5 (Explain Like I'm 5)

Imagine a filing cabinet organized by the first letter of each word, then the second letter, and so on. To find "apple", you open drawer 'a', then 'p', then 'p', then 'l', then 'e'. If the drawer doesn't exist, the word isn't there. A Trie is this filing cabinet — a tree where each path from root to a leaf spells out a word.

## Explanation

- A Trie (prefix tree) stores strings by their characters — each node represents one character
- All words sharing a prefix share the same path from root
- Two key operations: **insert** (add a word), **search** (find exact word or prefix)
- Each node has up to 26 children (one per letter) and an `is_end` flag

> Keyword trigger: _"autocomplete"_, _"prefix matching"_, _"word dictionary"_, _"starts with"_ → Trie.

## When to use?

- Prefix search (autocomplete, IP routing)
- Dictionary of words with fast lookup
- "Does any word in the list start with this prefix?"
- Word search on a board with a dictionary

## Approach

### Trie Node
```python
class TrieNode:
    def __init__(self):
        self.children = {}     # char → TrieNode
        self.is_end = False    # marks end of a complete word
```

### Insert
```
Walk character by character.
If child doesn't exist, create it.
Mark is_end = True at the last character.
```

### Search (exact)
```
Walk character by character.
If any child is missing → word doesn't exist.
At the end, return is_end (True only if it's a full word).
```

### StartsWith (prefix)
```
Same as search, but return True as long as all characters exist
(don't check is_end).
```

## Notes

- Time Complexity: O(m) per insert/search where m = word length
- Space Complexity: O(n × m) where n = number of words, m = average length
- A dict of children is flexible; a fixed array `[None] * 26` is faster for lowercase-only input
- Tries are memory-heavy — each node can hold 26 pointers

## Data structures

- **TrieNode** — contains a children map and an `is_end` flag
- **Dict** — `children` maps character → TrieNode (flexible, handles any alphabet)

## How to Master This

### Step-by-step
1. Implement the `Trie` class from scratch — insert, search, startsWith
2. Solve #208 (implement Trie) — the template itself as a problem
3. Solve #211 (design add/search words) — adds wildcard `.` matching
4. Solve #212 (word search II) — Trie + DFS on a board, the hardest Trie problem

### Key Resources
- 📹 [NeetCode — Implement Trie](https://www.youtube.com/watch?v=oobqoCJlHA0)
- 📹 [NeetCode — Word Search II](https://www.youtube.com/watch?v=asbcE9mZz_U)
- 📝 [NeetCode.io — Tries section](https://neetcode.io/roadmap)
- 🔁 Drill: [#208](https://leetcode.com/problems/implement-trie-prefix-tree) → [#211](https://leetcode.com/problems/design-add-and-search-words-data-structure) → [#212](https://leetcode.com/problems/word-search-ii)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 208 | [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree) | Medium | High | ✅ |
| 211 | [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure) | Medium | High | ✅ |
| 212 | [Word Search II](https://leetcode.com/problems/word-search-ii) | Hard | Medium | ⚡ |
| 648 | [Replace Words](https://leetcode.com/problems/replace-words) | Medium | Low | 📖 |
| 676 | [Implement Magic Dictionary](https://leetcode.com/problems/implement-magic-dictionary) | Medium | Low | 📖 |

## Code Samples

1. Implement Trie — full implementation

```python
class TrieNode:
    def __init__(self):
        self.children: dict[str, 'TrieNode'] = {}
        self.is_end = False


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end  # must be end of a complete word

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True  # prefix exists — don't need is_end
```

2. Word Search II — Trie + backtracking DFS on grid

```python
def findWords(board: list[list[str]], words: list[str]) -> list[str]:
    # build trie from word list
    root = TrieNode()
    for word in words:
        node = root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = word  # store the word itself at the end node

    rows, cols = len(board), len(board[0])
    results = set()

    def dfs(r, c, node, path):
        char = board[r][c]
        if char not in node.children:
            return

        next_node = node.children[char]
        path += char

        if next_node.is_end:
            results.add(next_node.is_end)  # found a complete word

        board[r][c] = '#'  # mark visited
        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and board[nr][nc] != '#':
                dfs(nr, nc, next_node, path)
        board[r][c] = char  # restore

    for r in range(rows):
        for c in range(cols):
            dfs(r, c, root, '')

    return list(results)
```
