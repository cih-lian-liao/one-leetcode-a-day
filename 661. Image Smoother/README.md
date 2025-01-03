
# LeetCode Problem 661: Image Smoother

### Problem Statement

Given a 2D matrix `img` representing a grayscale image, smooth the image by replacing each value with the average of the surrounding 8 neighbors and itself. Use floor division for the result. If a neighbor is out of bounds, ignore it.

### Input
- A 2D list `img` with dimensions `m x n`.
- `1 <= m, n <= 200`
- `0 <= img[i][j] <= 255`

### Output
- A 2D list `res` where `res[i][j]` is the smoothed value.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Inputs:
- A 2D integer list `img`.

#### Outputs:
- A 2D integer list `res` where each value is the average of the current cell and its valid neighbors.

#### Constraints:
- Neighbors outside the matrix boundary are ignored.
- Use floor division when calculating averages.

#### Ambiguities/Clarifications:
- Confirmed that each cell can have between 1 and 9 contributing values.

---

### 2. Match

#### Problem Type:
- This problem involves matrix traversal and sliding window operations.

#### Relevant Patterns:
- Matrix traversal
- Directional iteration

#### Data Structures:
- A 2D list for storing results.
- A list of tuples to represent 8 possible directions for neighbors.

---

### 3. Plan

1. Initialize a result matrix `res` of the same dimensions as `img` filled with zeros.
2. Define all 8 directions for neighbors: top-left, top, top-right, left, right, bottom-left, bottom, bottom-right.
3. For each cell `(i, j)` in `img`:
   - Initialize `total = img[i][j]` and `count = 1` (including the current cell).
   - Traverse all 8 directions and check if the neighbor `(ni, nj)` is within bounds.
   - If valid, add the neighbor's value to `total` and increment `count`.
   - Update `res[i][j]` with `total // count` (floor division).
4. Return the result matrix `res`.

#### Time Complexity:
- O(m * n * k), where `k = 8` (constant) is the number of directions.
- Simplifies to O(m * n).

#### Space Complexity:
- O(m * n) for the result matrix.

---

### 4. Implement

```python
def imageSmoother(img):
    # Step 1: Initialize variables
    rows, cols = len(img), len(img[0])
    res = [[0] * cols for _ in range(rows)]  # Result matrix
    
    # Step 2: Define 8 possible directions
    directions = [(-1, -1), (-1, 0), (-1, 1),
                  (0, -1),          (0, 1),
                  (1, -1), (1, 0), (1, 1)]
    
    # Step 3: Traverse the matrix
    for i in range(rows):
        for j in range(cols):
            total, count = img[i][j], 1  # Initialize with current cell
            for di, dj in directions:
                ni, nj = i + di, j + dj
                if 0 <= ni < rows and 0 <= nj < cols:  # Check bounds
                    total += img[ni][nj]
                    count += 1
            res[i][j] = total // count  # Average value
    
    # Step 4: Return the result
    return res
```

---

### 5. Review

#### Debugging:
- Test edge cases:
  - A 1x1 matrix: `[[1]]`
  - A 1xN matrix: `[[1, 2, 3]]`
  - An Mx1 matrix: `[[1], [2], [3]]`
  - All elements the same: `[[5, 5], [5, 5]]`

#### Dry Run:
Input:
```python
img = [[100, 200, 100],
       [200, 50, 200],
       [100, 200, 100]]
```

Step-by-step for `(0, 0)`:
1. Start with `total = 100`, `count = 1`.
2. Neighbors: `(0, 1), (1, 0), (1, 1)` are valid.
3. Total: `100 + 200 + 200 + 50 = 550`.
4. Count: `1 + 3 = 4`.
5. `res[0][0] = 550 // 4 = 137`.

Repeat for other cells. Final output:
```python
[[137, 141, 137],
 [141, 138, 141],
 [137, 141, 137]]
```

---

### 6. Evaluate

#### Time Complexity:
- O(m * n), where `m` is the number of rows and `n` is the number of columns.

#### Space Complexity:
- O(m * n) for the result matrix.

#### Optimizations:
- If space optimization is needed, the result can be stored in-place if the input matrix can be overwritten.

---

### Additional Notes

#### The Reason Why This Problem is Important:
- Enhances skills in matrix traversal and boundary handling.
- Teaches averaging calculations with variable-sized neighborhoods.

#### Prerequisites for Practicing This Problem:
- Familiarity with matrix traversal.
- Knowledge of handling boundary conditions in 2D grids.

#### Industry Relevance:
- Useful for image processing and other applications like blurring or smoothing filters in graphics and vision systems.

#### Follow-up Practice Problems:
1. LeetCode 73: Set Matrix Zeroes
2. LeetCode 289: Game of Life
3. LeetCode 54: Spiral Matrix
