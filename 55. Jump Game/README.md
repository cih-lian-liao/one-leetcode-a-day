# LeetCode Problem 55: Jump Game

### Problem Statement

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

#### Example 1:
**Input:**  
`nums = [2, 3, 1, 1, 4]`  
**Output:**  
`true`  
**Explanation:**  
Jump 1 step from index 0 to 1, then 3 steps to the last index.

#### Example 2:
**Input:**  
`nums = [3, 2, 1, 0, 4]`  
**Output:**  
`false`  
**Explanation:**  
You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

#### Constraints:
- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 10^5`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**
- **Inputs:**  
  - `nums`: An array of non-negative integers where `nums[i]` represents the maximum number of steps you can jump forward from index `i`.
- **Outputs:**  
  - Return `true` if it is possible to reach the last index; otherwise, return `false`.
- **Constraints:**  
  - `nums.length` can be large (up to 10,000).
  - `nums[i]` can also be large (up to 100,000).
- **Assumptions:**  
  - It is always possible to start at index 0.
  - The array may contain zeros, which means no jump is possible from that index.

---

### **2. Match**
This problem is related to:
1. **Greedy Algorithms:**  
   - Update the farthest reachable index as you traverse the array.
   - Check if the current index can be reached by comparing it with the farthest reachable index.
2. **Dynamic Programming (Optional):**  
   - Alternatively, we could use a DP approach to determine if each index is reachable, but it will not be optimal.

We will use the **Greedy Approach** for optimal performance, as it ensures O(n) time complexity and O(1) space complexity.

---

### **3. Plan**

**Plan using the Greedy Approach:**
1. Initialize `max_reach` to 0. This will store the farthest index you can currently reach.
2. Traverse the array using a for loop:
   - If the current index `i` exceeds `max_reach`, return `false` (since this means the index is not reachable).
   - Otherwise, update `max_reach = max(max_reach, i + nums[i])`.
   - If `max_reach` is greater than or equal to the last index, return `true`.
3. If the loop ends and `max_reach` is still less than the last index, return `false`.

#### **Pseudocode:**
```python
Initialize max_reach = 0
For each index i from 0 to len(nums) - 1:
    If i > max_reach:
        Return False
    Update max_reach = max(max_reach, i + nums[i])
    If max_reach >= len(nums) - 1:
        Return True
Return False
```

---

### **4. Implement**

```python
class Solution:
    def canJump(self, nums):
        # Initialize the farthest reachable index
        max_reach = 0
        
        # Iterate over each index
        for i in range(len(nums)):
            # If the current index is not reachable, return False
            if i > max_reach:
                return False
            
            # Update the farthest reachable index
            max_reach = max(max_reach, i + nums[i])
            
            # If we can reach or exceed the last index, return True
            if max_reach >= len(nums) - 1:
                return True
        
        # If the loop ends without returning True, return False
        return False
```

---

### **5. Review**

#### Debugging the Code:
Let’s dry-run the code with a complex test case.

**Test Case:** `nums = [2, 3, 1, 1, 4]`  
- Initialize `max_reach = 0`.
- **Index 0:** `i = 0`, `nums[0] = 2`, `max_reach = max(0, 0 + 2) = 2`.
- **Index 1:** `i = 1`, `nums[1] = 3`, `max_reach = max(2, 1 + 3) = 4`.
  - `max_reach >= len(nums) - 1`, so return `True`.

**Test Case:** `nums = [3, 2, 1, 0, 4]`  
- Initialize `max_reach = 0`.
- **Index 0:** `i = 0`, `nums[0] = 3`, `max_reach = max(0, 0 + 3) = 3`.
- **Index 1:** `i = 1`, `nums[1] = 2`, `max_reach = max(3, 1 + 2) = 3`.
- **Index 2:** `i = 2`, `nums[2] = 1`, `max_reach = max(3, 2 + 1) = 3`.
- **Index 3:** `i = 3`, `nums[3] = 0`, `max_reach = max(3, 3 + 0) = 3`.
- **Index 4:** `i = 4`, but `i > max_reach`, so return `False`.

---

### **6. Evaluate**

#### Time Complexity:
- **O(n):** We traverse the array once.

#### Space Complexity:
- **O(1):** We use a single integer `max_reach`.

#### Optimizations:
This is already optimal for time and space complexity.

---

### **Additional Notes**

#### The Reason Why This Problem is Important:
- This problem tests your understanding of **greedy algorithms** and your ability to optimize solutions for time and space complexity.

#### Prerequisites for Practicing This Problem:
1. Basic understanding of arrays and loops.
2. Familiarity with greedy algorithms.

#### Industry Relevance:
- Problems like this often appear in technical interviews as they test optimization and algorithmic thinking.

#### Follow-up Practice Problems:
1. [Jump Game II (LeetCode 45)](https://leetcode.com/problems/jump-game-ii/)  
2. [Minimum Number of Arrows to Burst Balloons (LeetCode 452)](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)  
3. [Can Place Flowers (LeetCode 605)](https://leetcode.com/problems/can-place-flowers/)

---

### Alternative Solution: Backward Greedy

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        goal = len(nums) - 1

        for i in range(len(nums) - 2, -1, -1):
            if i + nums[i] >= goal:
                goal = i
        
        return goal == 0
```

#### Explanation:
- **Goal Initialization:** Start with `goal = len(nums) - 1` (the last index).
- **Backward Iteration:** Iterate from the second-to-last index to the first.
  - If the current index `i` can jump to or beyond the current `goal`, update `goal` to `i`.
- **Final Check:** If `goal` is reduced to 0, it means the first index can eventually reach the last index. Otherwise, return `false`.

#### Time Complexity:
- **O(n):** We traverse the array once from right to left.

#### Space Complexity:
- **O(1):** Only the `goal` variable is used.

#### Why It's Effective:
- This solution is also greedy, but it simplifies the problem by working backward and ensuring the last index is reachable from previous indices.
