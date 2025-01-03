
# LeetCode Problem 59: Spiral Matrix II

### Problem Statement

Given a positive integer `n`, generate an `n x n` matrix filled with elements from 1 to n^2 in spiral order.

**Example:**

**Input:** `n = 3`  
**Output:**  
```
[
 [1, 2, 3],
 [8, 9, 4],
 [7, 6, 5]
]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs:**
  - A single positive integer `n`, where 1 ≤ n ≤ 20.
  
- **Outputs:**
  - An `n x n` 2D list (matrix) filled in spiral order with integers from 1 to n^2.

- **Constraints:**
  - Each number in the matrix is unique and follows the spiral order.

- **Assumptions:**
  - The matrix is square.
  - There are no invalid inputs.

---

### 2. Match

- **Pattern:**
  - This is a matrix traversal problem that can be solved by iteratively filling the matrix layer by layer in a spiral order.

- **Relevant Data Structures:**
  - A 2D list to represent the matrix.
  - Variables to track the boundaries (`top`, `bottom`, `left`, `right`).

---

### 3. Plan

**Step-by-Step Plan:**
1. Initialize an `n x n` matrix filled with zeros.
2. Set boundaries:
   - `top = 0`, `bottom = n - 1`
   - `left = 0`, `right = n - 1`
3. Initialize a counter `num = 1`.
4. Loop until `num > n^2`:
   - Fill the top row from left to right, then increment `top`.
   - Fill the right column from top to bottom, then decrement `right`.
   - Fill the bottom row from right to left, then decrement `bottom`.
   - Fill the left column from bottom to top, then increment `left`.
5. Return the matrix.

**Edge Cases:**
- n = 1: Single cell matrix.
- n = 2: Smallest matrix with an actual spiral.

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n^2)

---

### 4. Implement

```python
def generateMatrix(n):
    # Step 1: Initialize the matrix
    matrix = [[0] * n for _ in range(n)]
    
    # Step 2: Define boundaries
    top, bottom = 0, n - 1
    left, right = 0, n - 1
    
    # Step 3: Initialize the counter
    num = 1
    
    # Step 4: Fill the matrix in a spiral order
    while num <= n * n:
        # Fill the top row
        for col in range(left, right + 1):
            matrix[top][col] = num
            num += 1
        top += 1
        
        # Fill the right column
        for row in range(top, bottom + 1):
            matrix[row][right] = num
            num += 1
        right -= 1
        
        # Fill the bottom row
        for col in range(right, left - 1, -1):
            matrix[bottom][col] = num
            num += 1
        bottom -= 1
        
        # Fill the left column
        for row in range(bottom, top - 1, -1):
            matrix[row][left] = num
            num += 1
        left += 1
    
    return matrix
```

**Implementation Notes:**
- The boundaries are updated after each direction is filled.
- The condition `num <= n * n` ensures we stop once all numbers are filled.

---

### 5. Review

**Debugging Steps:**
1. Test with small inputs like `n = 1` and `n = 2` to ensure edge cases are handled.
2. For `n = 3`, dry run the code:
   - Initial matrix:  
     ```
     [[0, 0, 0],
      [0, 0, 0],
      [0, 0, 0]]
     ```
   - After filling top row:  
     ```
     [[1, 2, 3],
      [0, 0, 0],
      [0, 0, 0]]
     ```
   - After filling right column:  
     ```
     [[1, 2, 3],
      [0, 0, 4],
      [0, 0, 5]]
     ```
   - After filling bottom row:  
     ```
     [[1, 2, 3],
      [0, 0, 4],
      [7, 6, 5]]
     ```
   - After filling left column:  
     ```
     [[1, 2, 3],
      [8, 0, 4],
      [7, 6, 5]]
     ```
   - Final matrix:  
     ```
     [[1, 2, 3],
      [8, 9, 4],
      [7, 6, 5]]
     ```

**Testing:**
- Test with n = 4 and validate the output matches expectations.

---

### 6. Evaluate

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n^2)

**Optimizations:**
- None needed; the solution is already efficient for the given constraints.

**Alternative Approaches:**
- Recursive filling of layers (less intuitive and harder to implement).

---

### Additional Notes

#### Why This Problem is Important
- Helps practice matrix traversal, a common pattern in algorithmic problems.
- Strengthens understanding of boundary management in 2D arrays.

#### Prerequisites for Practicing This Problem
- Basic understanding of 2D arrays.
- Familiarity with loops and conditionals.

#### Industry Relevance
- Useful for problems involving grid-based data like games, simulations, and spatial algorithms.

### Follow-Up Practice Problems
1. LeetCode 54: Spiral Matrix
2. LeetCode 885: Spiral Matrix III
3. LeetCode 498: Diagonal Traverse
