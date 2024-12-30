# LeetCode Problem 200: Number of Islands

### Problem Description
Given a 2D grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Example:
**Input:**
```
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

**Output:**
```
3
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Inputs:
- A 2D grid containing `'1'`s and `'0'`s.

#### Outputs:
- An integer representing the number of islands in the grid.

#### Constraints:
- The grid size is \( m \times n \), where \( 1 \leq m, n \leq 300 \).
- Each cell in the grid is either `'1'` (land) or `'0'` (water).
- An island is a group of connected `'1'`s (connected horizontally or vertically).

#### Ambiguities/Assumptions:
- Diagonal connections do not count as part of the same island.
---
### 2. Match

This problem is a **graph traversal problem** where:
- Each cell in the grid is a node.
- Adjacent cells represent edges.

We can solve it using:
- **Depth-First Search (DFS)**: Recursive exploration of connected nodes.
- **Breadth-First Search (BFS)**: Level-by-level exploration using a queue.
---
### 3. Plan

#### General Steps:
1. Traverse every cell in the grid.
2. When a `'1'` is encountered, it represents the start of a new island.
3. Perform BFS or DFS to mark all connected `'1'`s as visited (convert them to `'0'`).
4. Increment the island count.

#### Edge Cases:
- Empty grid.
- Grid with no `'1'`s (output should be 0).
- Grid with all `'1'`s (output should be 1).

#### Time Complexity:
- \( O(m \times n) \): Each cell is visited once.

#### Space Complexity:
- \( O(m \times n) \): Worst-case stack/queue usage for DFS/BFS.

---

### 4. Implement

#### Solution 1: Depth-First Search (DFS)

```python
from typing import List

def numIslandsDFS(grid: List[List[str]]) -> int:
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    count = 0

    def dfs(r, c):
        if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] == '0':
            return
        grid[r][c] = '0'  # Mark the cell as visited
        dfs(r - 1, c)  # Up
        dfs(r + 1, c)  # Down
        dfs(r, c - 1)  # Left
        dfs(r, c + 1)  # Right

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                dfs(r, c)

    return count
```

#### Notes for DFS Implementation:
1. **Base Case:** If the cell is out of bounds or water (`'0'`), return.
2. **Recursive Exploration:** Visit all connected cells.
3. **Mark as Visited:** Modify the grid in place to avoid revisiting nodes.

#### Solution 2: Breadth-First Search (BFS)

```python
from collections import deque
from typing import List

def numIslandsBFS(grid: List[List[str]]) -> int:
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    count = 0

    def bfs(r, c):
        queue = deque([(r, c)])
        while queue:
            row, col = queue.popleft()
            if row < 0 or col < 0 or row >= rows or col >= cols or grid[row][col] == '0':
                continue
            grid[row][col] = '0'  # Mark the cell as visited
            queue.extend([(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)])

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                bfs(r, c)

    return count
```

#### Notes for BFS Implementation:
1. **Queue Initialization:** Start from the current cell.
2. **Iterative Exploration:** Process all connected nodes level by level.
3. **Mark as Visited:** Modify the grid in place to prevent revisits.

---

### 5. Review

#### Test Cases:
1. **Standard Case:**
   ```python
   grid = [
       ["1","1","0","0","0"],
       ["1","1","0","0","0"],
       ["0","0","1","0","0"],
       ["0","0","0","1","1"]
   ]
   print(numIslandsDFS(grid))  # Output: 3
   print(numIslandsBFS(grid))  # Output: 3
   ```

2. **Edge Case:** Empty grid
   ```python
   grid = []
   print(numIslandsDFS(grid))  # Output: 0
   print(numIslandsBFS(grid))  # Output: 0
   ```

3. **Edge Case:** All water
   ```python
   grid = [
       ["0","0","0"],
       ["0","0","0"],
       ["0","0","0"]
   ]
   print(numIslandsDFS(grid))  # Output: 0
   print(numIslandsBFS(grid))  # Output: 0
   ```

4. **Edge Case:** Single large island
   ```python
   grid = [
       ["1","1"],
       ["1","1"]
   ]
   print(numIslandsDFS(grid))  # Output: 1
   print(numIslandsBFS(grid))  # Output: 1
   ```

---

### 6. Evaluate

#### Time Complexity:
- Both DFS and BFS have \( O(m \times n) \) complexity as every cell is visited once.

#### Space Complexity:
- DFS: \( O(m \times n) \) in the worst case due to recursion.
- BFS: \( O(m \times n) \) in the worst case due to queue storage.

---

### Additional Notes

#### Why This Problem is Important:
- Teaches fundamental graph traversal techniques.
- Commonly used in real-world problems like image processing and geographic analysis.

#### Prerequisites:
- Understanding of DFS and BFS.
- Familiarity with 2D grid traversal.

#### Industry Relevance:
- Similar techniques are used in connected component analysis, flood fill algorithms, and social network graphs.

#### Follow-up Practice Problems:
1. LeetCode 130: Surrounded Regions
2. LeetCode 695: Max Area of Island
3. LeetCode 463: Island Perimeter
