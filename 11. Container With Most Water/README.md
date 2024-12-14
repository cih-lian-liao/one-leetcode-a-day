
# LeetCode 11: Container With Most Water

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### Problem Statement:
You are given an array `height` where each element represents the height of a vertical line at that position. Two lines and the x-axis form a container that can hold water. Find the maximum amount of water the container can store.

#### Input:
- `height`: List of integers representing heights of lines.  
  Example: `[1,8,6,2,5,4,8,3,7]`

#### Output:
- Integer representing the maximum area of water the container can store.  
  Example: `49`

#### Constraints:
1. `n == len(height)`
2. `2 <= n <= 10^5`
3. `0 <= height[i] <= 10^4`

#### Assumptions:
- The heights are non-negative integers.
- The container is formed by choosing two lines.
- The width of the container is the distance between the two lines.

---

### **2. Match**

#### Problem Type:
- This is an **optimization problem**, requiring us to maximize the area of the container.

#### Pattern:
- **Two-pointer technique**: Start with pointers at the two ends of the array and move them inward to find the maximum area.

#### Data Structures and Algorithms:
- Use two pointers to iteratively calculate the area while adjusting the pointers to optimize for maximum area.

---

### **3. Plan**

#### Step-by-Step Plan:
1. **Initialize Pointers and Variables**:
   - Use two pointers `left` and `right`, starting at the beginning and end of the array, respectively.
   - Initialize `max_area` to store the maximum area found.

2. **Iterate Until Pointers Meet**:
   - Calculate the current area using `min(height[left], height[right]) * (right - left)`.
   - Update `max_area` if the current area is greater.
   - Move the pointer corresponding to the smaller height inward:
     - If `height[left] < height[right]`, move `left` inward (`left += 1`).
     - Otherwise, move `right` inward (`right -= 1`).

3. **Return the Result**:
   - Once the pointers meet, return `max_area`.

#### Edge Cases:
1. **Small Input**: If `height = [1, 1]`, the maximum area is `1`.
2. **Equal Heights**: If `height = [2, 2, 2, 2]`, the algorithm should still find the correct area.
3. **Very Large Heights**: The algorithm should handle large values of `height` efficiently.

#### Time Complexity:
- O(n): Each pointer is moved at most `n` times.

#### Space Complexity:
- O(1): No additional data structures are used.

---

### **4. Implement**

#### Python Code:
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # Initialize pointers and variables
        left, right = 0, len(height) - 1
        max_area = 0
        
        # Iterate until the pointers meet
        while left < right:
            # Calculate the current area
            width = right - left
            current_area = min(height[left], height[right]) * width
            
            # Update max_area if the current area is larger
            max_area = max(max_area, current_area)
            
            # Move the pointer corresponding to the smaller height inward
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        
        # Return the maximum area found
        return max_area
```

#### Implementation Notes:
- **Line-by-line Explanation**:
  1. Initialize `left` and `right` pointers and `max_area`.
  2. Use a `while` loop to calculate the area until the pointers meet.
  3. Use `min(height[left], height[right])` to calculate the container height.
  4. Move the smaller height pointer inward to explore larger heights.
  5. Return `max_area` after the loop.

---

### **5. Review**

#### Dry Run:
Example Input: `height = [1,8,6,2,5,4,8,3,7]`

1. Initial: `left = 0`, `right = 8`, `max_area = 0`.
   - Width = `8 - 0 = 8`.
   - Area = `min(1, 7) * 8 = 8`.
   - Update `max_area = 8`.

2. Move `left`: `left = 1`.
   - Width = `8 - 1 = 7`.
   - Area = `min(8, 7) * 7 = 49`.
   - Update `max_area = 49`.

3. Move `right`: `right = 7`.
   - Width = `7 - 1 = 6`.
   - Area = `min(8, 3) * 6 = 18`.

4. Continue until `left = right`.  
   Final `max_area = 49`.

#### Test Cases:
1. `height = [1,1]` → Output: `1`
2. `height = [4,3,2,1,4]` → Output: `16`
3. `height = [1,2,4,3]` → Output: `4`

---

### **6. Evaluate**

#### Time Complexity:
- O(n): The two pointers traverse the array at most once.

#### Space Complexity:
- O(1): No additional space is used.

#### Optimizations:
- This solution is optimal in terms of time and space for the given problem.

---

### **Additional Notes**

#### Why This Problem is Important:
- It tests understanding of the **two-pointer technique**, which is widely used in array-related problems.
- It demonstrates how to optimize brute force approaches to achieve linear time complexity.

#### Prerequisites:
- Understanding of two-pointer technique.
- Basic knowledge of arrays and how to calculate areas.

#### Industry Relevance:
- Common interview question to assess optimization and array manipulation skills.

#### Follow-up Practice Problems:
1. [Trapping Rain Water (LeetCode 42)](https://leetcode.com/problems/trapping-rain-water/)
2. [Largest Rectangle in Histogram (LeetCode 84)](https://leetcode.com/problems/largest-rectangle-in-histogram/)
3. [Minimum Size Subarray Sum (LeetCode 209)](https://leetcode.com/problems/minimum-size-subarray-sum/)
