

# LeetCode Problem 918: Maximum Sum Circular Subarray

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**
The problem asks us to find the maximum sum of a subarray, where the subarray can either be non-circular or circular.

**Inputs:**
- `nums`: A list of integers (e.g., `[-2, 4, -1, 4, -1, 3]`).

**Outputs:**
- An integer representing the maximum sum of the subarray.

**Constraints:**
- `1 <= nums.length <= 3 * 10^4`
- `-3 * 10^4 <= nums[i] <= 3 * 10^4`

**Clarifications:**
- A circular subarray can wrap around the edges of the array (e.g., in `[-2, 4, -1, 4, -1, 3]`, `[3, -2, 4, -1]` is circular).
- If all elements are negative, the largest subarray sum is the maximum single element.

---

### **2. Match**
This problem is a variation of the **maximum subarray sum** problem, solvable using **Kadane's Algorithm**. To handle circular subarrays:
1. Find the maximum sum of non-circular subarrays using Kadane’s algorithm.
2. Compute the total array sum and subtract the minimum sum of non-circular subarrays (using Kadane’s algorithm to find the minimum).
3. Compare the two values: `max(non-circular sum, circular sum)`.

If all elements are negative, the maximum subarray is the largest single element.

---

### **3. Plan**
We’ll break the solution into these steps:

1. Initialize variables for:
   - `globMax` and `globMin`: Track the global maximum and minimum sums.
   - `curMax` and `curMin`: Track the current maximum and minimum sums during iteration.
   - `total`: Track the sum of all elements.

2. Traverse the array:
   - Update `curMax` and `curMin` using Kadane's logic.
   - Update `globMax` and `globMin` as the global values.
   - Update `total` with the running sum of the array.

3. Compute the result:
   - If `globMax > 0`, return `max(globMax, total - globMin)` (considering both non-circular and circular subarrays).
   - Otherwise, return `globMax` (if all elements are negative).

**Pseudocode:**
```
Initialize globMax, globMin = nums[0], nums[0]
Initialize curMax, curMin = 0, 0
Initialize total = 0

For each number n in nums:
    Update curMax = max(curMax + n, n)
    Update curMin = min(curMin + n, n)
    Update total += n
    Update globMax = max(globMax, curMax)
    Update globMin = min(globMin, curMin)

If globMax > 0:
    Return max(globMax, total - globMin)
Else:
    Return globMax
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### **4. Implement**
```python
from typing import List

class Solution:
    def maxSubarraySumCircular(self, nums: List[int]) -> int:
        # Initialize variables
        globMax, globMin = nums[0], nums[0]
        curMax, curMin = 0, 0
        total = 0

        # Iterate over the array
        for n in nums:
            # Update the current maximum subarray sum
            curMax = max(curMax + n, n)
            # Update the current minimum subarray sum
            curMin = min(curMin + n, n)
            # Add current number to total
            total += n
            # Update global maximum
            globMax = max(globMax, curMax)
            # Update global minimum
            globMin = min(globMin, curMin)

        # Return result based on global max value
        return max(globMax, total - globMin) if globMax > 0 else globMax
```

**Implementation Notes:**
- `curMax` and `curMin` dynamically adjust as we traverse the array, ensuring efficient computation.
- `globMax` and `globMin` track the best results found so far.
- The final result considers the edge case where all elements are negative.

---

### **5. Review**
#### Debugging and Testing:
**Test Case 1:**  
Input: `nums = [1, -2, 3, -2]`  
Expected Output: `3`  
**Explanation:** Non-circular maximum subarray is `[3]`.

**Test Case 2:**  
Input: `nums = [5, -3, 5]`  
Expected Output: `10`  
**Explanation:** Circular subarray is `[5, 5]`.

**Test Case 3:**  
Input: `nums = [-3, -2, -3]`  
Expected Output: `-2`  
**Explanation:** All elements are negative; the result is the largest single element.

**Complex Case Dry Run:**  
Input: `nums = [3, -1, 2, -1]`  
- `curMax` evolves as `[3, 2, 4, 3]` → `globMax = 4`.  
- `curMin` evolves as `[3, -1, 1, -1]` → `globMin = -1`.  
- `total = 3 + (-1) + 2 + (-1) = 3`.  
- Circular max = `total - globMin = 3 - (-1) = 4`.  
- Result: `max(4, 4) = 4`.

---

### **6. Evaluate**
#### Complexity Analysis:
- **Time Complexity:** O(n)  
  - Single traversal of the array.
- **Space Complexity:** O(1)  
  - Constant space usage.

#### Optimizations:
This approach is already optimal for this problem, as it uses linear time and constant space.

---

### **Additional Notes**

#### **Why This Problem is Important**
- Teaches an extension of Kadane's Algorithm, a fundamental algorithm for array problems.
- Introduces the concept of handling circular conditions efficiently.

#### **Prerequisites**
- Familiarity with Kadane’s Algorithm.
- Understanding of dynamic programming principles for maximum/minimum subarray sums.

#### **Industry Relevance**
- Common in interviews to test array manipulation and algorithmic thinking.
- Similar problems occur in handling cyclic data structures like buffers or time-based arrays.

#### **Follow-up Practice Problems**
1. LeetCode 53: Maximum Subarray
2. LeetCode 152: Maximum Product Subarray
3. LeetCode 62: Unique Paths (Dynamic Programming)
