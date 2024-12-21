# LeetCode Problem 36: Valid Sudoku

### Problem Statement
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the rules:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 3x3 sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**
- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated.

### Inputs
- `board`: A 2D list of strings representing the Sudoku board.

### Outputs
- `True` if the Sudoku board is valid; otherwise, `False`.

### Constraints
- The board is always a 9x9 grid.
- Each cell contains `.` or digits `1-9`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

#### Key Points:
- Validate the board without solving it.
- Ensure no digit repeats in rows, columns, or 3x3 sub-boxes.

#### Questions:
- How should empty cells (`.`) be handled? 
  - Skip them; only validate filled cells.

---

### 2. Match

#### Pattern:
- This problem involves checking constraints for rows, columns, and sub-boxes. We can solve it using:
  - **Set-based validation**: Use sets to track seen numbers for rows, columns, and sub-boxes.
  - **Bitmask validation**: Use integers to represent bitmaps for rows, columns, and sub-boxes for a more space-efficient solution.

---

### 3. Plan

#### Approach 1: Set-based Validation
1. Initialize three dictionaries:
   - `rows`: A dictionary mapping row indices to sets of seen numbers.
   - `cols`: A dictionary mapping column indices to sets of seen numbers.
   - `boxes`: A dictionary mapping sub-box indices to sets of seen numbers.
2. Iterate through each cell in the board.
   - If the cell is empty (`.`), skip it.
   - Calculate the sub-box index using `(r // 3) * 3 + (c // 3)`.
   - Check if the number is already in `rows[r]`, `cols[c]`, or `boxes[box_index]`. If so, return `False`.
   - Otherwise, add the number to the respective sets.
3. If no conflicts are found, return `True`.

#### Approach 2: Bitmask Validation
1. Use three arrays of integers (`rows`, `cols`, `boxes`) to store bitmaps for seen numbers in rows, columns, and sub-boxes.
2. Iterate through each cell in the board.
   - If the cell is empty (`.`), skip it.
   - Calculate the sub-box index using `(r // 3) * 3 + (c // 3)`.
   - Create a bitmask for the current number using `1 << (int(num) - 1)`.
   - Check if the bit is already set in `rows[r]`, `cols[c]`, or `boxes[box_index]`. If so, return `False`.
   - Otherwise, update the bitmaps with the current number.
3. If no conflicts are found, return `True`.

---

### 4. Implement

#### Solution 1: Set-based Validation
```python
from collections import defaultdict

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rows = defaultdict(set)
        cols = defaultdict(set)
        boxes = defaultdict(set)

        for r in range(9):
            for c in range(9):
                num = board[r][c]
                if num == ".":
                    continue

                box_index = (r // 3) * 3 + (c // 3)

                if (num in rows[r] or
                    num in cols[c] or
                    num in boxes[box_index]):
                    return False

                rows[r].add(num)
                cols[c].add(num)
                boxes[box_index].add(num)

        return True
```

#### Solution 2: Bitmask Validation
```python
def isValidSudoku(board):
    rows = [0] * 9
    cols = [0] * 9
    boxes = [0] * 9

    for r in range(9):
        for c in range(9):
            num = board[r][c]
            if num == '.':
                continue

            val = 1 << (int(num) - 1)
            box_index = (r // 3) * 3 + (c // 3)

            if (rows[r] & val) or (cols[c] & val) or (boxes[box_index] & val):
                return False

            rows[r] |= val
            cols[c] |= val
            boxes[box_index] |= val

    return True
```

---

### 5. Review

#### Edge Cases:
1. **Empty Board:** Only `.` should be present.
2. **Single Row or Column Filled:** Ensure no repetition in the filled row or column.
3. **Partially Filled Sub-box:** Check for repetitions within the sub-box.

#### Dry Run:
Input:
```python
board = [
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```
Dry-run step-by-step:
1. Process cell `(0,0)` with value `5`.
   - Add `5` to `rows[0]`, `cols[0]`, `boxes[0]`.
2. Process cell `(0,1)` with value `3`.
   - Add `3` to `rows[0]`, `cols[1]`, `boxes[0]`.
3. Continue processing... (validate no repetition in rows, columns, and boxes).

---

### 6. Evaluate

#### Time Complexity
1. **Set-based Validation:**
   - Outer loop iterates over 81 cells.
   - Each set operation is O(1).
   - Overall: O(81) = O(1).

2. **Bitmask Validation:**
   - Similar to set-based, operations are O(1).
   - Overall: O(81) = O(1).

#### Space Complexity
1. **Set-based Validation:** O(9) for rows, columns, and boxes.
2. **Bitmask Validation:** O(1) for rows, columns, and boxes.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates proficiency in using hash maps, sets, or bit manipulation.
- Tests understanding of constraints and efficient checking of multiple conditions.

#### Prerequisites for Practicing This Problem
- Familiarity with hash maps and sets in Python.
- Understanding of bit manipulation basics.

#### Industry Relevance
- Similar validations are required in data integrity checks and rule-based validations in real-world applications.

#### Follow-up Practice Problems
1. LeetCode 37: Sudoku Solver.
2. LeetCode 48: Rotate Image.
3. LeetCode 128: Longest Consecutive Sequence.
