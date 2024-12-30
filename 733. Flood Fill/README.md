
# LeetCode Problem 733: Flood Fill

### Problem Statement

You are given an image represented as a 2D array of integers `image` where `image[row][col]` represents the pixel value of the image. You are also given three integers `sr`, `sc`, and `newColor`. 

Starting from the pixel `image[sr][sc]`, replace all pixels connected to this pixel by a flood fill (all the pixels connected by a 4-directional path that have the same color as `image[sr][sc]`) with `newColor`.

Return the modified image after performing the flood fill.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs:**
- `image`: A 2D list of integers, representing pixel values.
- `sr`: Integer, starting row index.
- `sc`: Integer, starting column index.
- `newColor`: Integer, the color to replace the region with.

**Output:**
- A 2D list of integers with the flood-filled region updated to `newColor`.

**Constraints:**
- The dimensions of `image` are `m x n` where `1 <= m, n <= 50`.
- The starting pixel is always valid.
- The `newColor` will be a valid integer (0 to 65535).

**Assumptions:**
- If the starting pixel already has `newColor`, return the `image` as is.
- The region connected to the starting pixel is only affected if they share the same color.

---

### 2. Match

This is a classic graph traversal problem, which can be solved using:
- **Depth-First Search (DFS)**: Recursively explore connected pixels.
- **Breadth-First Search (BFS)**: Iteratively explore connected pixels layer by layer.

**Relevant Algorithm/Pattern:**
- DFS or BFS for connected components.

---

### 3. Plan

**Step-by-Step Plan:**
1. Identify the original color of the starting pixel (`image[sr][sc]`).
2. If `newColor` is the same as the original color, return the `image` unchanged.
3. Implement a DFS or BFS to traverse and replace all connected pixels with the same color:
   - For each valid neighboring pixel (up, down, left, right):
     - Check if the pixel's color matches the original color.
     - Replace the color with `newColor`.
     - Recursively or iteratively apply the same operation to its neighbors.
4. Return the updated image.

**Edge Cases:**
- The `image` is a single pixel (`1x1` grid).
- The `newColor` is the same as the starting pixel's color.
- The region to be filled is disconnected or small.

**Time Complexity:** O(m * n), where `m` is the number of rows and `n` is the number of columns in `image`.

**Space Complexity:** O(m * n) for recursion stack (DFS) or queue (BFS).

---

### 4. Implement

#### Python Implementation: DFS Approach

```python
def floodFill(image, sr, sc, newColor):
    rows, cols = len(image), len(image[0])
    original_color = image[sr][sc]

    # If the starting pixel already has the new color, return the image
    if original_color == newColor:
        return image

    # Helper function for DFS
    def dfs(r, c):
        # Base case: if out of bounds or different color, return
        if r < 0 or r >= rows or c < 0 or c >= cols or image[r][c] != original_color:
            return
        # Update the color
        image[r][c] = newColor
        # Recursively call DFS in all four directions
        dfs(r + 1, c)  # down
        dfs(r - 1, c)  # up
        dfs(r, c + 1)  # right
        dfs(r, c - 1)  # left

    dfs(sr, sc)
    return image
```

**Step-by-Step Explanation:**
1. Determine the original color of the starting pixel.
2. If the original color is the same as `newColor`, skip processing to avoid infinite recursion.
3. Use a recursive DFS to explore each connected pixel:
   - Check bounds and pixel color before modifying.
   - Change the pixel color to `newColor`.
   - Call the function for all adjacent pixels.

---

### 5. Review

#### Debugging and Edge Cases

1. **Test Case 1: Basic Case**
   ```python
   image = [[1, 1, 1], [1, 1, 0], [1, 0, 1]]
   sr, sc = 1, 1
   newColor = 2
   print(floodFill(image, sr, sc, newColor))
   ```
   **Expected Output:** 
   ```
   [[2, 2, 2], 
    [2, 2, 0], 
    [2, 0, 1]]
   ```

2. **Test Case 2: Already Updated**
   ```python
   image = [[0, 0, 0], [0, 0, 0]]
   sr, sc = 0, 0
   newColor = 0
   print(floodFill(image, sr, sc, newColor))
   ```
   **Expected Output:** 
   ```
   [[0, 0, 0], 
    [0, 0, 0]]
   ```

3. **Test Case 3: Single Pixel**
   ```python
   image = [[1]]
   sr, sc = 0, 0
   newColor = 3
   print(floodFill(image, sr, sc, newColor))
   ```
   **Expected Output:** 
   ```
   [[3]]
   ```

#### Dry Run

**Complex Case:**
```python
image = [[1, 1, 1], [1, 1, 0], [1, 0, 1]]
sr, sc = 0, 0
newColor = 3
```

- Initial pixel: `(0, 0)` changes from `1` to `3`.
- Neighbors: `(1, 0)`, `(0, 1)` are also `1`, so they change to `3`.
- Continue for all connected pixels.

Final Output:
```
[[3, 3, 3], 
 [3, 3, 0], 
 [3, 0, 1]]
```

---

### 6. Evaluate

**Time Complexity:** O(m * n) - Every pixel is visited once.

**Space Complexity:** O(m * n) - For recursion stack in DFS or queue in BFS.

---

### Additional Notes

#### Why This Problem is Important
- Teaches graph traversal techniques like DFS and BFS.
- Highlights the importance of handling edge cases in grid-based problems.

#### Prerequisites for Practicing This Problem
- Basic understanding of recursion.
- Knowledge of graph traversal methods (DFS/BFS).

#### Industry Relevance
- Commonly used in image processing, flood-filling algorithms, and simulations.

#### Follow-Up Practice Problems
1. LeetCode 200: Number of Islands
2. LeetCode 695: Max Area of Island
3. LeetCode 417: Pacific Atlantic Water Flow
