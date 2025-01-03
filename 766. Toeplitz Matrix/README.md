

# LeetCode 766: Toeplitz Matrix

### Problem Statement

A matrix is a Toeplitz matrix if every diagonal from top-left to bottom-right has the same elements. Given an `m x n` matrix, return `true` if it is a Toeplitz matrix; otherwise, return `false`.

#### Example 1:
```plaintext
Input: matrix = [
  [1, 2, 3, 4],
  [5, 1, 2, 3],
  [9, 5, 1, 2]
]
Output: true
```

#### Example 2:
```plaintext
Input: matrix = [
  [1, 2],
  [2, 2]
]
Output: false
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input**: A 2D list `matrix` of size `m x n`.
- **Output**: A boolean indicating if the matrix is a Toeplitz matrix.
- **Constraints**:
  - `matrix.length >= 1` and `matrix[0].length >= 1`.
  - Each element is an integer.
- **Assumptions**:
  - Diagonals must have the same values.
  - If the matrix has only one row or one column, it is automatically a Toeplitz matrix.

---

### 2. Match

- This problem involves **matrix traversal** and **diagonal comparison**.
- Pattern: Iterative checks comparing current elements with their top-left neighbor.
- Data Structures: Nested loops are sufficient. No additional data structures are required.

---

### 3. Plan

1. Iterate through the matrix starting from the second row and second column.
2. For each element, check if it matches the top-left element (`matrix[row-1][col-1]`).
3. If any element does not match, return `false`.
4. If all elements pass the check, return `true`.


#### Pseudocode

```plaintext
1. Get the dimensions of the matrix: rows and cols.
2. For each row from 1 to rows-1:
   a. For each column from 1 to cols-1:
      i. Compare matrix[row][col] with matrix[row-1][col-1].
      ii. If they are not equal, return false.
3. Return true after completing all checks.
```

---

### 4. Implement

```python
from typing import List

class Solution:
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        # Get the dimensions of the matrix
        rows = len(matrix)
        cols = len(matrix[0])
        
        # Traverse the matrix starting from the second row and second column
        for row in range(1, rows):
            for col in range(1, cols):
                # Check if the current element matches the top-left element
                if matrix[row][col] != matrix[row - 1][col - 1]:
                    return False
        
        # Return true if all elements match
        return True
```


#### Implementation Notes

- **Initialization**:
  - `rows` and `cols` are used to control the loop ranges and avoid out-of-bounds errors.
- **Traversing the Matrix**:
  - The range starts from `1` because the first row and column do not need a comparison.
- **Comparison**:
  - `matrix[row][col] != matrix[row-1][col-1]` ensures diagonal consistency.

---

### 5. Review

**Debugging and Testing:**

1. **Simple Case**:
   ```plaintext
   Input: [[1]]
   Output: true
   ```
2. **Typical Case**:
   ```plaintext
   Input: [
     [1, 2, 3],
     [4, 1, 2],
     [5, 4, 1]
   ]
   Output: true
   ```
3. **Failing Case**:
   ```plaintext
   Input: [
     [1, 2],
     [2, 2]
   ]
   Output: false
   ```
4. **Edge Case**:
   ```plaintext
   Input: [[1], [1], [1]]
   Output: true
   ```

**Dry Run with Complex Input**:
```plaintext
matrix = [
  [1, 2, 3, 4],
  [5, 1, 2, 3],
  [9, 5, 1, 2]
]
Steps:
- Compare (1,1) with (0,0): 1 == 1 (pass)
- Compare (1,2) with (0,1): 2 == 2 (pass)
- ...
- All checks pass.
Output: true
```

---

### 6. Evaluate

- **Time Complexity**: O(m × n)  
  - The matrix is traversed once, where `m` is the number of rows and `n` is the number of columns.
- **Space Complexity**: O(1)  
  - Only constant extra space is used.

---

### Additional Notes

#### Why This Problem is Important
- Tests fundamental matrix traversal concepts.
- Helps build an understanding of diagonal traversal logic.
- Commonly used in coding interviews for problem-solving practice.

#### Prerequisites for Practicing This Problem
- Familiarity with 2D array traversal.
- Understanding of boundary conditions to avoid index out-of-bounds errors.

#### Industry Relevance
- Problems with matrix traversal occur in image processing and numerical computations.
- Diagonal consistency checks are important in pattern recognition.

#### Follow-up Practice Problems
- LeetCode 48: Rotate Image
- LeetCode 73: Set Matrix Zeroes
- LeetCode 54: Spiral Matrix
