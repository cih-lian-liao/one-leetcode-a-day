# LeetCode Problem 566: Reshape the Matrix


### Problem Statement

You are given a `m x n` matrix `mat` and two integers `r` and `c` representing the number of rows and columns of the reshaped matrix. The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were in `mat`.

If the reshape operation with given parameters is not possible, return the original matrix.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

- **Input**:
  - `mat`: 2D list of integers (size `m x n`).
  - `r`: Integer, number of rows in the reshaped matrix.
  - `c`: Integer, number of columns in the reshaped matrix.

- **Output**:
  - Reshaped matrix of size `r x c` if possible.
  - Original matrix if reshaping is not possible.

- **Constraints**:
  - The total number of elements in `mat` must equal `r * c` to allow reshaping.
  - All elements must retain their order from `mat`.

- **Assumptions**:
  - The matrix `mat` is non-empty.
  - Reshaping is row-major (row-by-row).

---

### **2. Match**

- This problem relates to array traversal and manipulation.
- Pattern: 2D to 1D transformation and vice versa.
- Useful algorithms and techniques:
  - Flatten a matrix into a 1D list.
  - Split a 1D list into chunks of size `c`.

---

### **3. Plan**

1. **Validation**:
   - Check if `m * n == r * c`. If not, return the original matrix.

2. **Flatten Matrix**:
   - Use list comprehension to convert `mat` into a 1D list.

3. **Reshape**:
   - Iterate through the flattened list and group elements into sublists of size `c`.

4. **Output**:
   - Return the reshaped matrix.

**Edge Cases**:
- `r` or `c` is 1 (result is a single row/column).
- The matrix is already of size `r x c` (return the same matrix).
- Invalid reshape dimensions (`m * n != r * c`).

**Time Complexity**: O(m * n) (where `m` is the number of rows and `n` is the number of columns).  
**Space Complexity**: O(m * n) (for storing the reshaped matrix).

---

### **4. Implement**

```python
def matrixReshape(mat, r, c):
    # 1. Validate dimensions
    m, n = len(mat), len(mat[0])
    if m * n != r * c:
        return mat  # Return original matrix if reshape is impossible

    # 2. Flatten the matrix
    flat_list = [num for row in mat for num in row]

    # 3. Reshape the matrix
    reshaped_matrix = []
    for i in range(r):
        reshaped_matrix.append(flat_list[i * c:(i + 1) * c])

    return reshaped_matrix
```

#### **Step-by-Step Explanation**

1. **Validation**:
   - Check if `m * n == r * c`:
     - If the total number of elements doesn’t match, return `mat`.

2. **Flattening**:
   - Use a nested loop within a list comprehension:
     ```python
     flat_list = [num for row in mat for num in row]
     ```
   - Extract each element from `mat` row-by-row.

3. **Reshaping**:
   - Create new rows from `flat_list` using slicing:
     ```python
     reshaped_matrix.append(flat_list[i * c:(i + 1) * c])
     ```
   - Iterate `r` times to create `r` rows, each with `c` elements.

4. **Output**:
   - Return the reshaped matrix.

---

### **5. Review**

**Test Cases**:

1. **Basic Case**:
   ```python
   mat = [[1, 2], [3, 4]]
   r = 1
   c = 4
   # Output: [[1, 2, 3, 4]]
   ```

2. **Invalid Reshape**:
   ```python
   mat = [[1, 2], [3, 4]]
   r = 2
   c = 3
   # Output: [[1, 2], [3, 4]] (original matrix)
   ```

3. **Already Reshaped**:
   ```python
   mat = [[1, 2], [3, 4]]
   r = 2
   c = 2
   # Output: [[1, 2], [3, 4]]
   ```

4. **Edge Case: Single Row**:
   ```python
   mat = [[1, 2, 3, 4]]
   r = 2
   c = 2
   # Output: [[1, 2], [3, 4]]
   ```

**Debugging**:

- Dry-run the test cases step-by-step:
  - Flattening: Verify the 1D list.
  - Reshaping: Ensure slicing and grouping are correct.

---

### **6. Evaluate**

**Complexity Analysis**:
- Time Complexity: O(m * n) (flattening + reshaping).
- Space Complexity: O(m * n) (storage for reshaped matrix).

**Optimizations**:
- Direct indexing instead of intermediate flattening to reduce memory usage.

---

### Additional Notes

**Why This Problem is Important**:
- Helps understand array traversal and manipulation.
- Teaches efficient use of Python’s list comprehension and slicing.

**Prerequisites for Practicing**:
- Understanding of 2D arrays and list comprehension in Python.
- Familiarity with slicing operations.

**Industry Relevance**:
- Common in data preprocessing tasks where reshaping arrays is necessary.
- Useful in machine learning and numerical computing (e.g., reshaping tensors).

**Follow-up Practice Problems**:
1. LeetCode 118: Pascal's Triangle
2. LeetCode 485: Max Consecutive Ones
3. LeetCode 54: Spiral Matrix
