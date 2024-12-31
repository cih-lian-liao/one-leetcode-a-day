# LeetCode 542: 01 Matrix

### Problem Statement
Given an `m x n` binary matrix `mat`, return the distance of the nearest `0` for each cell.

The distance between two adjacent cells is `1`.

### Inputs
- `mat`: A list of lists of integers (`List[List[int]]`) where `0 <= mat[i][j] <= 1`.
- Dimensions: `m x n`, where `1 <= m, n <= 10^4`.

### Outputs
- A list of lists of integers (`List[List[int]]`) representing the matrix where each cell contains the distance to the nearest `0`.

### Constraints
1. There is at least one `0` in the matrix.
2. Each cell contains either `0` or `1`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

We need to calculate the shortest distance from every cell to the nearest `0` in the matrix.

#### Key Points:
- If a cell already contains `0`, its distance is `0`.
- Use the Manhattan distance: `|x1 - x2| + |y1 - y2|`.

#### Examples
##### Example 1:
**Input**:
```
mat = [[0,0,0],
       [0,1,0],
       [1,1,1]]
```
**Output**:
```
[[0,0,0],
 [0,1,0],
 [1,2,1]]
```

##### Example 2:
**Input**:
```
mat = [[0,1,1],
       [1,1,0],
       [1,1,1]]
```
**Output**:
```
[[0,1,2],
 [1,1,0],
 [2,2,1]]
```

---

### 2. Match

#### Problem Type:
This is a **multi-source shortest path** problem in an unweighted grid. A breadth-first search (BFS) algorithm is well-suited for such problems.

#### Algorithm:
- Use BFS starting from all `0` cells simultaneously.
- Update distances for each cell as we traverse.

#### Data Structures:
- **Queue**: For BFS traversal.
- **Matrix**: To store distances (`dist` matrix).

---

### 3. Plan

1. Initialize a `dist` matrix with `float('inf')` for all cells except `0` cells, which are set to `0`.
2. Use a queue to enqueue all `0` cells as the BFS starting points.
3. Define the possible directions for traversal: up, down, left, right.
4. Perform BFS:
   - For each cell in the queue, explore its neighbors.
   - If a neighbor's current distance is greater than the current cell's distance + 1, update it and enqueue the neighbor.
5. Return the `dist` matrix.

#### Edge Cases:
- Small matrices (e.g., `1x1`).
- All cells are `0`.
- Only one `0` cell in the matrix.

#### Complexity:
- **Time Complexity**: O(m * n), where `m` and `n` are the dimensions of the matrix.
- **Space Complexity**: O(m * n) for the `dist` matrix and BFS queue.

---

### 4. Implement

```python
from collections import deque
from typing import List

class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        rows, cols = len(mat), len(mat[0])

        # Step 1: Initialize distance matrix and queue
        dist = [[float('inf')] * cols for _ in range(rows)]
        queue = deque()

        # Step 2: Enqueue all 0s and set their distance to 0
        for r in range(rows):
            for c in range(cols):
                if mat[r][c] == 0:
                    dist[r][c] = 0
                    queue.append((r, c))

        # Step 3: Define directions for BFS traversal
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

        # Step 4: Perform BFS
        while queue:
            r, c = queue.popleft()
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols:
                    # If a shorter distance is found, update it
                    if dist[nr][nc] > dist[r][c] + 1:
                        dist[nr][nc] = dist[r][c] + 1
                        queue.append((nr, nc))

        # Step 5: Return the distance matrix
        return dist
```

#### Implementation Notes
1. **Initialization**:
   - `float('inf')` is used to represent an infinite distance for non-zero cells initially.
   - `queue` is initialized with all `0` cells to start the BFS simultaneously from all sources.
2. **BFS Traversal**:
   - Directions (`directions`) represent up, down, left, and right movements.
   - The condition `dist[nr][nc] > dist[r][c] + 1` ensures we only update distances when a shorter path is found.

---

### 5. Review

#### Edge Case Testing

1. **Case 1**: All `0`
   ```
   Input: [[0, 0], [0, 0]]
   Output: [[0, 0], [0, 0]]
   ```

2. **Case 2**: Single `0`
   ```
   Input: [[1, 1], [1, 0]]
   Output: [[2, 1], [1, 0]]
   ```

3. **Case 3**: Small Matrix
   ```
   Input: [[0]]
   Output: [[0]]
   ```

#### Dry Run
**Input**:
```
mat = [[0, 1, 1],
       [1, 1, 0],
       [1, 1, 1]]
```
**Steps**:
1. Initialize:
   ```
   dist = [[0, inf, inf],
           [inf, inf, 0],
           [inf, inf, inf]]
   queue = [(0,0), (1,2)]
   ```
2. BFS (First Layer):
   ```
   dist = [[0, 1, inf],
           [1, inf, 0],
           [inf, inf, inf]]
   queue = [(0,1), (1,0), (2,2)]
   ```
3. BFS (Second Layer):
   ```
   dist = [[0, 1, 2],
           [1, 1, 0],
           [2, 2, 1]]
   queue = []
   ```

---

### 6. Evaluate

#### Time Complexity
- O(m * n): Each cell is processed at most once in the BFS traversal.

#### Space Complexity
- O(m * n): For the `dist` matrix and BFS queue.

---

### Additional Notes

#### Importance of This Problem
- Teaches the **multi-source BFS** technique, applicable in grid-based shortest path problems.

#### Prerequisites
- Understanding of BFS, queues, and matrix traversal.

#### Industry Relevance
- Frequently asked in interviews for roles involving graph and matrix traversal problems.

#### Follow-up Problems
1. LeetCode 994: Rotting Oranges
2. LeetCode 286: Walls and Gates
3. LeetCode 1293: Shortest Path in a Grid with Obstacles Elimination
