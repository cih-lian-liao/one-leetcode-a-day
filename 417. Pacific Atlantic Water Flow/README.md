# LeetCode Problem 417: Pacific Atlantic Water Flow

### Problem Statement

Given an `m x n` matrix of integers `heights` representing the height of terrain, find all grid coordinates where water can flow to both the **Pacific Ocean** and the **Atlantic Ocean**.

- Water can only flow in four directions: up, down, left, or right.
- Water can flow from a cell to another if the height of the destination cell is less than or equal to the height of the current cell.
- The Pacific Ocean is on the top and left edges of the matrix.
- The Atlantic Ocean is on the bottom and right edges of the matrix.

### Example Input
```plaintext
heights = [
  [1, 2, 2, 3, 5],
  [3, 2, 3, 4, 4],
  [2, 4, 5, 3, 1],
  [6, 7, 1, 4, 5],
  [5, 1, 1, 2, 4]
]
```

### Example Output
```plaintext
[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**: A 2D matrix `heights` of size `m x n`.
  - Each cell `heights[i][j]` is the height of the terrain at coordinate `(i, j)`.
- **Outputs**: A list of coordinates `[i, j]` where water can flow to both the Pacific and Atlantic Oceans.
- **Constraints**:
  - `1 <= m, n <= 200`
  - `0 <= heights[i][j] <= 10^9`

#### Clarifications
- Water flows from high terrain to low terrain or to equal height.
- Consider edge cells as starting points for each ocean.

---

### 2. Match

- **Pattern**:
  - This is a graph traversal problem.
  - Each cell in the matrix can be seen as a node, and the possible directions (up, down, left, right) represent edges.
- **Techniques**:
  - Use **DFS** or **BFS** to explore all reachable nodes from the boundary cells for both oceans.
  - The solution involves finding the intersection of reachable nodes for the Pacific and Atlantic Oceans.

---

### 3. Plan

#### DFS Solution
1. Define a helper function `dfs` that traverses the matrix recursively from a starting cell.
2. Start DFS from all Pacific boundary cells and mark reachable cells in a `pacific` set.
3. Start DFS from all Atlantic boundary cells and mark reachable cells in an `atlantic` set.
4. Collect all cells that are present in both `pacific` and `atlantic` sets as the final result.
5. Return the result.

#### BFS Solution
1. Define a helper function `bfs` that traverses the matrix using a queue.
2. Initialize queues for the Pacific and Atlantic boundaries.
3. Perform BFS for each boundary and mark reachable cells in respective `pacific` and `atlantic` sets.
4. Collect all cells that are present in both sets.
5. Return the result.

---

### 4. Implement

#### DFS Solution
```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        # Get the number of rows and columns in the matrix
        ROWS, COLS = len(heights), len(heights[0])
        # Sets to store cells reachable by Pacific and Atlantic oceans
        pacific, atlantic = set(), set()

        # Helper function for DFS traversal
        def dfs(row, col, visited, prev_height):
            # Check if the cell is out of bounds, already visited, or lower than the previous height
            if ((row, col) in visited or 
                not (0 <= row < ROWS and 0 <= col < COLS) or 
                heights[row][col] < prev_height):
                return
            # Mark the cell as visited
            visited.add((row, col))
            # Define possible directions to move: down, up, right, left
            directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
            # Perform DFS for all valid neighboring cells
            for dr, dc in directions:
                dfs(row + dr, col + dc, visited, heights[row][col])

        # Start DFS from all Pacific ocean boundary cells (top row and left column)
        for col in range(COLS):
            dfs(0, col, pacific, heights[0][col])  # Top row
            dfs(ROWS - 1, col, atlantic, heights[ROWS - 1][col])  # Bottom row

        for row in range(ROWS):
            dfs(row, 0, pacific, heights[row][0])  # Left column
            dfs(row, COLS - 1, atlantic, heights[row][COLS - 1])  # Right column

        # Find the intersection of cells reachable by both oceans
        return list(map(list, pacific & atlantic))
```

#### BFS Solution
```python
from collections import deque

class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        # Get the number of rows and columns in the matrix
        ROWS, COLS = len(heights), len(heights[0])

        # Helper function for BFS traversal
        def bfs(starts):
            visited = set()  # Set to keep track of visited cells
            queue = deque(starts)  # Initialize a queue with starting points
            while queue:
                r, c = queue.popleft()  # Dequeue the front element
                if (r, c) in visited:
                    continue  # Skip already visited cells
                visited.add((r, c))  # Mark the current cell as visited
                # Iterate through all possible directions: down, up, right, left
                for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                    nr, nc = r + dr, c + dc  # Calculate the new row and column
                    # Check if the neighboring cell is within bounds, not visited, and valid for water flow
                    if (0 <= nr < ROWS and 0 <= nc < COLS and
                        (nr, nc) not in visited and
                        heights[nr][nc] >= heights[r][c]):
                        queue.append((nr, nc))  # Add the cell to the queue
            return visited  # Return all reachable cells

        # Initialize the starting points for Pacific and Atlantic oceans
        pacific_starts = [(0, c) for c in range(COLS)] + [(r, 0) for r in range(ROWS)]  # Top row and left column
        atlantic_starts = [(ROWS - 1, c) for c in range(COLS)] + [(r, COLS - 1) for r in range(ROWS)]  # Bottom row and right column

        # Perform BFS for both oceans
        pacific_reachable = bfs(pacific_starts)  # Cells reachable by the Pacific Ocean
        atlantic_reachable = bfs(atlantic_starts)  # Cells reachable by the Atlantic Ocean

        # Find the intersection of cells reachable by both oceans
        return list(map(list, pacific_reachable & atlantic_reachable))
```

---

### 5. Review

#### Dry Run for DFS
Input:
```plaintext
heights = [
  [1, 2, 2, 3, 5],
  [3, 2, 3, 4, 4],
  [2, 4, 5, 3, 1],
  [6, 7, 1, 4, 5],
  [5, 1, 1, 2, 4]
]
```

1. Start DFS from the Pacific boundary.
2. Mark reachable cells in `pacific` set.
3. Start DFS from the Atlantic boundary.
4. Mark reachable cells in `atlantic` set.
5. Compute the intersection of `pacific` and `atlantic` sets.
6. Output: `[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]]`

#### Dry Run for BFS
1. Start BFS from Pacific boundary nodes.
2. Traverse reachable nodes and add them to `pacific` set.
3. Start BFS from Atlantic boundary nodes.
4. Traverse reachable nodes and add them to `atlantic` set.
5. Compute the intersection of `pacific` and `atlantic` sets.
6. Output: `[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]]`

---

### 6. Evaluate

#### Time Complexity
- DFS: O(ROWS * COLS)
- BFS: O(ROWS * COLS)

#### Space Complexity
- DFS: O(ROWS * COLS) for visited sets and recursion stack.
- BFS: O(ROWS * COLS) for visited sets and queue.

---

### Additional Notes

#### Why This Problem is Important
- Highlights graph traversal techniques like DFS and BFS.
- Demonstrates practical use cases of intersections of sets.

#### Prerequisites
- Familiarity with DFS and BFS.
- Understanding of set operations and matrix traversal.

#### Industry Relevance
- Common in grid-based simulations and geographical data processing.

#### Follow-Up Practice Problems
- LeetCode 200: Number of Islands
- LeetCode 695: Max Area of Island
- LeetCode 286: Walls and Gates
