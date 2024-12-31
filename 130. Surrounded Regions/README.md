# LeetCode Problem 130: Surrounded Regions

### Problem Statement

You are given an `m x n` matrix called `board` containing characters `'X'` and `'O'`. Your task is to modify the matrix such that:

- Any `'O'` that is completely surrounded by `'X'` is replaced by `'X'`.
- Any `'O'` that is connected to the boundary (directly or indirectly) should remain unchanged.

### Example

**Input:**
```python
board = [
    ["X", "X", "X", "X"],
    ["X", "O", "O", "X"],
    ["X", "X", "O", "X"],
    ["X", "O", "X", "X"]
]
```

**Output:**
```python
[
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "O", "X", "X"]
]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs:**
  - A 2D matrix `board` of size `m x n` with characters `'X'` and `'O'`.
- **Outputs:**
  - The same matrix with surrounded `'O'`s converted to `'X'`.
- **Constraints:**
  - Matrix size: `1 <= m, n <= 200`.
  - `'O'`s on the edges are not surrounded.

- **Clarifications:**
  - `'O'` is considered surrounded if it is not connected to any edge of the board.

### 2. Match

- **Problem Type:**
  - Graph traversal using DFS or BFS.
- **Relevant Algorithms:**
  - Flood fill algorithm.
  - Depth-first search (DFS).
  - Breadth-first search (BFS).

### 3. Plan

1. **Mark Unsurrounded Regions:**
   - Start from all edge `'O'`s and mark them (and connected regions) as a special character (e.g., `'#'`) using DFS.
2. **Convert Remaining Regions:**
   - Traverse the board:
     - Convert all `'O'`s to `'X'` (surrounded regions).
     - Convert `'#'` back to `'O'` (unmarked regions).
3. **Time Complexity:**
   - O(m * n), where `m` is the number of rows and `n` is the number of columns.
4. **Space Complexity:**
   - O(m * n) in the worst case for recursion stack or BFS queue.

### 4. Implement

#### Code Implementation

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        # Step 1: Handle edge cases
        if not board or not board[0]:
            return 
        
        rows, cols = len(board), len(board[0])

        # Step 2: Helper function for DFS
        def mark_unsurrounded(r, c):
            if r < 0 or r >= rows or c < 0 or c >= cols or board[r][c] != 'O':
                return
            board[r][c] = '#'
            mark_unsurrounded(r + 1, c)
            mark_unsurrounded(r - 1, c)
            mark_unsurrounded(r, c + 1)
            mark_unsurrounded(r, c - 1)

        # Step 3: Mark boundary-connected 'O's
        for i in range(rows):
            mark_unsurrounded(i, 0)
            mark_unsurrounded(i, cols - 1)
        for j in range(cols):
            mark_unsurrounded(0, j)
            mark_unsurrounded(rows - 1, j)

        # Step 4: Convert remaining 'O's to 'X', and '#' back to 'O'
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == '#':
                    board[i][j] = 'O'
```

#### Implementation Notes

1. **Edge Case Handling:**
   - If the matrix is empty, return immediately.
2. **Mark Boundary Regions:**
   - Traverse the edges (first and last rows/columns) and call `mark_unsurrounded` to flood-fill connected `'O'`s.
3. **Final Conversion:**
   - Remaining `'O'`s are surrounded and must be converted to `'X'`.
   - `'#'` represents unsurrounded `'O'`s and must be restored.

---

### 5. Review

#### Dry Run

**Input:**
```python
board = [
    ["X", "X", "X", "X"],
    ["X", "O", "O", "X"],
    ["X", "X", "O", "X"],
    ["X", "O", "X", "X"]
]
```

**Step-by-Step Execution:**

1. **Mark Unsurrounded Regions:**
   - Mark edge `'O'` at (3, 1) and connected `'O'`s:
     ```
     ["X", "X", "X", "X"],
     ["X", "O", "O", "X"],
     ["X", "X", "O", "X"],
     ["X", "#", "X", "X"]
     ```

2. **Convert Remaining Regions:**
   - Replace `'O'` with `'X'`:
     ```
     ["X", "X", "X", "X"],
     ["X", "X", "X", "X"],
     ["X", "X", "X", "X"],
     ["X", "#", "X", "X"]
     ```
   - Restore `'#'` to `'O'`:
     ```
     ["X", "X", "X", "X"],
     ["X", "X", "X", "X"],
     ["X", "X", "X", "X"],
     ["X", "O", "X", "X"]
     ```

**Output:**
```python
[
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "O", "X", "X"]
]
```

---

### 6. Evaluate

#### Time Complexity
- Traversal of the board: O(m * n).

#### Space Complexity
- Recursion stack (DFS): O(m * n) in the worst case.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates flood-fill algorithms, which are useful in solving connected component problems.

#### Prerequisites
- Understanding DFS/BFS traversal.

#### Industry Relevance
- Used in grid-based games, simulations, and image processing tasks.

#### Follow-Up Practice Problems
- LeetCode 200: Number of Islands
- LeetCode 733: Flood Fill
- LeetCode 695: Max Area of Island
