
# LeetCode Problem 463: Island Perimeter

### Problem Statement

Given a `grid` that represents a map of an island, where:
- `1` represents land.
- `0` represents water.

You need to calculate the perimeter of the island. The island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. There is exactly one island.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Problem Details**:
- **Input**: A 2D grid of integers where:
  - `grid[i][j] = 1` means land.
  - `grid[i][j] = 0` means water.
- **Output**: An integer representing the total perimeter of the island.

**Constraints**:
- Grid dimensions: `1 <= rows, cols <= 100`.
- Grid cells are either `0` or `1`.
- There is exactly one island.

**Assumptions**:
- The grid is completely surrounded by water.
- Land cells are connected, forming a single island.

**Examples**:
Input:
```python
grid = [
    [0, 1, 0, 0],
    [1, 1, 1, 0],
    [0, 1, 0, 0],
    [1, 1, 0, 0]
]
```
Output:
```
16
```

---

### 2. Match

**Problem Type**: 
- Grid traversal problem.
- Basic arithmetic based on grid cell relationships.

**Patterns**:
- Iterating over all cells in the grid.
- Adding/subtracting contributions to the perimeter based on adjacency.

**Data Structures**:
- A nested `for` loop for iterating through the grid.

---

### 3. Plan

**Step-by-Step Plan**:
1. Initialize a variable `perimeter = 0` to track the total perimeter.
2. Traverse each cell in the `grid`:
   - If the cell is land (`grid[i][j] == 1`):
     - Add `4` to the perimeter.
     - Check the **right neighbor** (if exists and is land): Subtract `2` from the perimeter.
     - Check the **bottom neighbor** (if exists and is land): Subtract `2` from the perimeter.
3. Return the total `perimeter`.

**Edge Cases**:
- A grid with no land (e.g., `grid = [[0, 0], [0, 0]]`).
- A single-cell grid with land (e.g., `grid = [[1]]`).

**Complexity**:
- **Time Complexity**: O(rows * cols) - Traverse every cell in the grid once.
- **Space Complexity**: O(1) - No extra space used.

---

### 4. Implement

```python
from typing import List

def islandPerimeter(grid: List[List[int]]) -> int:
    """
    Calculate the perimeter of the island in the grid.
    
    Args:
    grid (List[List[int]]): 2D grid representing the island and water.
    
    Returns:
    int: The perimeter of the island.
    """
    rows, cols = len(grid), len(grid[0])  # Grid dimensions
    perimeter = 0  # Initialize perimeter
    
    # Traverse the grid
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:  # Land cell
                perimeter += 4  # Add 4 sides for the land cell
                
                # Check right neighbor
                if c < cols - 1 and grid[r][c + 1] == 1:
                    perimeter -= 2  # Shared side with the right cell
                
                # Check bottom neighbor
                if r < rows - 1 and grid[r + 1][c] == 1:
                    perimeter -= 2  # Shared side with the bottom cell
    
    return perimeter
```

**Implementation Notes**:
- Use nested loops to traverse the grid.
- Carefully check bounds when accessing neighbors.
- Avoid overcomplicating the logic for shared sides.

---

### 5. Review

**Debugging**:
1. Dry run the code with the provided example:
   ```python
   grid = [
       [0, 1, 0, 0],
       [1, 1, 1, 0],
       [0, 1, 0, 0],
       [1, 1, 0, 0]
   ]
   ```
   - Traverse each cell and calculate contributions.
   - Ensure shared sides are handled correctly.

2. Test with edge cases:
   ```python
   grid = [[1]]
   print(islandPerimeter(grid))  # Output: 4

   grid = [[0, 0], [0, 0]]
   print(islandPerimeter(grid))  # Output: 0
   ```

**Explanation of Dry Run**:
For the provided example, iterate over the grid:
- Add `4` for each land cell.
- Subtract `2` for each shared side with adjacent land.

**Output Validation**:
- For input `[[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]`, the output is `16`.

---

### 6. Evaluate

**Time Complexity**:
- O(rows * cols) because we visit each cell exactly once.

**Space Complexity**:
- O(1) because we use a constant amount of extra memory.

**Optimizations**:
- None needed for this problem due to simplicity.

---

### Additional Notes

#### Why This Problem is Important
- Develops skills in grid traversal and boundary condition handling.
- Teaches how to manage adjacency relationships effectively.

#### Prerequisites for Practicing This Problem
- Familiarity with nested loops.
- Basic understanding of 2D grid indexing.

#### Industry Relevance
- Grid traversal problems appear frequently in game development, geographical modeling, and computer vision tasks.

#### Follow-up Practice Problems
1. [Flood Fill](https://leetcode.com/problems/flood-fill/)
2. [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
3. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
