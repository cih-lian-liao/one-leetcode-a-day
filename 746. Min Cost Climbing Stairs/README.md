
# LeetCode 746: Min Cost Climbing Stairs

### Problem Statement

You are given an integer array `cost` where `cost[i]` is the cost of the `i-th` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return the minimum cost to reach the top of the floor.

#### Example

**Input**: `cost = [10, 15, 20]`  
**Output**: `15`  
**Explanation**: Start at index 1 and pay 15. Climb two steps to reach the top.

**Input**: `cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]`  
**Output**: `6`  
**Explanation**: Start at index 0. Climb to index 2, 3, and 4. Then climb to index 6, 7, and 9. Pay costs 1 + 1 + 1 + 1 + 1 = 6.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**:
  - `cost`: A list of integers representing the cost of each step.
- **Output**:
  - An integer representing the minimum cost to reach the top of the staircase.
- **Constraints**:
  - `2 <= cost.length <= 1000`
  - `0 <= cost[i] <= 999`

---

### 2. Match

This problem can be categorized under **Dynamic Programming** because:
- It involves making optimal decisions (choosing the minimal cost path).
- The solution depends on previous computations (subproblems).

The recurrence relation:
- To calculate the minimum cost for each step `i`:  
  `cost[i] += min(cost[i + 1], cost[i + 2])`

---

### 3. Plan

1. Append a zero at the end of the `cost` array to represent the "top of the staircase."
2. Iterate backwards through the array from the second-to-last step, updating each step with the minimal cost of climbing one or two steps ahead.
3. Return the minimum of the first two steps (`cost[0]` or `cost[1]`).

**Pseudocode**:
```plaintext
Append 0 to cost
For i from len(cost) - 3 to 0:
    cost[i] += min(cost[i + 1], cost[i + 2])
Return min(cost[0], cost[1])
```

---

### 4. Implement

Here is the Python implementation of the solution:

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        # Append a zero to represent the top of the staircase
        cost.append(0)

        # Iterate backward from the second-to-last step
        for i in range(len(cost) - 3, -1, -1):
            cost[i] += min(cost[i + 1], cost[i + 2])

        # Return the minimum cost of the first two steps
        return min(cost[0], cost[1])
```

### Step-by-Step Explanation:

1. **Initialization**: Append a `0` to `cost` since reaching the top requires no additional cost.
2. **Backward Iteration**:
   - Start from the step `len(cost) - 3` (third-to-last) and iterate to `0`.
   - For each step `i`, update `cost[i]` with the minimum cost of reaching `i+1` or `i+2`.
3. **Final Decision**: Return the smaller value between `cost[0]` and `cost[1]`.

---

### 5. Review

**Edge Cases**:
1. `cost = [0, 0]`: Directly return `0`.
2. `cost = [10, 15, 20]`: Correct output is `15`.

**Dry Run Example**:

Input: `cost = [10, 15, 20]`  
**Step-by-Step Execution**:
1. Append `0`: `cost = [10, 15, 20, 0]`
2. Update:
   - `i = 1`: `cost[1] = 15 + min(20, 0) = 15`
   - `i = 0`: `cost[0] = 10 + min(15, 20) = 25`
3. Result: `min(cost[0], cost[1]) = min(25, 15) = 15`

Output: `15`

---

### 6. Evaluate

**Time Complexity**:  
- Iterating through the array once: O(n)

**Space Complexity**:  
- Modifying the input array in place: O(1)

---

### Additional Notes

#### Why This Problem is Important:
- Teaches the fundamentals of dynamic programming.
- Highlights the importance of breaking down problems into subproblems.

#### Prerequisites for Practicing:
- Understanding arrays and basic iteration.
- Familiarity with dynamic programming concepts.

#### Industry Relevance:
- Problems like this often appear in real-world scenarios, such as cost optimization and planning.

#### Follow-up Practice Problems:
1. LeetCode 70: Climbing Stairs
2. LeetCode 198: House Robber
3. LeetCode 64: Minimum Path Sum
