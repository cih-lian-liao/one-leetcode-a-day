
# LeetCode 994: Rotting Oranges

## Problem Description

You are given an `m x n` grid where each cell can have one of three values:

- `0` represents an empty cell.
- `1` represents a fresh orange.
- `2` represents a rotten orange.

Every minute, any rotten orange spreads rot to its adjacent fresh oranges (up, down, left, right). Return the minimum number of minutes that must elapse until no cell has a fresh orange. If it is impossible to rot every orange, return `-1`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs:**
- `grid`: A 2D list of integers (`m x n`) representing the grid.

**Outputs:**
- An integer indicating the minimum number of minutes to rot all oranges or `-1` if impossible.

**Constraints:**
- `1 <= grid.length, grid[0].length <= 10`.
- Grid values are `0`, `1`, or `2`.
- At least one cell contains an orange (`1` or `2`).

**Clarifications:**
- A rotten orange can only affect its immediate neighbors (no diagonal spreading).
- The process stops when no more oranges can rot.

---

### 2. Match

This is a **grid traversal problem**. BFS (Breadth-First Search) is well-suited because:
- BFS naturally processes elements layer by layer, which corresponds to the minutes elapsed as the rot spreads.
- A queue is ideal to track the current state of rotten oranges and propagate changes.

Relevant data structures:
- **Queue**: To store the current rotten oranges and their coordinates.
- **List**: To represent the grid.

---

### 3. Plan

1. **Initialize:**
   - Create a queue for BFS and add all initial rotten oranges (`2`) along with a time counter.
   - Count the number of fresh oranges (`1`).

2. **Process with BFS:**
   - For each rotten orange, spread rot to its fresh neighbors and add them to the queue with the incremented time.
   - Decrease the fresh orange count for each newly rotten orange.

3. **Check Results:**
   - If all fresh oranges are rotten by the end of BFS, return the elapsed time.
   - Otherwise, return `-1`.

**Pseudocode:**
```
1. Initialize a queue and count fresh oranges.
2. Add all rotten oranges (value 2) to the queue with time 0.
3. Perform BFS:
   - For each orange in the queue, spread to neighbors (up, down, left, right).
   - If a fresh orange is found, rot it, reduce fresh count, and add it to the queue.
4. If fresh oranges remain, return -1; otherwise, return time.
```

**Time Complexity:** O(m * n)  
**Space Complexity:** O(m * n)

---

### 4. Implement

```python
from collections import deque

def orangesRotting(grid):
    # Initialize variables
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_count = 0

    # Step 1: Populate initial rotten oranges and count fresh oranges
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:  # Rotten orange
                queue.append((r, c, 0))  # (row, col, time)
            elif grid[r][c] == 1:  # Fresh orange
                fresh_count += 1

    # If no fresh oranges, return 0
    if fresh_count == 0:
        return 0

    # Step 2: Perform BFS to spread rot
    minutes_elapsed = 0
    while queue:
        r, c, minutes_elapsed = queue.popleft()
        for dr, dc in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            nr, nc = r + dr, c + dc
            # If within bounds and fresh orange
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2  # Rot the orange
                fresh_count -= 1
                queue.append((nr, nc, minutes_elapsed + 1))

    # Step 3: Check remaining fresh oranges
    return -1 if fresh_count > 0 else minutes_elapsed
```

---

### 5. Review

**Test Cases:**

**Example 1:**
```python
grid = [
    [2, 1, 1],
    [1, 1, 0],
    [0, 1, 1]
]
print(orangesRotting(grid))  # Output: 4
```

**Example 2:**
```python
grid = [
    [2, 1, 1],
    [0, 1, 1],
    [1, 0, 1]
]
print(orangesRotting(grid))  # Output: -1
```

**Example 3:**
```python
grid = [
    [0, 2]
]
print(orangesRotting(grid))  # Output: 0
```

**Complex Case Dry Run:**

Input:
```python
grid = [
    [2, 1, 0, 2],
    [1, 0, 1, 1],
    [0, 1, 1, 1],
    [2, 0, 1, 0]
]
```

**Dry Run Steps:**
1. Start with rotten oranges: (0,0), (0,3), (3,0).
2. At minute 1: Rot (0,1), (1,0).
3. At minute 2: Rot (1,3), (2,2).
4. At minute 3: Rot (2,1), (2,3).
5. At minute 4: Rot (3,2).

Final Output: `4`

---

### 6. Evaluate

**Time Complexity:**
- Each cell is processed once, making the complexity O(m * n), where `m` is the number of rows and `n` is the number of columns.

**Space Complexity:**
- The queue can hold up to all cells, so the space complexity is O(m * n).

**Optimizations:**
- Early termination if fresh oranges count reaches zero during BFS.
- Avoid redundant checks by marking processed cells immediately.

---

### Additional Notes

#### Why This Problem is Important
- Teaches BFS traversal on a grid.
- Improves problem-solving for real-world scenarios like virus spread or water flow.

#### Prerequisites
- Understanding of BFS and queue operations.
- Familiarity with grid-based problems.

#### Industry Relevance
- Useful in simulations, graph algorithms, and game development.
- Concepts applicable to pathfinding and spread analysis.

#### Follow-Up Practice Problems
- **200. Number of Islands**
- **733. Flood Fill**
- **785. Is Graph Bipartite?**
