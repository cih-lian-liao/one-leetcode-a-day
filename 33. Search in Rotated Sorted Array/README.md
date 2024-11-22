
# LeetCode Problem 33: Search in Rotated Sorted Array

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### Problem Statement
You are given an integer array `nums` sorted in ascending order (with distinct values) that has been rotated at some pivot. You are also given an integer `target`. Write a function to search for `target` in `nums`. If `target` exists, return its index. Otherwise, return `-1`.

#### Inputs:
- `nums`: A list of integers (rotated, sorted in ascending order, no duplicates).
- `target`: An integer to search for in the array.

#### Outputs:
- Index of `target` if found, otherwise `-1`.

#### Constraints:
- 1 <= `nums.length` <= 5000
- -10000 <= `nums[i]`, target <= 10000
- All integers in `nums` are unique.

#### Assumptions:
- The array is rotated at an unknown pivot index.
- Binary search principles can still apply due to partial sorting.

---

### **2. Match**

#### Problem Type:
- Search problem with a twist: the input array is rotated.

#### Relevant Algorithm:
- Binary search:
  - Use binary search to reduce the search space efficiently.
  - Identify which part of the array is sorted (left or right) and adjust search bounds accordingly.

#### Suitable Data Structure:
- Array (with index-based access).

---

### **3. Plan**

#### Key Steps:
1. Initialize two pointers: `left` and `right`.
2. While `left` <= `right`, calculate `mid = (left + right) // 2`.
3. Check if `nums[mid] == target`. If so, return `mid`.
4. Determine if the left half of the array (`nums[left]` to `nums[mid]`) is sorted:
   - If sorted, check if `target` lies in this range. If yes, move `right` to `mid - 1`. Otherwise, move `left` to `mid + 1`.
5. Otherwise, the right half (`nums[mid]` to `nums[right]`) is sorted:
   - If sorted, check if `target` lies in this range. If yes, move `left` to `mid + 1`. Otherwise, move `right` to `mid - 1`.
6. If `target` is not found, return `-1`.

---

### **4. Implement**

#### Python Code with Step-by-Step Notes:
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # Step 1: Initialize pointers
        left, right = 0, len(nums) - 1

        # Step 2: Start binary search loop
        while left <= right:
            # Step 3: Calculate mid-point
            mid = (left + right) // 2
            
            # Step 4: Check if mid is the target
            if nums[mid] == target:
                return mid
            
            # Step 5: Determine if left half is sorted
            if nums[left] <= nums[mid]:
                # Target lies in the sorted left half
                if nums[left] <= target < nums[mid]:
                    right = mid - 1  # Narrow search to left half
                else:
                    left = mid + 1   # Narrow search to right half
            else:
                # Target lies in the sorted right half
                if nums[mid] < target <= nums[right]:
                    left = mid + 1   # Narrow search to right half
                else:
                    right = mid - 1  # Narrow search to left half

        # Step 6: Target not found
        return -1
```

---

### **5. Review**

#### Debugging:
- Test with small arrays and common edge cases.

#### Edge Cases:
1. Array has only one element:
   - Input: `nums = [1], target = 1`. Output: `0`.
   - Input: `nums = [1], target = 2`. Output: `-1`.
2. Target is at the rotation point:
   - Input: `nums = [6, 7, 1, 2, 3], target = 1`. Output: `2`.
3. Target is not in the array:
   - Input: `nums = [4, 5, 6, 7, 0, 1, 2], target = 10`. Output: `-1`.
4. Fully sorted array without rotation:
   - Input: `nums = [1, 2, 3, 4, 5], target = 3`. Output: `2`.

#### Dry Run Example:
Input: `nums = [4, 5, 6, 7, 0, 1, 2], target = 0`

1. Initialize: `left = 0`, `right = 6`.
2. Iteration 1:
   - `mid = (0 + 6) // 2 = 3`
   - `nums[mid] = 7`, left half `[4, 5, 6, 7]` is sorted.
   - Target `0` not in `[4, 5, 6, 7]`, so update `left = 4`.
3. Iteration 2:
   - `mid = (4 + 6) // 2 = 5`
   - `nums[mid] = 1`, left half `[0, 1]` is sorted.
   - Target `0` in `[0, 1]`, so update `right = 4`.
4. Iteration 3:
   - `mid = (4 + 4) // 2 = 4`
   - `nums[mid] = 0`, target found at index `4`.
5. Output: `4`.

---

### **6. Evaluate**

#### Time Complexity:
- O(log n): Binary search reduces the search space logarithmically.

#### Space Complexity:
- O(1): No additional space is used.

#### Optimization Notes:
- This solution is already optimal for the given constraints.
- Alternative solutions (like linear search) are less efficient, with time complexity O(n).

---

### **Additional Notes**

#### Why This Problem is Important:
- **Industry Relevance**: Searching in rotated arrays simulates real-world scenarios like searching in rotated logs or datasets.
- **Practical Skills**: Combines binary search knowledge with problem-solving under modified conditions.

#### Prerequisites for Practicing This Problem:
1. Understanding of binary search.
2. Familiarity with array traversal and conditions.

#### Follow-Up Practice Problems:
1. [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) - Handles duplicates.
2. [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
3. [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)
