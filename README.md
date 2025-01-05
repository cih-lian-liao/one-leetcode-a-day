
# 70: Climbing Stairs

### Problem Statement

You are climbing a staircase. It takes `n` steps to reach the top.  
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?  

### Inputs:
- `n`: an integer, the total number of steps in the staircase. (1 <= n <= 45)

### Outputs:
- An integer representing the number of distinct ways to climb the staircase.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- The problem asks us to determine the number of distinct ways to reach the top of a staircase.
- You can climb 1 or 2 steps at a time.
- Constraints ensure `n` is relatively small, which means solutions with `O(n)` time complexity are acceptable.
- Base cases:
  - If `n = 1`, there is only 1 way to climb (1 step).
  - If `n = 2`, there are 2 ways to climb (1+1 or 2 steps).

---

### 2. Match
- This is a classic **Dynamic Programming** problem.
- Similar to the Fibonacci sequence where:
  - `dp[i]` = number of ways to reach step `i`.
  - Transition relation: `dp[i] = dp[i-1] + dp[i-2]`.
- We can optimize space using a **sliding window** technique instead of storing the entire `dp` array.

---

### 3. Plan
#### Steps:
1. **Base Cases**:
   - If `n = 1`, return 1.
   - If `n = 2`, return 2.
2. **Iterative Calculation**:
   - Use two variables (`prev1` and `prev2`) to represent the last two computed values.
   - Iterate from step 3 to step `n`.
   - For each step, compute the current number of ways using the relation: `current = prev1 + prev2`.
   - Update `prev1` and `prev2`.
3. **Return Result**:
   - Return `prev2`, which represents the ways to reach step `n`.

#### Pseudocode:
```
If n is 1, return 1.
If n is 2, return 2.
Initialize prev1 = 1, prev2 = 2.
For i from 3 to n:
    current = prev1 + prev2
    Update prev1 = prev2
    Update prev2 = current
Return prev2.
```

---

### 4. Implement
```python
def climbStairs(n):
    # Base cases
    if n == 1:
        return 1
    if n == 2:
        return 2

    # Initialize variables for sliding window approach
    prev1, prev2 = 1, 2

    # Iteratively calculate the number of ways
    for _ in range(3, n + 1):
        current = prev1 + prev2
        prev1, prev2 = prev2, current

    return prev2
```

#### Step-by-Step Explanation:
1. **Base Cases**:
   - For `n = 1`, directly return `1`.
   - For `n = 2`, directly return `2`.
2. **Initialize**:
   - Set `prev1` = 1 (ways to step 1).
   - Set `prev2` = 2 (ways to step 2).
3. **Iterative Loop**:
   - Start from step 3 and iterate up to `n`.
   - Calculate `current` as the sum of the last two values (`prev1` and `prev2`).
   - Update `prev1` to `prev2` (shift the window).
   - Update `prev2` to `current` (shift the window).
4. **Return**:
   - After the loop, `prev2` contains the result for step `n`.

---

### 5. Review
#### Test Cases
**Example 1**:
- Input: `n = 3`
- Process:
  - Base: `prev1 = 1`, `prev2 = 2`
  - Iteration: `current = 1 + 2 = 3`
- Output: `3`

**Example 2**:
- Input: `n = 5`
- Process:
  - Base: `prev1 = 1`, `prev2 = 2`
  - Iteration 1: `current = 1 + 2 = 3`
  - Iteration 2: `current = 2 + 3 = 5`
  - Iteration 3: `current = 3 + 5 = 8`
- Output: `8`

#### Dry Run
For `n = 4`:
1. Initialize: `prev1 = 1`, `prev2 = 2`.
2. Step 3: `current = 1 + 2 = 3`, update `prev1 = 2`, `prev2 = 3`.
3. Step 4: `current = 2 + 3 = 5`, update `prev1 = 3`, `prev2 = 5`.
4. Return: `prev2 = 5`.

---

### 6. Evaluate
#### Time Complexity
- **O(n)**: We iterate from step 3 to `n`.

#### Space Complexity
- **O(1)**: Only two variables are used (`prev1` and `prev2`).

---

### Additional Notes

#### Why This Problem is Important
- Teaches fundamental concepts of dynamic programming.
- Frequently asked in technical interviews.
- Demonstrates the use of space optimization techniques.

#### Prerequisites for Practicing This Problem
- Familiarity with loops and arrays.
- Understanding of dynamic programming.

#### Industry Relevance
- Dynamic programming problems like this appear in real-world scenarios such as optimization problems and predictive algorithms.

#### Follow-Up Practice Problems
1. **198. House Robber**: Similar dynamic programming concept.
2. **120. Triangle**: More complex dynamic programming.
3. **746. Min Cost Climbing Stairs**: Variation of this problem.
