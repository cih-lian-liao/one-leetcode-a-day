
# LeetCode 74: Search a 2D Matrix

### Problem Description
Given an `m x n` matrix with the following properties:
1. Each row is sorted in ascending order.
2. The first integer of each row is greater than the last integer of the previous row.

Write an efficient algorithm to determine if a given target value exists in the matrix.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- **Inputs:**
  - `matrix`: A 2D array of integers with dimensions `m x n`.
  - `target`: An integer value to search for.

- **Outputs:**
  - A boolean `True` if the target exists in the matrix, otherwise `False`.

- **Constraints:**
  - The matrix dimensions (`m` and `n`) are at least 1.
  - The matrix contains integers in sorted order across rows and columns.
  - `target` is an integer.

- **Clarifications:**
  - What if the matrix is empty? → Return `False`.
  - Can `target` be smaller than the smallest value or larger than the largest value? → Yes, handle these cases.

---

### 2. Match
- This problem can be mapped to a **binary search** problem because:
  - The matrix is sorted row-wise and column-wise.
  - We can treat the entire matrix as a flattened 1D sorted array for binary search.

- **Algorithm:** Binary Search
- **Key Data Structures:** Array-like indexing using matrix row and column calculations.

---

### 3. Plan
1. Verify the matrix is not empty.
2. Use binary search over the matrix elements treated as a 1D array.
   - Calculate `row` and `col` using:
     - `row = mid // n`
     - `col = mid % n`
   - Access the matrix element using `matrix[row][col]`.
3. Adjust binary search boundaries (`left` and `right`) based on the comparison with `target`.
4. If the element matches `target`, return `True`.
5. If the search completes without finding the element, return `False`.

- **Edge Cases:**
  - Empty matrix → Return `False`.
  - Single row or single column matrix → Binary search works seamlessly.

- **Time Complexity:** O(log(m*n)), where `m` is the number of rows and `n` is the number of columns.
- **Space Complexity:** O(1), as no extra space is used.

---

### 4. Implement
```python
def searchMatrix(matrix, target):
    # Step 1: Check if the matrix is empty
    if not matrix or not matrix[0]:
        return False
    
    # Step 2: Initialize binary search boundaries
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1
    
    # Step 3: Perform binary search
    while left <= right:
        mid = (left + right) // 2
        row = mid // n
        col = mid % n
        mid_value = matrix[row][col]
        
        if mid_value == target:
            return True
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    
    # Step 4: Target not found
    return False
```

**Implementation Notes:**
1. **Matrix Flattening:**
   - Treat the 2D matrix as a 1D array, where `mid` is the virtual index.
   - Calculate `row` and `col` using:
     - `row = mid // n` (integer division).
     - `col = mid % n` (remainder).

2. **Binary Search Logic:**
   - Compare the middle value (`mid_value`) with `target`.
   - Adjust the search range (`left` and `right`) accordingly.

---

### 5. Review
- **Test Cases:**
  ```python
  matrix1 = [
      [1, 3, 5, 7],
      [10, 11, 16, 20],
      [23, 30, 34, 60]
  ]
  assert searchMatrix(matrix1, 3) == True
  assert searchMatrix(matrix1, 13) == False
  assert searchMatrix([], 1) == False
  assert searchMatrix([[1]], 1) == True
  assert searchMatrix([[1, 3, 5]], 4) == False
  ```

- **Dry Run Example:**
  Input:
  ```python
  matrix = [
      [1, 3, 5, 7],
      [10, 11, 16, 20],
      [23, 30, 34, 60]
  ]
  target = 11
  ```
  Steps:
  1. Initialize `left = 0`, `right = 11`.
  2. Compute `mid = 5`, `row = 1`, `col = 1`, `mid_value = 11`.
  3. `mid_value == target`, return `True`.

---

### 6. Evaluate
- **Time Complexity:**
  - O(log(m*n)), as we perform binary search over `m*n` elements.

- **Space Complexity:**
  - O(1), since no additional space is required.

- **Optimizations:**
  - The solution is already optimal given the constraints and requirements.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Demonstrates the application of binary search in unconventional scenarios.
- Tests your ability to work with multidimensional data structures.

#### Prerequisites for Practicing This Problem
- Understanding of 2D arrays.
- Proficiency in binary search.
- Knowledge of matrix indexing techniques.

#### Industry Relevance
- Searching efficiently in large datasets is a common task in software engineering.
- The ability to adapt binary search for multidimensional data is relevant in database querying, image processing, and game development.

#### Follow-up Practice Problems
1. [LeetCode 240: Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
2. [LeetCode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
3. [LeetCode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
4. [LeetCode 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
