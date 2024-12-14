
# LeetCode Problem 45: Jump Game II

### **Problem Description**
You are given a 0-indexed array `nums` of integers where `nums[i]` represents the maximum number of steps you can jump forward from that position. Your goal is to reach the last index in the minimum number of jumps. You can assume that you can always reach the last index.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

**Inputs:**
- An array `nums` of size `n` where `1 <= n <= 10^4` and each element satisfies `0 <= nums[i] <= 10^5`.

**Outputs:**
- An integer representing the minimum number of jumps to reach the last index.

**Constraints:**
- You are guaranteed that it is always possible to reach the last index.
- Optimize for time complexity as much as possible.

**Clarifications:**
- We want the fewest number of jumps, not the shortest distance.
- Jumps can overlap and use the maximum range possible.

---

### **2. Match**

**Pattern:**
- This is a greedy problem. It involves iterating through the array and determining the farthest range reachable at each step.

**Algorithm/Techniques:**
- Greedy Algorithm: Maximize the farthest position reachable in each jump.
- Breadth-first traversal over the range of indices within the jump.

---

### **3. Plan**

**Step-by-Step Plan:**
1. Initialize:
   - `jumps` to `0` (number of jumps made).
   - `current_end` to `0` (current jump's farthest reach).
   - `farthest` to `0` (global farthest reach).
2. Iterate through the array up to `n-1`:
   - For each index `i`, calculate the farthest position reachable as `farthest = max(farthest, i + nums[i])`.
   - If `i` reaches `current_end`, increment `jumps` and set `current_end = farthest`.
   - If `current_end` exceeds or equals the last index, exit the loop.
3. Return `jumps` as the result.

**Time Complexity:**
- O(n): Single pass through the array.

**Space Complexity:**
- O(1): Only a few variables are used.

---

### **4. Implement**

```python
class Solution:
    def jump(self, nums):
        # Initialize variables
        jumps = 0  # Number of jumps made
        current_end = 0  # Current jump's farthest reach
        farthest = 0  # Farthest position reachable globally

        # Iterate over the array (excluding the last index)
        for i in range(len(nums) - 1):
            # Update the farthest position reachable
            farthest = max(farthest, i + nums[i])
            
            # If we reach the end of the current jump range
            if i == current_end:
                jumps += 1  # Increment jump count
                current_end = farthest  # Update current range to the farthest reachable
                
                # Exit early if we can already reach the end
                if current_end >= len(nums) - 1:
                    break

        return jumps
```

**Implementation Notes:**
- The variable `farthest` ensures we maximize the range in each jump.
- The condition `if i == current_end` ensures we only jump when needed.
- Early exit ensures efficiency for large inputs.

---

### **5. Review**

**Testing Edge Cases:**
1. Minimal Input:
   - Input: `[0]`
   - Expected Output: `0`
   - Reason: Already at the last index.
2. All Steps Maximize Reach:
   - Input: `[4, 1, 1, 1, 1]`
   - Expected Output: `1`
   - Reason: The first step covers all indices.
3. Small Steps:
   - Input: `[1, 1, 1, 1, 1]`
   - Expected Output: `4`
   - Reason: Each step covers only one position.

**Complex Case Dry Run:**
Input: `[2, 3, 1, 1, 4]`

Step-by-Step Execution:
1. Start at index `0`:
   - `farthest = max(0, 0 + nums[0]) = 2`.
   - `current_end = 0`, so increment `jumps = 1` and set `current_end = 2`.
2. At index `1`:
   - `farthest = max(2, 1 + nums[1]) = 4`.
   - No jump yet since `i != current_end`.
3. At index `2`:
   - `farthest = max(4, 2 + nums[2]) = 4`.
   - `i == current_end = 2`, so increment `jumps = 2` and set `current_end = 4`.
4. At this point, `current_end` exceeds the last index. Exit.

Output: `2`

---

### **6. Evaluate**

**Time Complexity:**
- O(n): A single iteration over the array.

**Space Complexity:**
- O(1): No additional data structures are used.

**Optimizations:**
- This solution is already optimal for this problem’s constraints.

---

### **Additional Notes**

#### **Why This Problem is Important:**
- Teaches greedy algorithms and their application in range coverage problems.
- Frequently asked in coding interviews to test problem-solving and optimization skills.

#### **Prerequisites:**
- Understanding of greedy algorithms.
- Familiarity with array traversal and optimization techniques.

#### **Industry Relevance:**
- Range coverage problems are common in dynamic programming, networking, and resource allocation tasks.

#### **Follow-up Practice Problems:**
1. [LeetCode 55: Jump Game](https://leetcode.com/problems/jump-game/)
2. [LeetCode 134: Gas Station](https://leetcode.com/problems/gas-station/)
3. [LeetCode 1306: Jump Game III](https://leetcode.com/problems/jump-game-iii/)
