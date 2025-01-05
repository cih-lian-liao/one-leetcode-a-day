
# LeetCode 198: House Robber

### Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, and the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected. If two adjacent houses are robbed, the system will alert the police.

Given an integer array `nums` representing the amount of money in each house, return the maximum amount of money you can rob without alerting the police.

### Example 1:

**Input:**  
`nums = [1, 2, 3, 1]`  
**Output:**  
`4`  
**Explanation:**  
Rob house 1 (money = 1) and house 3 (money = 3). Total = 1 + 3 = 4.

### Example 2:

**Input:**  
`nums = [2, 7, 9, 3, 1]`  
**Output:**  
`12`  
**Explanation:**  
Rob house 1 (money = 2), house 3 (money = 9), and house 5 (money = 1). Total = 2 + 9 + 1 = 12.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input:**  
  An array of non-negative integers `nums` where `nums[i]` represents the amount of money in the `i-th` house.
  
- **Output:**  
  An integer representing the maximum amount of money you can rob without robbing two adjacent houses.

- **Constraints:**  
  - `1 <= nums.length <= 100`
  - `0 <= nums[i] <= 400`
  - You cannot rob two adjacent houses.

**Clarifications:**  
If `nums` has only one house, the answer is the value of that house. If `nums` has two houses, the answer is the maximum of the two values.

---

### 2. Match

This problem fits the **Dynamic Programming** pattern:
- Subproblems: The decision at each house depends on whether to rob it or skip it.
- Transition relation: The result for the `i-th` house depends on results from the `(i-1)-th` and `(i-2)-th` houses.

---

### 3. Plan

#### Approach:
1. Use two variables (`prev2` and `prev1`) to store the results of the `(i-2)-th` and `(i-1)-th` houses respectively.
2. Iterate through the array starting from the 3rd house (`i = 2`).
3. At each step, calculate the maximum amount by:
   - Robbing the current house: `prev2 + nums[i]`.
   - Skipping the current house: `prev1`.
4. Update `prev2` and `prev1` for the next iteration.
5. Return `prev1` as the final result.

#### Pseudocode:
```
If nums.length == 1: return nums[0]
Initialize prev2 = nums[0]
Initialize prev1 = max(nums[0], nums[1])
For i from 2 to nums.length - 1:
    curr = max(prev1, prev2 + nums[i])
    prev2 = prev1
    prev1 = curr
Return prev1
```

---

### 4. Implement

```python
def rob(nums):
    # Handle edge cases
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]

    # Initialize variables
    prev2 = nums[0]  # Max money from house 1
    prev1 = max(nums[0], nums[1])  # Max money from house 1 or 2

    # Iterate through the houses
    for i in range(2, len(nums)):
        # Calculate the maximum money for the current house
        curr = max(prev1, prev2 + nums[i])
        # Update variables for the next house
        prev2 = prev1
        prev1 = curr

    # Return the maximum money robbed
    return prev1
```

#### Notes for Implementation:
1. **Edge Cases:**  
   - If the input is an empty list, return `0`.
   - If there’s only one house, return its value directly.

2. **Modularity:**  
   - Each calculation is simple, and variables are updated systematically.

3. **Maintainability:**  
   - Variables `prev2`, `prev1`, and `curr` are intuitive and clearly represent their purpose.

---

### 5. Review

#### Dry Run with Input `nums = [2, 7, 9, 3, 1]`:

| House | Money (`nums[i]`) | Rob (`prev2 + nums[i]`) | Skip (`prev1`) | Current Max (`curr`) | `prev2` | `prev1` |
|-------|--------------------|-------------------------|----------------|-----------------------|---------|---------|
| 1     | 2                  | 2                       | -              | 2                     | -       | 2       |
| 2     | 7                  | 7                       | 2              | 7                     | 2       | 7       |
| 3     | 9                  | 11                      | 7              | 11                    | 7       | 11      |
| 4     | 3                  | 10                      | 11             | 11                    | 11      | 11      |
| 5     | 1                  | 12                      | 11             | 12                    | 11      | 12      |

**Output:** `12`

#### Edge Cases:
1. `nums = [1]`  
   Output: `1`
2. `nums = [1, 2]`  
   Output: `2`

---

### 6. Evaluate

#### Time Complexity:
- Iterates through the array once: **O(n)**.

#### Space Complexity:
- Uses constant space for variables: **O(1)**.

#### Optimizations:
- No further optimization required; the solution is already optimal.

---

### Additional Notes

#### Why This Problem is Important:
- Teaches how to identify and solve dynamic programming problems.
- Introduces space optimization techniques by replacing arrays with variables.

#### Prerequisites:
- Basic understanding of dynamic programming.
- Familiarity with iterative approaches.

#### Industry Relevance:
- Useful in financial and resource allocation problems.
- Foundational knowledge for more advanced DP problems.

#### Follow-up Practice Problems:
1. LeetCode 213: House Robber II
2. LeetCode 740: Delete and Earn
3. LeetCode 62: Unique Paths

