
# LeetCode 42: Trapping Rain Water

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

#### Problem Statement:
Given an array `height` representing the elevation map, where the width of each bar is 1, compute how much water it can trap after raining.

#### Input:
- `height`: A list of non-negative integers, where `height[i]` represents the height of the ith bar.

#### Output:
- An integer representing the total water trapped.

#### Constraints:
1. 1 <= len(height) <= 10^5  
2. 0 <= height[i] <= 10^4

#### Assumptions:
- Water trapped between two bars is calculated as the difference between the smaller of the left or right maximum heights and the bar's height at that position.

---

### **2. Match**

#### Related Problem Types:
- Two-pointer technique
- Dynamic programming

#### Suitable Approach:
- Use the **two-pointer technique** to efficiently calculate trapped water with O(n) time complexity and O(1) space complexity.

---

### **3. Plan**

#### Steps to Solve:
1. **Handle edge cases**:
   - If `height` is empty or its length is less than 2, return `0`.

2. **Initialize pointers and variables**:
   - Two pointers: `l` (left) and `r` (right), initialized to 0 and len(height) - 1.
   - Two variables: `left_max` and `right_max`, initialized to `height[l]` and `height[r]`.
   - `trapped_water`: A variable to store the total trapped water, initialized to 0.

3. **Iterate using two pointers**:
   - Compare `left_max` and `right_max`:
     - If `left_max < right_max`, calculate trapped water at `l`, update `left_max`, and move the left pointer to the right.
     - Otherwise, calculate trapped water at `r`, update `right_max`, and move the right pointer to the left.

4. **Return the total trapped water**.

#### Time Complexity:
- O(n): Single traversal of the `height` array.

#### Space Complexity:
- O(1): Uses constant space for variables.

---

### **4. Implement**

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        # Edge case: empty list or less than 2 elements
        if not height or len(height) < 2: 
            return 0

        # Initialize two pointers and variables
        l, r = 0, len(height) - 1
        left_max, right_max = height[l], height[r]
        trapped_water = 0

        # Two-pointer traversal
        while l < r:
            if left_max < right_max:
                # Calculate water trapped at left pointer
                trapped_water += max(0, left_max - height[l])
                # Move left pointer and update left_max
                l += 1
                left_max = max(left_max, height[l])
            else:
                # Calculate water trapped at right pointer
                trapped_water += max(0, right_max - height[r])
                # Move right pointer and update right_max
                r -= 1
                right_max = max(right_max, height[r])
        
        # Return the total trapped water
        return trapped_water
```

#### Implementation Notes:
- **Edge case check**: `if not height or len(height) < 2:` ensures the program doesn’t perform unnecessary calculations for invalid input.
- **Two-pointer traversal**: Efficiently determines the trapped water for each segment by comparing `left_max` and `right_max`.

---

### **5. Review**

#### Dry Run Example:

##### Input:
```python
height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
```

##### Execution:
1. **Initialization**:
   - `l = 0`, `r = 11`
   - `left_max = 0`, `right_max = 1`
   - `trapped_water = 0`

2. **First iteration**:
   - `left_max < right_max` (0 < 1)
   - Add `max(0, left_max - height[l]) = max(0, 0 - 0) = 0` to `trapped_water`.
   - Increment `l` to 1 and update `left_max = max(0, 1) = 1`.

3. **Second iteration**:
   - `left_max == right_max` (1 == 1)
   - Add `max(0, right_max - height[r]) = max(0, 1 - 1) = 0` to `trapped_water`.
   - Decrement `r` to 10 and update `right_max = max(1, 2) = 2`.

4. **Repeat** until `l == r`.

##### Output:
```python
trapped_water = 6
```

---

### **6. Evaluate**

#### Time Complexity:
- O(n): Only one traversal of the array is needed.

#### Space Complexity:
- O(1): No additional data structures are used.

#### Optimizations:
- This approach is already optimal with respect to time and space.

---

### **Additional Notes**

#### Why This Problem is Important:
- It tests the ability to apply efficient algorithms such as the two-pointer technique.
- It emphasizes handling edge cases and optimizing for both time and space.

#### Prerequisites:
- Understanding of the two-pointer technique.
- Basic knowledge of array manipulation and edge case handling.

#### Industry Relevance:
- Common in coding interviews for companies like FAANG.
- Highlights problem-solving skills and efficiency in algorithm design.

#### Follow-Up Practice Problems:
1. [LeetCode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
2. [LeetCode 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
3. [LeetCode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
