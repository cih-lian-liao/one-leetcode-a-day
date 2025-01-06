
# LeetCode 256: Paint House

### Problem Statement

You are given an array `costs` where `costs[i]` is an array of three integers:
- `costs[i][0]` is the cost of painting the ith house red,
- `costs[i][1]` is the cost of painting the ith house blue,
- `costs[i][2]` is the cost of painting the ith house green.

Return the minimum cost to paint all the houses such that no two adjacent houses have the same color.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs:**
- A 2D array `costs` of size `n x 3` where `n` is the number of houses.

**Outputs:**
- An integer representing the minimum cost to paint all houses under the given conditions.

**Constraints:**
1. No two adjacent houses can have the same color.
2. Each house must be painted with one of the three colors.
3. The number of houses `n` can range from 1 to a large value (e.g., 10^4).

**Assumptions:**
- The array `costs` is not empty.
- Each cost value is a non-negative integer.

**Clarifications:**
- If there is only one house, return the minimum cost among the three colors for that house.
- If no house is given (empty array), return 0.

---

### 2. Match

This is a **Dynamic Programming** problem:
- Use a rolling array to store intermediate results.
- The key observation is that the cost of painting the current house depends on the cost of painting the previous house with different colors.

**Patterns/Algorithms:**
- Transition relation: Use the minimum cost of the other two colors from the previous house to determine the cost for the current house.

---

### 3. Plan

1. Initialize a DP array `dp = [0, 0, 0]` to represent the minimum costs for the previous house.
2. Iterate through each house:
   - Calculate the minimum costs for the current house for all three colors using the transition relation.
   - Update the `dp` array with the new costs.
3. Return the minimum value from the DP array after iterating through all houses.

---

### 4. Implement

```python
class Solution:
    def minCost(self, costs: List[List[int]]) -> int:
        # Step 1: Handle the edge case where costs are empty
        if not costs:
            return 0

        # Step 2: Initialize DP array with zeros
        dp = [0, 0, 0]

        # Step 3: Iterate through each house
        for i in range(len(costs)):
            # Calculate the new DP values for the current house
            dp0 = costs[i][0] + min(dp[1], dp[2])  # Cost for red
            dp1 = costs[i][1] + min(dp[0], dp[2])  # Cost for blue
            dp2 = costs[i][2] + min(dp[0], dp[1])  # Cost for green

            # Update DP array
            dp = [dp0, dp1, dp2]

        # Step 4: Return the minimum value from the DP array
        return min(dp)
```

**Implementation Notes:**
- `dp0`, `dp1`, and `dp2` store temporary costs for red, blue, and green respectively.
- The `dp` array is updated after processing each house to save space.

---

### 5. Review

#### Debugging:
1. Test with edge cases:
   - **Empty costs:** `costs = []` → Expected: `0`
   - **One house:** `costs = [[10, 20, 30]]` → Expected: `10`

2. Dry-run example:

**Input:**
```python
costs = [
    [17, 2, 17],
    [16, 16, 5],
    [14, 3, 19]
]
```

**Execution:**
- Initialize: `dp = [0, 0, 0]`
- **First house:**
  - `dp0 = 17 + min(0, 0) = 17`
  - `dp1 = 2 + min(0, 0) = 2`
  - `dp2 = 17 + min(0, 0) = 17`
  - `dp = [17, 2, 17]`
- **Second house:**
  - `dp0 = 16 + min(2, 17) = 18`
  - `dp1 = 16 + min(17, 17) = 33`
  - `dp2 = 5 + min(17, 2) = 7`
  - `dp = [18, 33, 7]`
- **Third house:**
  - `dp0 = 14 + min(33, 7) = 21`
  - `dp1 = 3 + min(18, 7) = 10`
  - `dp2 = 19 + min(18, 33) = 37`
  - `dp = [21, 10, 37]`
- Return: `min(21, 10, 37) = 10`

**Output:**
```python
10
```

---

### 6. Evaluate

**Time Complexity:**  
- O(n): Iterates through all houses once.

**Space Complexity:**  
- O(1): Uses a constant-sized DP array.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It teaches how to optimize space in dynamic programming problems.
- Demonstrates a rolling array technique for 1D DP.

#### Prerequisites for Practicing This Problem
1. Understanding of dynamic programming basics.
2. Familiarity with rolling arrays for space optimization.

#### Industry Relevance
- Cost optimization problems appear in real-world applications like budgeting, scheduling, and resource allocation.

#### Follow-up Practice Problems
1. LeetCode 265: **Paint House II** (Supports k colors).
2. LeetCode 198: **House Robber** (Linear DP with constraints).
3. LeetCode 120: **Triangle** (Triangle DP optimization).
4. LeetCode 64: **Minimum Path Sum** (Grid DP).
