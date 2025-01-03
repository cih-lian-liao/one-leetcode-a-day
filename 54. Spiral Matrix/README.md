
# LeetCode Problem 54: Spiral Matrix

### Problem Statement

Given an `m x n` matrix, return all elements of the matrix in spiral order.

**Example 1:**
```
Input: matrix = [
 [1, 2, 3],
 [4, 5, 6],
 [7, 8, 9]
]
Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

**Example 2:**
```
Input: matrix = [
 [1, 2, 3, 4],
 [5, 6, 7, 8],
 [9, 10, 11, 12]
]
Output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input:**
  - A 2D matrix `m x n`.
  - Each cell contains an integer.

- **Output:**
  - A list of integers representing the matrix elements in spiral order.

- **Constraints:**
  - Matrix dimensions: `1 <= m, n <= 10`.
  - Elements: `-100 <= matrix[i][j] <= 100`.

- **Clarifications:**
  - The matrix can be rectangular (e.g., `3x4` or `4x3`).
  - Single-row or single-column matrices are valid inputs.
  - Empty matrices should return an empty list.

---

### 2. Match

- **Pattern:** 
  - This problem involves traversing a 2D matrix in a specific order.
  - It's similar to matrix traversal problems but requires maintaining boundaries dynamically.

- **Data Structures and Algorithms:**
  - Use four boundary pointers (`top`, `bottom`, `left`, `right`) to define the traversal area.
  - Iterative approach with conditional checks to ensure valid traversal bounds.

---

### 3. Plan

1. **Initialization:**
   - Define four pointers: `top = 0`, `bottom = m-1`, `left = 0`, `right = n-1`.
   - Create an empty list `result` to store the output.

2. **Traversal Logic:**
   - Traverse the matrix in the following order:
     1. **Right:** Traverse from `left` to `right` along `top`.
     2. **Down:** Traverse from `top+1` to `bottom` along `right`.
     3. **Left:** If `top <= bottom`, traverse from `right-1` to `left` along `bottom`.
     4. **Up:** If `left <= right`, traverse from `bottom-1` to `top` along `left`.
   - After each direction, update the corresponding boundary.

3. **Termination:**
   - Stop the traversal when `top > bottom` or `left > right`.

4. **Edge Cases:**
   - Empty matrix.
   - Single-row or single-column matrix.

---

### 4. Implement

```python
def spiralOrder(matrix):
    # Step 1: Initialize boundaries
    if not matrix or not matrix[0]:
        return []
    
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    result = []

    # Step 2: Traverse while valid bounds exist
    while top <= bottom and left <= right:
        # Traverse Right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1  # Shrink top boundary
        
        # Traverse Down
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1  # Shrink right boundary
        
        # Traverse Left
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1  # Shrink bottom boundary
        
        # Traverse Up
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1  # Shrink left boundary

    return result
```

---

### 5. Review

**Test Cases:**

1. **Example 1:**
   ```python
   matrix = [
       [1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]
   ]
   print(spiralOrder(matrix))
   # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
   ```

2. **Example 2:**
   ```python
   matrix = [
       [1, 2, 3, 4],
       [5, 6, 7, 8],
       [9, 10, 11, 12]
   ]
   print(spiralOrder(matrix))
   # Output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
   ```

3. **Edge Case: Empty Matrix:**
   ```python
   matrix = []
   print(spiralOrder(matrix))
   # Output: []
   ```

4. **Edge Case: Single Row:**
   ```python
   matrix = [[1, 2, 3, 4]]
   print(spiralOrder(matrix))
   # Output: [1, 2, 3, 4]
   ```

**Dry Run with Complex Case:**

Matrix:
```
[
 [1, 2, 3, 4],
 [5, 6, 7, 8],
 [9, 10, 11, 12],
 [13, 14, 15, 16]
]
```

Steps:
1. Traverse top row: `[1, 2, 3, 4]`.
2. Traverse right column: `[8, 12, 16]`.
3. Traverse bottom row: `[15, 14, 13]`.
4. Traverse left column: `[9, 5]`.
5. Repeat for inner spiral: `[6, 7, 11, 10]`.

Output:
```
[1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10]
```

---

### 6. Evaluate

- **Time Complexity:** O(m * n) — Every element is visited exactly once.
- **Space Complexity:** O(1) — No additional data structures are used apart from the output list.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates boundary management and matrix traversal techniques.
- Frequently appears in interviews to test problem decomposition skills.

#### Prerequisites for Practicing
- Understanding of 2D arrays and traversal logic.
- Basic knowledge of loops and conditionals.

#### Industry Relevance
- Used in applications requiring grid-based traversal (e.g., games, graphics).

#### Follow-up Practice Problems
1. **LeetCode 59:** Spiral Matrix II
2. **LeetCode 498:** Diagonal Traverse
3. **LeetCode 885:** Spiral Matrix III
