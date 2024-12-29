
# LeetCode 724: Find Pivot Index

### **Problem Statement**

Given an array of integers `nums`, return the *pivot index* of this array. The pivot index is the index where the sum of all numbers strictly to the left of the index is equal to the sum of all numbers strictly to the right of the index.

If no such index exists, return `-1`. If there are multiple pivot indexes, return the smallest one.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

- **Input**: An array of integers, `nums`.
- **Output**: The smallest pivot index, or `-1` if no such index exists.
- **Constraints**:
  - `1 <= nums.length <= 10^4`
  - `-1000 <= nums[i] <= 1000`
- **Clarifications**:
  - Pivot index is determined based on left and right sums.
  - Elements at the pivot index are **not included** in either left or right sums.

---

### **2. Match**

- This problem relates to the **Prefix Sum** pattern.
- We calculate cumulative sums dynamically to avoid recomputation, allowing for efficient comparison of left and right sums.
- Data structures:
  - A single variable for `left_sum` to track the cumulative sum on the left side.

---

### **3. Plan**

1. Compute the total sum of the array.
2. Initialize `left_sum` as 0.
3. Iterate through the array:
   - For each index `i`, compute `right_sum` as: ` right_sum = total − nums[i] − left_sum `
   - Check if `left_sum == right_sum`. If true, return `i`.
   - Update `left_sum` by adding the current element (`nums[i]`).
4. If no pivot index is found after the iteration, return `-1`.

---

### **4. Implement**

```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        # Step 1: Compute the total sum of the array
        total = sum(nums)
        
        # Step 2: Initialize the left sum to 0
        left_sum = 0
        
        # Step 3: Iterate through the array
        for i in range(len(nums)):
            # Calculate right sum dynamically
            right_sum = total - nums[i] - left_sum
            
            # Check if left and right sums are equal
            if left_sum == right_sum:
                return i
            
            # Update left sum with the current element
            left_sum += nums[i]
        
        # Step 4: If no pivot index is found, return -1
        return -1
```

**Implementation Notes**:
- **Line 1**: Calculate the total sum of the array to determine the `right_sum` dynamically during iteration.
- **Line 4-10**: The loop iterates through each index, dynamically calculating the left and right sums to find the pivot.
- **Line 7**: If the left and right sums are equal, the index is returned immediately.
- **Line 11**: After completing the loop, if no pivot is found, return `-1`.

---

### **5. Review**

#### Debugging and Edge Case Testing

Test the implementation with several cases:

1. **Example 1**:  
   Input: `[1, 7, 3, 6, 5, 6]`  
   Steps:
   - Total sum = 28
   - Iterations:
     - i=0: left_sum=0, right_sum=27 → not equal.
     - i=1: left_sum=1, right_sum=20 → not equal.
     - i=2: left_sum=8, right_sum=17 → not equal.
     - i=3: left_sum=11, right_sum=11 → equal! Return `3`.
   Output: `3`

2. **Example 2**:  
   Input: `[1, 2, 3]`  
   Steps:
   - Total sum = 6
   - Iterations:
     - i=0: left_sum=0, right_sum=5 → not equal.
     - i=1: left_sum=1, right_sum=3 → not equal.
     - i=2: left_sum=3, right_sum=0 → not equal.
   Output: `-1`

3. **Edge Case 1**:  
   Input: `[2, 1, -1]`  
   Steps:
   - Total sum = 2
   - Iterations:
     - i=0: left_sum=0, right_sum=0 → equal! Return `0`.
   Output: `0`

4. **Edge Case 2**:  
   Input: `[0]`  
   Steps:
   - Total sum = 0
   - i=0: left_sum=0, right_sum=0 → equal! Return `0`.
   Output: `0`

---

### **6. Evaluate**

#### **Time Complexity**
- Calculating the total sum: O(n)
- Iterating through the array: O(n)
- Total: O(n)

#### **Space Complexity**
- Only a few variables (`total`, `left_sum`, `right_sum`) are used: O(1)

#### **Optimizations**
- This is already the optimal solution for this problem due to the linear time complexity and constant space usage.

---

### **Additional Notes**

#### **The Reason Why This Problem is Important**
- It emphasizes the efficient use of **Prefix Sum**.
- It develops skills in dynamically tracking cumulative sums.

#### **Prerequisites for Practicing This Problem**
- Understanding basic array manipulation.
- Familiarity with prefix sums and iterative approaches.

#### **Industry Relevance**
- Used in scenarios where balancing data is critical (e.g., load balancing, distributed systems).

#### **Follow-up Practice Problems**
1. [LeetCode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
2. [LeetCode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
3. [LeetCode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
