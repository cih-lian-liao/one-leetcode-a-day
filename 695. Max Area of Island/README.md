# LeetCode 695: Max Area of Island

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**
#### Problem Statement:
You are given an `m x n` binary matrix `grid` where:
- `grid[i][j] = 1` represents land.
- `grid[i][j] = 0` represents water.

An island is a group of `1`s (land) connected 4-directionally (horizontal or vertical). The area of an island is the number of `1`s in the island.

Return the maximum area of an island in the `grid`. If there is no island, return `0`.

#### Inputs:
- `grid`: A 2D list of integers where `1` represents land and `0` represents water.

#### Outputs:
- An integer representing the maximum area of an island.

#### Constraints:
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

#### Assumptions:
- Each cell is either land or water.
- No diagonally connected land is considered part of the same island.

---

### **2. Match**
#### Problem Pattern:
- This is a **graph traversal problem**.
- The grid can be visualized as a graph where each cell is a node, and 4-directionally connected cells are edges.

#### Techniques:
- Use **Depth-First Search (DFS)** or **Breadth-First Search (BFS)** to explore the island and calculate its area.
- A visited matrix or in-place marking ensures we do not count the same cell twice.

---

### **3. Plan**
#### Steps:
1. Initialize a variable `max_area` to store the maximum area encountered.
2. Loop through each cell in the grid:
   - If the cell is land (`1`), perform a DFS/BFS from that cell to calculate the area of the island.
   - Update `max_area` with the larger value between the current area and `max_area`.
3. Return `max_area`.

#### DFS Implementation Plan:
1. Define a helper function `dfs(r, c)` that:
   - Marks the current cell as visited (set `grid[r][c] = 0`).
   - For each direction (up, down, left, right), recursively call `dfs` if the cell is within bounds and is land.
   - Returns the total area of the island.
2. Traverse the grid and call `dfs` for unvisited land cells, accumulating the area for each island.

#### Time Complexity:
- O(m * n), where `m` and `n` are the dimensions of the grid.

#### Space Complexity:
- O(m * n) in the worst case due to the recursion stack (DFS) or queue (BFS).

---

### **4. Implement**
```python
from typing import List

def maxAreaOfIsland(grid: List[List[int]]) -> int:
    def dfs(r, c):
        # Base case: out of bounds or water
        if r < 0 or r >= len(grid) or c < 0 or c >= len(grid[0]) or grid[r][c] == 0:
            return 0

        # Mark the cell as visited
        grid[r][c] = 0

        # Explore all 4 directions and sum up the area
        return 1 + dfs(r + 1, c) + dfs(r - 1, c) + dfs(r, c + 1) + dfs(r, c - 1)

    max_area = 0

    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == 1:  # Found a piece of land
                max_area = max(max_area, dfs(r, c))

    return max_area
```

#### Implementation Notes:
1. **Grid Updates:** Marking visited cells directly modifies the input grid.
2. **Recursive Function:** `dfs` explores all connected land and sums up the area.
3. **Edge Cases:** Handles grids with no islands or multiple isolated islands.

---

### **5. Review**
#### Debugging:
- Test the code with small grid examples.
- Add print statements in `dfs` to trace the recursive calls and track the area calculation.

#### Dry Run Example:
**Input:**
```plaintext
[[0, 0, 1, 0, 0],
 [1, 1, 1, 0, 0],
 [0, 0, 0, 0, 0]]
```

**Execution:**
1. Start at `grid[0][2]` (first land cell).
2. Perform DFS to explore the connected land.
   - Mark `grid[0][2]`, `grid[1][2]`, and `grid[1][1]` as visited.
3. Calculate the total area (4).
4. Update `max_area` to 4.

**Output:**
```plaintext
4
```

---

### **6. Evaluate**
#### Complexity Analysis:
- **Time Complexity:** O(m * n), as every cell is visited once.
- **Space Complexity:** O(m * n) for the recursion stack in the worst case.

#### Optimizations:
- Use an iterative approach (BFS) to avoid recursion stack overflow for larger grids.

---

### **Additional Notes**
#### The Reason Why This Problem is Important:
- Teaches grid-based graph traversal, which is foundational for problems like flood fill or island counting.

#### Prerequisites for Practicing This Problem:
- Knowledge of DFS/BFS.
- Understanding of grid traversal.

#### Industry Relevance:
- Used in image processing, game development, and map-based applications.

#### Follow-up Practice Problems:
1. [LeetCode 733: Flood Fill](https://leetcode.com/problems/flood-fill/)
2. [LeetCode 463: Island Perimeter](https://leetcode.com/problems/island-perimeter/)
3. [LeetCode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)
