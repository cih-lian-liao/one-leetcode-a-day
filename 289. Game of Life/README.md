
# LeetCode 289: Game of Life

### Problem Statement

The **Game of Life** is a cellular automaton where cells in a 2D grid evolve over time based on specific rules. Given an `m x n` grid, where:
- `1` represents a live cell.
- `0` represents a dead cell.

The state of each cell is updated simultaneously using these rules:
1. Any live cell with fewer than two live neighbors dies (underpopulation).
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies (overpopulation).
4. Any dead cell with exactly three live neighbors becomes a live cell (reproduction).

Constraints:
- Update the grid **in-place** if possible.
- Inputs will always be valid integers (0 or 1).

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input**: 
  A 2D grid of size `m x n`, where each cell is either `0` (dead) or `1` (alive).

- **Output**: 
  The grid updated according to the rules mentioned above.

- **Constraints**:
  - Update the grid **in-place** if possible.
  - Grid boundaries should not cause errors when calculating neighbors.
  - The update must occur simultaneously for all cells.

- **Assumptions**:
  - The grid can have any dimensions but is always rectangular.

---

### 2. Match

- **Pattern**:
  This problem is a simulation problem, often involving grid traversal.

- **Relevant Data Structures**:
  - A 2D array for the grid.
  - Direction vectors to navigate neighbors.

- **Algorithm**:
  Simulate the transitions using:
  - Directional vectors for neighbors.
  - Encoding state changes temporarily (`-1` for live → dead, `2` for dead → live).

---

### 3. Plan

1. Define directional vectors for 8 possible neighbors (top, bottom, left, right, and diagonals).
2. Iterate over each cell in the grid and count live neighbors:
   - Use the absolute value of cells to account for temporary states.
3. Apply rules:
   - If a live cell (`1`) has fewer than 2 or more than 3 live neighbors, mark it as dead (`-1`).
   - If a dead cell (`0`) has exactly 3 live neighbors, mark it as alive (`2`).
4. Traverse the grid again to update cells to their final states:
   - `-1` → `0`
   - `2` → `1`

**Edge Cases**:
- Small grids (e.g., `1x1`, `2x2`).
- Grids with all cells dead or alive.
- Cells on the edges or corners of the grid.

**Time Complexity**: O(m * n)  
**Space Complexity**: O(1) (in-place modification)

---

### 4. Implement

```python
def gameOfLife(board):
    """
    Update the grid in-place to the next state of the Game of Life.
    :param board: List[List[int]] (m x n grid)
    """
    # Define directions for neighbors (8 possible directions)
    directions = [(-1, -1), (-1, 0), (-1, 1),
                  (0, -1),         (0, 1),
                  (1, -1), (1, 0), (1, 1)]
    
    rows, cols = len(board), len(board[0])
    
    # Traverse each cell in the grid
    for r in range(rows):
        for c in range(cols):
            # Count live neighbors
            live_neighbors = 0
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and abs(board[nr][nc]) == 1:
                    live_neighbors += 1
            
            # Apply rules
            if board[r][c] == 1 and (live_neighbors < 2 or live_neighbors > 3):
                board[r][c] = -1  # Live → Dead
            if board[r][c] == 0 and live_neighbors == 3:
                board[r][c] = 2   # Dead → Live
    
    # Update to final state
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == -1:
                board[r][c] = 0  # Dead
            if board[r][c] == 2:
                board[r][c] = 1  # Alive
```

**Implementation Notes**:
1. **Directional Vectors**: Predefine neighbor offsets to simplify traversal.
2. **Temporary States**: Use `-1` and `2` to encode state changes without affecting neighbor counts.
3. **Final Update**: Traverse the grid a second time to apply final changes.

---

### 5. Review

#### Dry Run Example

**Input**:
```plaintext
[
 [0, 1, 0],
 [0, 0, 1],
 [1, 1, 1],
 [0, 0, 0]
]
```

**Step-by-Step Execution**:
1. Count live neighbors for each cell and apply rules:
   - First row: `[0, -1, 0]` (cell at (0,1) becomes dead).
   - Second row: `[2, 0, -1]` (cell at (1,0) becomes live; (1,2) becomes dead).
   - Third row: `[-1, 1, 1]` (cell at (2,0) becomes dead).
   - Fourth row: `[0, 1, 0]` (cell at (3,1) becomes live).

2. Update final states:
   - First row: `[0, 0, 0]`.
   - Second row: `[1, 0, 1]`.
   - Third row: `[0, 1, 1]`.
   - Fourth row: `[0, 1, 0]`.

**Output**:
```plaintext
[
 [0, 0, 0],
 [1, 0, 1],
 [0, 1, 1],
 [0, 1, 0]
]
```

---

### 6. Evaluate

- **Time Complexity**: O(m * n)  
  Each cell is visited twice: once for neighbor calculation and once for final state update.

- **Space Complexity**: O(1)  
  The grid is updated in-place without extra data structures.

---

### Additional Notes

#### Why This Problem Is Important
- Teaches simulation and grid-based problem-solving.
- Demonstrates techniques for in-place updates with temporary states.

#### Prerequisites for Practicing This Problem
- Familiarity with 2D arrays and nested loops.
- Understanding of directional vectors for grid traversal.

#### Industry Relevance
- Useful in simulations, automata theory, and modeling systems with local interactions.

#### Follow-up Practice Problems
1. **LeetCode 73: Set Matrix Zeroes**
2. **LeetCode 130: Surrounded Regions**
3. **LeetCode 48: Rotate Image**
