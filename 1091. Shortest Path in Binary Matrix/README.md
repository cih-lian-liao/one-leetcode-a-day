

# LeetCode 1091: Shortest Path in Binary Matrix

### Problem Description

Given an `n x n` binary matrix `grid`, return the length of the shortest clear path in the matrix. If there is no clear path, return `-1`.

The clear path in the matrix is a path:
1. Starting at the top-left cell `(0, 0)` and ending at the bottom-right cell `(n-1, n-1)`.
2. Moving only in the 8 possible directions (horizontally, vertically, and diagonally).
3. Traversing only cells with value `0`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Input**:  
- A 2D binary grid `grid` of size `n x n`.

**Output**:  
- Integer: the shortest path length or `-1` if no path exists.

**Constraints**:  
- The grid is square with size `1 <= n <= 100`.
- Each cell contains either `0` (passable) or `1` (blocked).

---

### 2. Match

This is a **shortest path in an unweighted grid** problem.  
**Approach**: Use Breadth-First Search (BFS) to explore all possible paths from the start to the end and return the shortest path length.

---

### 3. Plan

1. **Initial Checks**:  
   - If the start or end cell is blocked (`grid[0][0] == 1` or `grid[n-1][n-1] == 1`), return `-1` immediately.

2. **BFS Setup**:  
   - Use a queue to track positions and their path lengths.  
   - Mark cells as visited to avoid revisiting.

3. **Process BFS**:  
   - Dequeue a cell, check if it is the destination, and explore its neighbors.  
   - If a neighbor is valid (within bounds, passable, and not visited), add it to the queue.

4. **End BFS**:  
   - If the queue is empty and the destination is not reached, return `-1`.

---

### 4. Implement

#### Solution 1: Using `visited` Set
```python
from collections import deque
from typing import List

class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        n = len(grid)
        
        # Initial check: if start or end is blocked
        if grid[0][0] == 1 or grid[n-1][n-1] == 1:
            return -1
        
        # Directions for 8 possible moves
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0), 
                      (1, 1), (-1, -1), (1, -1), (-1, 1)]
        
        # Initialize BFS queue and visited set
        queue = deque([(0, 0, 1)])  # (row, col, path_length)
        visited = set((0, 0))
        
        # Process BFS
        while queue:
            row, col, path_length = queue.popleft()
            
            # If reached the bottom-right corner, return the path length
            if row == n - 1 and col == n - 1:
                return path_length
            
            # Explore neighbors
            for dr, dc in directions:
                new_row, new_col = row + dr, col + dc
                if (0 <= new_row < n and 0 <= new_col < n and 
                    grid[new_row][new_col] == 0 and 
                    (new_row, new_col) not in visited):
                    queue.append((new_row, new_col, path_length + 1))
                    visited.add((new_row, new_col))
        
        # If no path is found
        return -1
```

---

#### Solution 2: Using the Grid to Track Visited Cells
```python
from collections import deque

def shortestPathBinaryMatrix(grid):
    n = len(grid)
    
    # Initial check: if start or end is blocked
    if grid[0][0] != 0 or grid[n-1][n-1] != 0:
        return -1

    # Directions for 8 possible moves
    directions = [(-1, -1), (-1, 0), (-1, 1), (0, -1), 
                  (0, 1), (1, -1), (1, 0), (1, 1)]
    
    # Initialize BFS queue
    queue = deque([(0, 0, 1)])  # (row, col, path_length)
    grid[0][0] = 1  # Mark as visited

    # Process BFS
    while queue:
        row, col, path_length = queue.popleft()
        
        # If reached the bottom-right corner, return the path length
        if row == n - 1 and col == n - 1:
            return path_length
        
        # Explore neighbors
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            if 0 <= new_row < n and 0 <= new_col < n and grid[new_row][new_col] == 0:
                queue.append((new_row, new_col, path_length + 1))
                grid[new_row][new_col] = 1  # Mark as visited
    
    # If no path is found
    return -1
```

---

### 5. Review

#### Example Input:
```python
grid = [[0, 1, 0],
        [1, 0, 0],
        [1, 1, 0]]
```

**Dry Run**:  
1. Start BFS with `queue = deque([(0, 0, 1)])`.
2. Explore `(0, 0)`; add `(1, 1)` to the queue.
3. Explore `(1, 1)`; add `(2, 2)` to the queue.
4. Explore `(2, 2)`; reach the destination.  
**Output**: `3`.

---

### 6. Evaluate

**Time Complexity**:  
O(n^2). Each cell is visited at most once, and the grid size is n^2.

**Space Complexity**:  
O(n^2). The queue and visited set (or grid modification) require additional memory proportional to the grid size.

---

### Additional Notes

**Why This Problem is Important**:  
- Teaches BFS for shortest path problems.
- Helps understand grid traversal and managing visited states.

**Prerequisites**:  
- Basic understanding of BFS.
- Familiarity with grid-based traversal.

**Industry Relevance**:  
- BFS is widely used in navigation systems, game AI, and other applications.

**Follow-up Practice Problems**:
1. LeetCode 994: Rotting Oranges.
2. LeetCode 127: Word Ladder.
3. LeetCode 200: Number of Islands.
