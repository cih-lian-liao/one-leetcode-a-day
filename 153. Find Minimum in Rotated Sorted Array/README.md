
# LeetCode 153: Find Minimum in Rotated Sorted Array

### Problem Description

A sorted array of distinct integers is rotated at an unknown pivot. Given this rotated array, find the minimum element. The solution should be in **O(log n)** time complexity.

**Constraints**:
- All integers in the array are distinct.
- The array is guaranteed to be rotated at least once.
- `1 <= nums.length <= 5000`
- `-5000 <= nums[i] <= 5000`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

- **Input**: A rotated sorted array `nums` (e.g., `[4, 5, 6, 7, 0, 1, 2]`).
- **Output**: The minimum element in the array.
- **Key Observations**:
  1. The array is rotated, so the minimum element is the pivot point.
  2. The array is sorted except for the pivot, making it possible to use binary search for efficiency.
  3. If `nums[middle] > nums[right]`, the minimum lies in the right half.
  4. If `nums[middle] <= nums[right]`, the minimum lies in the left half (including the middle).

**Assumptions**:
- The array will never be empty.
- The integers in the array are distinct, so there will not be duplicate values.

---

### 2. Match

- **Pattern**: Binary search.
- **Why**: Binary search works because even though the array is rotated, it still maintains a partially sorted structure. This allows us to discard half of the search space in each iteration.
- **Algorithm/Technique**: Divide and conquer using binary search.

---

### 3. Plan

1. Initialize two pointers, `left = 0` and `right = len(nums) - 1`.
2. Use a `while` loop with the condition `left < right` to perform binary search.
3. Calculate the middle index as `mid = (left + right) // 2`.
4. Compare `nums[mid]` with `nums[right]`:
   - If `nums[mid] > nums[right]`, the minimum is in the right half (`left = mid + 1`).
   - Otherwise, the minimum is in the left half, including `mid` (`right = mid`).
5. When `left == right`, the minimum element is at `nums[left]`.
6. Return `nums[left]`.

---

### 4. Implement

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        # Initialize pointers
        left, right = 0, len(nums) - 1
        
        # Binary search loop
        while left < right:
            # Calculate mid-point
            mid = (left + right) // 2
            
            # Notes:
            # If nums[mid] > nums[right], the minimum is in the right half
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                # Otherwise, the minimum is in the left half (including mid)
                right = mid
        
        # Return the minimum element
        return nums[left]
```

#### Explanation of Code (Step-by-Step):

1. **Initialization**:
   - Start with `left` pointing to the beginning of the array and `right` pointing to the end.

2. **Binary Search**:
   - Divide the array into two halves using `mid = (left + right) // 2`.
   - Check whether the middle element is greater than the rightmost element:
     - If `nums[mid] > nums[right]`, the minimum cannot be in the left half, so move `left = mid + 1`.
     - Otherwise, reduce the search space to the left half, including `mid`, by moving `right = mid`.

3. **Termination**:
   - When the loop ends (`left == right`), the `left` pointer will point to the minimum element.

---

### 5. Review

#### Debugging and Edge Cases

**Test Cases**:
1. **Basic Case**: `[4, 5, 6, 7, 0, 1, 2]`
   - Output: `0`
   - Explanation: The pivot is `0`, and it is the minimum.
2. **Already Sorted**: `[1, 2, 3, 4, 5]`
   - Output: `1`
   - Explanation: The array is not rotated, so the first element is the minimum.
3. **Single Element**: `[10]`
   - Output: `10`
   - Explanation: With only one element, it is the minimum.
4. **Two Elements**: `[2, 1]`
   - Output: `1`
   - Explanation: The pivot is `1`.
5. **Maximum Rotation**: `[2, 3, 4, 5, 1]`
   - Output: `1`
   - Explanation: The pivot is `1`, and it is the minimum.

#### Dry Run (Complex Case)

**Input**: `[4, 5, 6, 7, 0, 1, 2]`

1. **Initial**: `left = 0, right = 6`
   - `mid = 3`, `nums[mid] = 7`, `nums[right] = 2`
   - `nums[mid] > nums[right]` → `left = mid + 1 = 4`
2. **Iteration 2**: `left = 4, right = 6`
   - `mid = 5`, `nums[mid] = 1`, `nums[right] = 2`
   - `nums[mid] <= nums[right]` → `right = mid = 5`
3. **Iteration 3**: `left = 4, right = 5`
   - `mid = 4`, `nums[mid] = 0`, `nums[right] = 1`
   - `nums[mid] <= nums[right]` → `right = mid = 4`
4. **Termination**: `left = right = 4`
   - Return `nums[left] = 0`.

---

### 6. Evaluate

**Time Complexity**:  
- Each iteration reduces the search space by half.  
- Total complexity: O(log n).

**Space Complexity**:  
- Only constant space is used for the pointers.  
- Total complexity: O(1).

---

### Why This Problem is Important
- Real-World Applications:Solving problems with rotated or cyclic data (e.g., circular buffers, time-series data).
Useful in systems like databases or memory management.

- Interview Relevance:Tests your ability to adapt binary search for complex scenarios.
Focuses on edge-case handling and optimizing for O(log n) time complexity.

- Core Concepts:Teaches partial sorting, divide-and-conquer, and narrowing search ranges effectively.
Strengthens your understanding of how to approach non-traditional binary search problems.

- Skill Building:Develops pattern recognition and structured problem-solving.
Encourages clear communication of logic during coding interviews.

- Preparation for Advanced Problems:Builds a foundation for more challenging variations (e.g., duplicates, searching in rotated arrays).
Helps tackle dynamic programming and wrap-around conditions.

### Industry Relevance
- Rotated arrays appear frequently in system design and competitive programming, especially in problems involving circular buffers or version control systems.

### Prerequisites for Practicing This Problem
- Understanding of binary search.
- Familiarity with array traversal and pointer manipulation.

### Follow-up Practice Problems
1. **LeetCode 154**: Find Minimum in Rotated Sorted Array II (handles duplicates).
2. **LeetCode 33**: Search in Rotated Sorted Array.
3. **LeetCode 81**: Search in Rotated Sorted Array II.
