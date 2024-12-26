
# LeetCode 240: Search a 2D Matrix II

### Problem Statement

Given an `m x n` matrix where each row is sorted in ascending order and each column is sorted in ascending order, write a function to determine whether a target integer exists in the matrix.

**Constraints:**
- `m == matrix.length`
- `n == matrix[i].length`
- `-10^9 <= matrix[i][j], target <= 10^9`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input:**
  - A 2D matrix of size `m x n` with sorted rows and columns.
  - An integer `target`.

- **Output:**
  - A boolean value: `True` if the target exists in the matrix, otherwise `False`.

- **Key Constraints:**
  - The matrix has sorted properties (rows and columns).
  - Efficient solution is expected (linear scan is not optimal).

- **Clarifications:**
  - The matrix could be empty; handle this edge case.
  - If the target is less than the smallest or greater than the largest element, return `False`.

---

### 2. Match

- This problem is a **search problem** in a sorted 2D matrix.
- The sorted nature of the rows and columns hints at a **binary search** or a **greedy search**.
- **Optimal Choice:** Use a **greedy search** starting from the top-right or bottom-left corner, which allows us to leverage sorting properties.

---

### 3. Plan

#### Approach
1. Start at the **top-right corner** (or bottom-left corner) of the matrix:
   - If the current element is greater than the target, move left.
   - If the current element is less than the target, move down.
   - If the current element matches the target, return `True`.
2. If the search goes out of bounds, return `False`.

#### Edge Cases
- Empty matrix or rows.
- Target is outside the matrix range.

#### Pseudocode
```plaintext
1. Initialize `row = 0` and `col = n - 1` (top-right corner).
2. While `row < m` and `col >= 0`:
   - If `matrix[row][col] == target`, return `True`.
   - If `matrix[row][col] > target`, decrement `col` (move left).
   - If `matrix[row][col] < target`, increment `row` (move down).
3. Return `False`.
```

#### Time Complexity
- **O(m + n)**: Each step moves one row or one column closer to the answer.

#### Space Complexity
- **O(1)**: Constant extra space.

---

### 4. Implement

```python
def searchMatrix(matrix, target):
    """
    Searches for a target value in a sorted 2D matrix.
    
    Args:
    matrix (List[List[int]]): 2D matrix with sorted rows and columns.
    target (int): The value to search for.
    
    Returns:
    bool: True if the target exists in the matrix, otherwise False.
    """
    if not matrix or not matrix[0]:  # Handle edge case of empty matrix
        return False

    m, n = len(matrix), len(matrix[0])
    row, col = 0, n - 1  # Start from the top-right corner

    while row < m and col >= 0:
        current = matrix[row][col]
        if current == target:
            return True
        elif current > target:
            col -= 1  # Move left
        else:
            row += 1  # Move down

    return False
```

---

### 5. Review

#### Dry Run Example

##### Input
```python
matrix = [
    [1, 4, 7, 11, 15],
    [2, 5, 8, 12, 19],
    [3, 6, 9, 16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
]
target = 5
```

##### Step-by-Step Execution
1. Start at `(row=0, col=4)`: `matrix[0][4] = 15`.
   - `15 > 5`, move left to `(row=0, col=3)`.
2. At `(row=0, col=3)`: `matrix[0][3] = 11`.
   - `11 > 5`, move left to `(row=0, col=2)`.
3. At `(row=0, col=2)`: `matrix[0][2] = 7`.
   - `7 > 5`, move left to `(row=0, col=1)`.
4. At `(row=0, col=1)`: `matrix[0][1] = 4`.
   - `4 < 5`, move down to `(row=1, col=1)`.
5. At `(row=1, col=1)`: `matrix[1][1] = 5`.
   - Found the target. Return `True`.

---

### 6. Evaluate

- **Time Complexity:**  
  - O(m + n): Each step eliminates either a row or a column.

- **Space Complexity:**  
  - O(1): No additional data structures are used.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates the ability to leverage sorted data properties for efficient searching.
- Highlights the importance of greedy strategies in coding interviews.

#### Prerequisites for Practicing This Problem
- Understanding of matrix indexing.
- Familiarity with greedy algorithms.

#### Industry Relevance
- Searching efficiently in sorted datasets is a common problem in software engineering.
- Examples include querying sorted databases or processing sorted logs.

#### Follow-up Practice Problems
- LeetCode 74: Search a 2D Matrix (simpler variant).
- LeetCode 378: Kth Smallest Element in a Sorted Matrix.
- LeetCode 34: Find First and Last Position of Element in Sorted Array.

#### Practice Notes

When explaining this problem during an interview:
1. Clearly articulate how the matrix's sorted properties allow for greedy search.
2. Emphasize the trade-off between time and space efficiency.
3. Use examples to validate your approach against edge cases. 
