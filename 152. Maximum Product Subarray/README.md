# LeetCode 152: Maximum Product Subarray

### Problem Statement

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.  

### Example 1
```
Input: nums = [2, 3, -2, 4]
Output: 6
Explanation: [2, 3] has the largest product 6.
```

### Example 2
```
Input: nums = [-2, 0, -1]
Output: 0
Explanation: The result cannot be -2, as [-2, -1] is not a subarray.
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**:
  - An array of integers `nums` (size ≥ 1).
  - Values may include positive, negative, and zero.

- **Outputs**:
  - An integer representing the maximum product of a contiguous subarray.

- **Constraints**:
  - The subarray must be contiguous.
  - Edge cases:
    - Single element: `nums = [5] → Output: 5`.
    - Contains zeros: `nums = [0, 2, -1] → Output: 2`.
    - All negatives: `nums = [-2, -3, -4] → Output: 12`.

- **Assumptions**:
  - The input array is non-empty.

---

### 2. Match

- **Problem Type**:
  - This is a **dynamic programming (DP)** problem.
  - It requires maintaining state across iterations, specifically the maximum and minimum products.

- **Key Observations**:
  - A negative number flips the sign of the product, so the current maximum can become the minimum and vice versa.
  - Need to track both the maximum and minimum products at each step.

- **Relevant Algorithms**:
  - Dynamic programming to keep track of running maximum and minimum products.
  - Use a rolling variable approach to optimize space complexity to O(1).

---

### 3. Plan

#### Steps:
1. Initialize `max_product`, `min_product`, and `result` to `nums[0]` (the first element).
2. Iterate through the array starting from index 1:
   - If the current number is negative, swap `max_product` and `min_product` (handle sign flipping).
   - Update `max_product` to the maximum of:
     - The current number.
     - The product of the current number and the previous `max_product`.
   - Update `min_product` to the minimum of:
     - The current number.
     - The product of the current number and the previous `min_product`.
   - Update `result` to the maximum of `result` and `max_product`.
3. Return `result` after the loop ends.

#### Time Complexity:
- O(n): Single pass through the array.

#### Space Complexity:
- O(1): Only constant extra space used.

---

### 4. Implement

```python
from typing import List

class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        # Handle edge case: empty array
        if not nums:
            return 0

        # Step 1: Initialize variables
        max_product = nums[0]
        min_product = nums[0]
        result = nums[0]

        # Step 2: Iterate through the array starting from index 1
        for i in range(1, len(nums)):
            num = nums[i]

            # Step 3: Handle negative numbers by swapping max and min products
            if num < 0:
                max_product, min_product = min_product, max_product
            
            # Step 4: Update max_product and min_product
            max_product = max(num, num * max_product)
            min_product = min(num, num * min_product)

            # Step 5: Update the result
            result = max(result, max_product)

        # Step 6: Return the final result
        return result
```

#### Notes for Implementation:
1. Initialize `max_product`, `min_product`, and `result` with the first element to account for arrays with one element.
2. The swap step (`if num < 0`) ensures that the flipping effect of negative numbers is handled correctly.
3. Each step updates the `max_product` and `min_product` based on the current number and previous state.

---

### 5. Review

#### Debugging and Edge Cases
- **Edge Case 1**: Single element array
  - Input: `nums = [-2]`
  - Output: `-2`
- **Edge Case 2**: Contains zeros
  - Input: `nums = [-2, 0, -1]`
  - Output: `0`
- **Edge Case 3**: All negatives
  - Input: `nums = [-2, -3, -4]`
  - Output: `12`

#### Dry Run
Input: `nums = [2, 3, -2, 4]`

1. **Initialization**:
   - `max_product = 2, min_product = 2, result = 2`

2. **Iteration**:
   - At `i = 1` (`num = 3`):
     - `max_product = max(3, 3 * 2) = 6`
     - `min_product = min(3, 3 * 2) = 3`
     - `result = max(6, 2) = 6`
   - At `i = 2` (`num = -2`):
     - Swap `max_product` and `min_product`:
       - `max_product = 3, min_product = 6`
     - `max_product = max(-2, -2 * 3) = -2`
     - `min_product = min(-2, -2 * 6) = -12`
     - `result = max(6, -2) = 6`
   - At `i = 3` (`num = 4`):
     - `max_product = max(4, 4 * -2) = 4`
     - `min_product = min(4, 4 * -12) = -48`
     - `result = max(6, 4) = 6`

3. **Result**:
   - `result = 6`

---

### 6. Evaluate

#### Time Complexity:
- O(n): Single traversal of the array.

#### Space Complexity:
- O(1): No additional data structures.

#### Optimizations:
- The solution already uses a space-optimized approach with rolling variables.

---

### Additional Notes

#### Why This Problem is Important
- It teaches handling sign flipping and edge cases effectively.
- Introduces the dynamic programming technique of maintaining multiple states.

#### Prerequisites for Practicing This Problem
- Understanding of dynamic programming.
- Familiarity with maintaining rolling variables for space optimization.

#### Industry Relevance
- Solving optimization problems efficiently is crucial for real-world applications such as financial modeling and data analytics.

#### Follow-up Practice Problems
1. [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
2. [198. House Robber](https://leetcode.com/problems/house-robber/)
3. [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
