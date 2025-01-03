
# LeetCode 867: Transpose Matrix

### Problem Statement

Given a 2D integer array `matrix`, return the **transpose** of `matrix`.

The **transpose** of a matrix is obtained by flipping the matrix over its diagonal, switching the row and column indices of each element.

### Example Input and Output

#### Example 1:
**Input**:
```
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
**Output**:
```
[[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

#### Example 2:
**Input**:
```
matrix = [[1, 2, 3], [4, 5, 6]]
```
**Output**:
```
[[1, 4], [2, 5], [3, 6]]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**:
  - `matrix`: A 2D list of integers with dimensions `m x n` where `m` is the number of rows and `n` is the number of columns.
- **Outputs**:
  - A new 2D list representing the transpose of `matrix`.
- **Constraints**:
  - `1 <= m, n <= 1000`
  - `1 <= matrix[i][j] <= 10⁹`
- **Clarifications**:
  - The result matrix will have dimensions `n x m`.
  - Each element in the result is located at `result[j][i] = matrix[i][j]`.

---

### 2. Match

- This problem is related to **array transformation**.
- A common pattern for such problems involves iterating through each element in the input matrix and mapping it to the appropriate position in the result matrix.

---

### 3. Plan

**Steps**:
1. Get the dimensions of the input matrix (`m` rows, `n` columns).
2. Create an empty result matrix with dimensions `n x m`.
3. Iterate through each element of the input matrix:
   - For each element at position `(i, j)` in the input, assign its value to position `(j, i)` in the result matrix.
4. Return the result matrix.

**Edge Cases**:
- Single row or column (`m = 1` or `n = 1`).
- Large matrices near the constraints.
- Input with all identical elements (e.g., `matrix = [[1, 1], [1, 1]]`).

**Time Complexity**:
- O(m × n): Iterating through all elements in the matrix.

**Space Complexity**:
- O(m × n): Storing the result matrix.

---

### 4. Implement

```python
from typing import List

class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        # Step 1: Get dimensions of the input matrix
        rows, cols = len(matrix), len(matrix[0])
        
        # Step 2: Initialize the result matrix with swapped dimensions
        result = [[0] * rows for _ in range(cols)]
        
        # Step 3: Fill the result matrix by transposing the elements
        for row in range(rows):
            for col in range(cols):
                result[col][row] = matrix[row][col]
        
        # Step 4: Return the transposed matrix
        return result
```

**Implementation Notes**:
- Create the result matrix using a list comprehension.
- Swap `row` and `col` indices while assigning values.

---

### 5. Review

**Debugging and Test Cases**:

| Input | Expected Output | Notes |
|-------|-----------------|-------|
| `[[1, 2, 3], [4, 5, 6]]` | `[[1, 4], [2, 5], [3, 6]]` | Non-square matrix |
| `[[1]]` | `[[1]]` | Single element |
| `[[1, 2], [3, 4], [5, 6]]` | `[[1, 3, 5], [2, 4, 6]]` | Rectangular matrix |
| `[[1, 1], [1, 1]]` | `[[1, 1], [1, 1]]` | Identical elements |

**Dry Run for `[[1, 2, 3], [4, 5, 6]]`**:
1. Input dimensions: `rows = 2, cols = 3`.
2. Result matrix initialized as `[[0, 0], [0, 0], [0, 0]]`.
3. Iteration:
   - For `(0, 0)`: `result[0][0] = 1`.
   - For `(0, 1)`: `result[1][0] = 2`.
   - For `(0, 2)`: `result[2][0] = 3`.
   - Continue for the second row...
4. Final result: `[[1, 4], [2, 5], [3, 6]]`.

---

### 6. Evaluate

- **Time Complexity**: O(m × n)
  - Iterating through every element in the input matrix.
- **Space Complexity**: O(m × n)
  - Allocating space for the result matrix.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Fundamental to understanding matrix operations, which are used in computer graphics, scientific computing, and machine learning.

#### Prerequisites for Practicing This Problem
- Understanding of 2D arrays.
- Familiarity with basic Python syntax for list comprehensions and loops.

#### Industry Relevance
- Useful in fields like data analysis and machine learning where matrix manipulations are common.

#### Follow-up Practice Problems
1. LeetCode 48: Rotate Image.
2. LeetCode 304: Range Sum Query 2D - Immutable.
3. LeetCode 73: Set Matrix Zeroes.

#### Mistakes Made and How to Avoid Them
- **Mistake**: Attempting to modify the input matrix directly without considering shape mismatches.
  - **Correction**: Always create a new matrix when dimensions differ.
- **Mistake**: Confusing row and column indices during iteration.
  - **Correction**: Clearly document index swaps in code comments.
