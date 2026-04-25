---
title: "Topological Sort"
parent: Topics
nav_order: 24
---

## ELI5 (Explain Like I'm 5)

You need to get dressed in the morning. You can't put on shoes before socks, or a jacket before a shirt. There's an ordering you must follow. Topological sort figures out a valid order to complete all tasks when some tasks must come before others — and if it's impossible (circular dependency), it tells you that too.

## Explanation

- Topological sort orders the nodes of a **Directed Acyclic Graph (DAG)** such that every directed edge `u → v` has `u` before `v`
- Only possible if the graph has **no cycles**
- Two approaches: **Kahn's algorithm** (BFS, uses in-degree) and **DFS-based** (reverse postorder)
- Kahn's is more intuitive for detecting cycles simultaneously

> Keyword trigger: _"prerequisites"_, _"task ordering"_, _"dependency resolution"_, _"build order"_, _"course schedule"_ → topological sort.

## When to use?

- Task scheduling with dependencies
- Build systems, package dependency resolution
- Course prerequisite ordering
- Any problem that asks for a valid ordering of nodes with directed edges

## Approach

### Kahn's Algorithm (BFS / In-degree)
```
1. Compute in-degree of every node (count of incoming edges)
2. Add all nodes with in-degree 0 to a queue
3. While queue is not empty:
   a. Pop a node, add to result
   b. For each neighbor: decrement in-degree
   c. If neighbor's in-degree becomes 0, add to queue
4. If result has all nodes → valid topological order
   If result is shorter → cycle exists
```

### DFS-based
```
1. DFS from every unvisited node
2. After fully exploring a node, push it to a stack
3. Pop the stack at the end → reverse postorder = topological order
```

## Notes

- Time Complexity: O(V + E)
- Space Complexity: O(V + E)
- Kahn's naturally detects cycles: if `len(result) < n`, a cycle exists
- For DFS approach, use a 3-state visited array (unvisited / in-progress / done) to detect cycles
- The result is not unique — there may be multiple valid orderings

## Data structures

- **In-degree array** — how many prerequisites each node has (Kahn's)
- **Queue / deque** — BFS frontier for Kahn's
- **Adjacency list** — graph representation
- **Stack** — collect postorder in DFS-based approach

## How to Master This

### Step-by-step
1. Implement Kahn's algorithm from scratch — it's BFS with in-degree tracking
2. Solve #207 (course schedule) — detect if topological order exists
3. Solve #210 (course schedule II) — return the actual ordering
4. Solve #269 (alien dictionary) — build graph from string comparisons, then topological sort
5. Solve #329 (longest increasing path in a matrix) — topological sort on a DAG

### Key Resources
- 📹 [NeetCode — Course Schedule II](https://www.youtube.com/watch?v=Akt3glAwyfY)
- 📹 [NeetCode — Alien Dictionary](https://www.youtube.com/watch?v=6kTZYvNNyps)
- 📝 [NeetCode.io — Advanced Graphs](https://neetcode.io/roadmap)
- 🔁 Drill: [#207](https://leetcode.com/problems/course-schedule) → [#210](https://leetcode.com/problems/course-schedule-ii) → [#269](https://leetcode.com/problems/alien-dictionary) → [#329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix)

## Sample LeetCode Problems

| # | Problem | Difficulty | Interview Frequency | Must-Do |
|---|---------|------------|---------------------|---------|
| 207 | [Course Schedule](https://leetcode.com/problems/course-schedule) | Medium | Very High | ✅ |
| 210 | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii) | Medium | Very High | ✅ |
| 269 | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary) | Hard | High | ✅ |
| 329 | [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix) | Hard | Medium | ⚡ |
| 444 | [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction) | Medium | Low | 📖 |

## Code Samples

1. Course Schedule II — Kahn's algorithm (return ordering)

```python
from collections import defaultdict, deque

def findOrder(numCourses: int, prerequisites: list[list[int]]) -> list[int]:
    graph = defaultdict(list)
    in_degree = [0] * numCourses

    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1

    # start with all courses that have no prerequisites
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    order = []

    while queue:
        course = queue.popleft()
        order.append(course)

        for next_course in graph[course]:
            in_degree[next_course] -= 1
            if in_degree[next_course] == 0:
                queue.append(next_course)

    # if we couldn't order all courses, there's a cycle
    return order if len(order) == numCourses else []
```

2. Alien Dictionary — build graph from string comparisons

```python
from collections import defaultdict, deque

def alienOrder(words: list[str]) -> str:
    # every unique character is a node
    adj = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}

    # compare adjacent words to find ordering constraints
    for i in range(len(words) - 1):
        w1, w2 = words[i], words[i + 1]
        min_len = min(len(w1), len(w2))

        # if w2 is a prefix of w1 (longer word first) → invalid
        if len(w1) > len(w2) and w1[:min_len] == w2[:min_len]:
            return ''

        for j in range(min_len):
            if w1[j] != w2[j]:
                if w2[j] not in adj[w1[j]]:
                    adj[w1[j]].add(w2[j])
                    in_degree[w2[j]] += 1
                break

    # Kahn's topological sort
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    result = []

    while queue:
        c = queue.popleft()
        result.append(c)
        for neighbor in adj[c]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return ''.join(result) if len(result) == len(in_degree) else ''
```
