
# 1137. N-th Tribonacci Number

### Problem Statement

The Tribonacci sequence T(n) is defined as follows:
- T(0) = 0, T(1) = 1, T(2) = 1
- For n >= 3, T(n) = T(n-1) + T(n-2) + T(n-3)

Given `n`, return the value of T(n).

### Example 1
**Input**: n = 4  
**Output**: 4  
**Explanation**: T(3) = 2, T(4) = T(3) + T(2) + T(1) = 2 + 1 + 1 = 4.

### Example 2
**Input**: n = 25  
**Output**: 1389537

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
1. **Problem Requirements**: Calculate the n-th number in the Tribonacci sequence.
2. **Input**: An integer `n` (0 <= n <= 37).
3. **Output**: An integer representing the Tribonacci number `T(n)`.
4. **Constraints**:
   - For `n <= 2`, directly return the corresponding value (0, 1, 1).
   - For `n >= 3`, calculate recursively based on the formula.
   - Keep the solution efficient for `n = 37`.

---

### 2. Match
1. **Problem Pattern**:
   - The problem involves recurrence and can be solved using:
     - Recursion (basic approach).
     - Memoization (to optimize recursion).
     - Dynamic Programming (iterative, bottom-up).
   - Space optimization can further improve the DP solution.

2. **Algorithms/Data Structures**:
   - Recursion with base cases.
   - HashMap (for memoization).
   - Arrays or variables (for iterative DP).

---

### 3. Plan

#### Solution 1: Recursive Solution
1. Base Cases:
   - If `n == 0`, return 0.
   - If `n == 1 or n == 2`, return 1.
2. Recursively calculate `T(n)` as:
   - `T(n) = T(n-1) + T(n-2) + T(n-3)`.

#### Solution 2: Memoized Recursion
1. Add a cache (dictionary) to store already computed results.
2. If `n` is in the cache, return it.
3. If not, compute `T(n)` recursively and store the result in the cache.

#### Solution 3: Iterative Dynamic Programming (Space Optimized)
1. Use three variables `t0`, `t1`, and `t2` to represent the last three Tribonacci numbers.
2. Iterate from `3` to `n`:
   - Compute the next Tribonacci number: `t3 = t0 + t1 + t2`.
   - Update `t0, t1, t2` for the next iteration.
3. Return `t3` as the result.

---

### 4. Implement

#### Solution 1: Recursive Solution
```python
def tribonacci_recursive(n):
    if n == 0:
        return 0
    elif n == 1 or n == 2:
        return 1
    return tribonacci_recursive(n-1) + tribonacci_recursive(n-2) + tribonacci_recursive(n-3)
```
**Step-by-Step Explanation**:
1. Check if `n` matches a base case (0, 1, 2).
2. Otherwise, recursively calculate `T(n)` using the formula.

---

#### Solution 2: Memoized Recursion
```python
def tribonacci_memoized(n, memo={}):
    if n in memo:
        return memo[n]
    if n == 0:
        return 0
    elif n == 1 or n == 2:
        return 1
    memo[n] = tribonacci_memoized(n-1, memo) + tribonacci_memoized(n-2, memo) + tribonacci_memoized(n-3, memo)
    return memo[n]
```
**Step-by-Step Explanation**:
1. Use a dictionary `memo` to store already computed results.
2. Check if `n` is in the dictionary; if yes, return the value.
3. Compute the value if not found, store it in `memo`, and return.

---

#### Solution 3: Iterative Dynamic Programming (Space Optimized)
```python
def tribonacci_dp(n):
    if n == 0:
        return 0
    elif n == 1 or n == 2:
        return 1

    t0, t1, t2 = 0, 1, 1
    for i in range(3, n + 1):
        t3 = t0 + t1 + t2
        t0, t1, t2 = t1, t2, t3

    return t3
```
**Step-by-Step Explanation**:
1. Initialize `t0 = 0, t1 = 1, t2 = 1`.
2. Loop from `3` to `n`:
   - Calculate `t3 = t0 + t1 + t2`.
   - Update `t0, t1, t2` for the next iteration.
3. Return `t3`.

---

### 5. Review

#### Dry Run Example
**Input**: `n = 5`  
**Iterative Solution**:
1. `t0 = 0, t1 = 1, t2 = 1`.
2. Iteration 3: `t3 = 0 + 1 + 1 = 2`. Update: `t0 = 1, t1 = 1, t2 = 2`.
3. Iteration 4: `t3 = 1 + 1 + 2 = 4`. Update: `t0 = 1, t1 = 2, t2 = 4`.
4. Iteration 5: `t3 = 1 + 2 + 4 = 7`. Update: `t0 = 2, t1 = 4, t2 = 7`.
5. Return `t3 = 7`.

---

### 6. Evaluate

#### Time Complexity
- Solution 1 (Recursive): O(3^n).
- Solution 2 (Memoized): O(n).
- Solution 3 (DP): O(n).

#### Space Complexity
- Solution 1 (Recursive): O(n) (stack space).
- Solution 2 (Memoized): O(n) (memo dictionary).
- Solution 3 (DP): O(1) (space optimized).

---

### Additional Notes

#### Why This Problem is Important
- Teaches basic recurrence relations and iterative optimization.
- Enhances understanding of recursion vs. dynamic programming.

#### Prerequisites
- Understanding of recursion and memoization.
- Familiarity with iterative techniques in dynamic programming.

#### Industry Relevance
- Similar patterns are common in dynamic programming problems in coding interviews.

#### Follow-Up Practice Problems
- 509. Fibonacci Number
- 70. Climbing Stairs
- 198. House Robber
