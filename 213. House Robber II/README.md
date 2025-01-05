
# LeetCode 213: House Robber II

### Problem Statement
You are a robber planning to rob houses along a street. Each house has a certain amount of money stashed, and all houses form a circle. This means the first house is the neighbor of the last one. Given an integer array `nums` representing the amount of money in each house, return the maximum amount of money you can rob tonight without alerting the police.

### Constraints:
1. 1 <= nums.length <= 100
2. 0 <= nums[i] <= 1000

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs:** 
  - An array `nums` of integers where `nums[i]` represents the amount of money in the `i-th` house.
  
- **Outputs:** 
  - An integer representing the maximum amount of money that can be robbed.

- **Constraints and Clarifications:** 
  - The houses form a **circular arrangement**, so robbing the first house excludes the possibility of robbing the last house and vice versa.
  - If there's only one house, return its value directly.

- **Assumptions:** 
  - The problem guarantees non-negative integers in the input array.

---

### 2. Match

This is a variation of the classic **House Robber I** problem. The key difference is the circular arrangement of houses.

- **Pattern Identified:** Dynamic Programming
  - Use a similar approach to House Robber I, but solve the problem twice:
    1. Exclude the first house and consider the rest.
    2. Exclude the last house and consider the rest.
  - Take the maximum of these two results.

---

### 3. Plan

1. **Subproblem:** Use a helper function to solve the linear version of the problem.
   - This function calculates the maximum money that can be robbed from a range of houses.
   - Transition relation: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

2. **Main Logic:**
   - If there is only one house, return its value.
   - Otherwise:
     1. Call the helper function on `nums[1:]` (exclude the first house).
     2. Call the helper function on `nums[:-1]` (exclude the last house).
     3. Return the maximum of these two values.

3. **Edge Cases:**
   - `nums` has only one house: Return `nums[0]`.
   - `nums` has two houses: Return `max(nums[0], nums[1])`.

4. **Time Complexity:** 
   - Each call to the helper function takes O(n), and there are two calls. Therefore, the overall complexity is O(n).

5. **Space Complexity:**
   - The solution uses two variables (`rob1` and `rob2`) in the helper function, so the space complexity is O(1).

---

### 4. Implement

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # Helper function to solve the linear house robber problem
        def helper(nums):
            rob1, rob2 = 0, 0  # Initialize variables for DP
            for n in nums:
                new_rob = max(rob1 + n, rob2)  # Decide whether to rob the current house
                rob1 = rob2  # Shift variables
                rob2 = new_rob
            return rob2

        # Base case: If there's only one house
        if len(nums) == 1:
            return nums[0]

        # Compare two scenarios: excluding the first or the last house
        return max(helper(nums[:-1]), helper(nums[1:]))
```

**Implementation Notes:**
- **`helper(nums)` Function:**
  - Iteratively computes the maximum money that can be robbed from a linear arrangement of houses using two variables to save space.
- The main function handles the circular logic by splitting the problem into two linear subproblems.

---

### 5. Review

#### Dry Run:

Input: `nums = [2, 3, 2]`

1. **Exclude the first house (`nums[1:] = [3, 2]`):**
   - Initialize `rob1 = 0`, `rob2 = 0`.
   - Iterate:
     - House 1: `n = 3`, `new_rob = max(0 + 3, 0) = 3`. Update: `rob1 = 0`, `rob2 = 3`.
     - House 2: `n = 2`, `new_rob = max(3 + 2, 3) = 3`. Update: `rob1 = 3`, `rob2 = 3`.
   - Result: `rob2 = 3`.

2. **Exclude the last house (`nums[:-1] = [2, 3]`):**
   - Initialize `rob1 = 0`, `rob2 = 0`.
   - Iterate:
     - House 1: `n = 2`, `new_rob = max(0 + 2, 0) = 2`. Update: `rob1 = 0`, `rob2 = 2`.
     - House 2: `n = 3`, `new_rob = max(2 + 3, 2) = 3`. Update: `rob1 = 2`, `rob2 = 3`.
   - Result: `rob2 = 3`.

3. Compare the results: `max(3, 3) = 3`.

Output: `3`

#### Test Cases:

| Input          | Output |
|----------------|--------|
| [2, 3, 2]      | 3      |
| [1, 2, 3, 1]   | 4      |
| [1, 2, 3]      | 3      |
| [5]            | 5      |
| [1, 1, 1, 1]   | 2      |

---

### 6. Evaluate

#### Time Complexity:
- Each call to `helper(nums)` takes O(n).
- Since there are two calls, the total complexity is O(n).

#### Space Complexity:
- The helper function uses constant space, so the total complexity is O(1).

#### Optimizations:
- The solution is already optimized in terms of space and time.

---

### Additional Notes

#### Why This Problem Is Important:
- It teaches how to adapt linear DP problems to circular constraints.
- It introduces optimization techniques for space efficiency.

#### Prerequisites for Practicing This Problem:
- Familiarity with basic Dynamic Programming.
- Understanding the `House Robber I` problem.

#### Industry Relevance:
- Similar patterns are used in scheduling problems and resource allocation.

#### Follow-Up Practice Problems:
- LeetCode 198: House Robber I
- LeetCode 337: House Robber III
- LeetCode 256: Paint House
- LeetCode 70: Climbing Stairs
