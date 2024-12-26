# LeetCode 84: Largest Rectangle in Histogram

### Problem Statement
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- **Inputs**: 
  - `heights`: List of non-negative integers representing the heights of histogram bars. Example: `[2,1,5,6,2,3]`
- **Outputs**:
  - An integer representing the maximum area of a rectangle that can be formed within the histogram. Example Output: `10`
- **Constraints**:
  - `1 <= heights.length <= 10^5`
  - `0 <= heights[i] <= 10^4`
- **Assumptions**:
  - The histogram's width for each bar is fixed at `1`.
  - An empty rectangle has an area of `0`.

---

### 2. Match
- **Pattern**:
  - This problem involves finding the maximum area of rectangles formed using contiguous bars in a histogram.
  - Known problem-solving techniques include the use of a **monotonic stack** to efficiently calculate the largest rectangle area.
- **Data Structures**:
  - Use a **stack** to store pairs `(height, index)` for maintaining monotonicity of increasing bar heights.

---

### 3. Plan
1. Initialize an empty stack and a variable `max_area` to store the maximum rectangle area.
2. Traverse the histogram array:
   - For each bar:
     - If the current bar height is less than the top of the stack, repeatedly pop from the stack and calculate the rectangle area.
     - Update `max_area` accordingly.
     - Push the current bar with its start index onto the stack.
3. After traversal, process any remaining bars in the stack.
4. Return `max_area` as the result.

**Edge Cases**:
- Histogram with only one bar: `[5]`.
- Histogram with all bars of the same height: `[2, 2, 2, 2]`.
- Histogram with varying heights: `[2,1,5,6,2,3]`.

**Time Complexity**: O(n)

**Space Complexity**: O(n)

---

### 4. Implement
```python
from typing import List

class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        stack = []  # Monotonic stack, stores (height, start index)
        max_area = 0  # Initialize max_area to 0

        # Iterate through the heights
        for i, height in enumerate(heights):
            start = i  # The start index for the current bar
            # Adjust stack to maintain monotonicity
            while stack and height < stack[-1][0]:
                h, idx = stack.pop()  # Pop the top element from the stack
                max_area = max(max_area, h * (i - idx))  # Calculate area and update max_area
                start = idx  # Update the start index for the current bar
            stack.append((height, start))  # Push the current bar with its start index

        # Process remaining elements in the stack
        while stack:
            h, idx = stack.pop()
            max_area = max(max_area, h * (n - idx))  # Calculate area and update max_area

        return max_area
```

**Step-by-Step Explanation**:
1. **Initialization**: Create an empty stack and set `max_area` to 0.
2. **Traverse `heights`**:
   - For each bar:
     - Pop bars from the stack if the current bar height is less than the top of the stack.
     - Calculate the area of the rectangle using the popped height as the smallest height.
     - Update `max_area` if the calculated area is larger.
     - Push the current bar with its effective start index onto the stack.
3. **Post-Traversal**:
   - Pop all remaining bars in the stack and calculate their rectangle areas.
   - Update `max_area` accordingly.
4. **Return**: Return `max_area` as the maximum rectangle area.

---

### 5. Review
#### Dry Run Example
**Input**: `[2, 1, 5, 6, 2, 3]`

1. **Initialization**: `stack = []`, `max_area = 0`
2. **Traverse `heights`**:
   - `i=0, height=2`: Push `(2, 0)`.
   - `i=1, height=1`: Pop `(2, 0)`, calculate `2 * (1 - 0) = 2`, update `max_area = 2`. Push `(1, 0)`.
   - `i=2, height=5`: Push `(5, 2)`.
   - `i=3, height=6`: Push `(6, 3)`.
   - `i=4, height=2`: Pop `(6, 3)`, calculate `6 * (4 - 3) = 6`. Pop `(5, 2)`, calculate `5 * (4 - 2) = 10`, update `max_area = 10`. Push `(2, 2)`.
   - `i=5, height=3`: Push `(3, 5)`.
3. **Post-Traversal**:
   - Pop `(3, 5)`, calculate `3 * (6 - 5) = 3`.
   - Pop `(2, 2)`, calculate `2 * (6 - 2) = 8`.
   - Pop `(1, 0)`, calculate `1 * (6 - 0) = 6`.
4. **Result**: `max_area = 10`

#### Edge Cases
- Single bar: `[5]` -> `max_area = 5`
- All bars same height: `[2, 2, 2, 2]` -> `max_area = 8`

---

### 6. Evaluate
**Time Complexity**: O(n)  
Each bar is pushed and popped from the stack exactly once.

**Space Complexity**: O(n)  
Stack space is proportional to the number of bars in the histogram.

---

### Additional Notes
#### The Reason Why This Problem is Important
This problem demonstrates how to use monotonic stacks for efficient computation, a common pattern in competitive programming and real-world applications involving range queries.

#### Prerequisites for Practicing This Problem
- Understanding of stacks.
- Basic array traversal and rectangle area calculation.

#### Industry Relevance
This problem is relevant in scenarios where histogram-like data is analyzed, such as data visualization and image processing.

#### Follow-up Practice Problems
- LeetCode 85: Maximal Rectangle
- LeetCode 42: Trapping Rain Water
- LeetCode 739: Daily Temperatures
