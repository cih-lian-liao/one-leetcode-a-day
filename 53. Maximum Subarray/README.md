
# LeetCode 53: Maximum Subarray

### Problem Statement
Given an integer array `nums`, find the contiguous subarray (containing at least one number) that has the largest sum and return its sum.

#### Example Inputs and Outputs:
- Input: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
  - Output: `6` (Subarray: `[4,-1,2,1]`)

- Input: `nums = [1]`
  - Output: `1` (Subarray: `[1]`)

- Input: `nums = [5,4,-1,7,8]`
  - Output: `23` (Subarray: `[5,4,-1,7,8]`)

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- **Inputs:**  
  A list of integers `nums` of size `n`. The integers can be negative or positive.

- **Outputs:**  
  An integer representing the maximum sum of a contiguous subarray.

- **Constraints:**  
  - `1 <= nums.length <= 10^5`
  - `-10^4 <= nums[i] <= 10^4`

- **Clarifications:**
  - The subarray must be contiguous.
  - The array contains at least one element, so no need to handle an empty array.

---

### 2. Match
This problem can be categorized as:
- **Dynamic Programming:** Using a running total to calculate the maximum subarray sum efficiently.
- **Greedy Algorithm:** Choosing whether to extend the current subarray or start a new one based on the current sum.

Known approaches:
1. **Kadane's Algorithm:** Optimal approach with O(n) time complexity and O(1) space complexity.

---

### 3. Plan

**Approach 1 (Solution 1):**  
1. Initialize `max_sum` with the first element of the array, and `current_sum` to `0`.
2. Iterate through the array.
   - Add the current number to `current_sum`.
   - If `current_sum` is greater than `max_sum`, update `max_sum`.
   - If `current_sum` drops below `0`, reset it to `0`.
3. Return `max_sum`.

**Approach 2 (Solution 2):**
1. Initialize `maxSum` with the first element and `curSum` to `0`.
2. Iterate through the array:
   - Reset `curSum` to `0` if it’s negative.
   - Add the current element to `curSum`.
   - Update `maxSum` to the maximum of `maxSum` and `curSum`.
3. Return `maxSum`.

---

### 4. Implement

#### Solution 1: Python Implementation
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Initialize variables
        current_sum = 0
        max_sum = nums[0]

        # Iterate through each number in nums
        for num in nums:
            current_sum += num
            max_sum = max(max_sum, current_sum)
            if current_sum < 0:  # Reset current_sum if it drops below 0
                current_sum = 0

        return max_sum
```

**Notes for Implementation:**
1. The variable `current_sum` tracks the running total of the current subarray.
2. The `max_sum` keeps track of the global maximum sum seen so far.
3. Resetting `current_sum` to `0` ensures that we don't carry forward a negative sum.

---

#### Solution 2: Python Implementation
```python
def kadanes(nums):
    # Initialize variables
    maxSum = nums[0]
    curSum = 0

    # Iterate through nums
    for n in nums:
        if curSum < 0:
            curSum = 0  # Reset curSum if negative
        curSum += n
        maxSum = max(maxSum, curSum)  # Update maxSum if curSum is greater
    return maxSum
```

**Notes for Implementation:**
1. The check `if curSum < 0` avoids carrying over a negative sum to the next subarray.
2. The use of `max()` updates `maxSum` to the largest value dynamically.

---

### 5. Review

#### Test Cases:
1. Input: `nums = [-2,1,-3,4,-1,2,1,-5,4]`  
   Dry Run:  
   - `current_sum`: -2 → 1 → -2 → 4 → 3 → 5 → 6 → 1 → 5  
   - `max_sum`: -2 → 1 → 1 → 4 → 4 → 5 → 6 → 6 → 6  
   Output: `6`

2. Input: `nums = [1]`  
   Output: `1` (Single-element array.)

3. Input: `nums = [5,4,-1,7,8]`  
   Dry Run:  
   - `curSum`: 5 → 9 → 8 → 15 → 23  
   - `maxSum`: 5 → 9 → 9 → 15 → 23  
   Output: `23`

---

### 6. Evaluate

**Time Complexity:**  
- Both solutions iterate through the array once, making the time complexity O(n).

**Space Complexity:**  
- Both solutions use O(1) space, as they only use a few variables for calculations.

**Optimizations:**  
- Both implementations are already optimal in terms of time and space complexity.

---

### Additional Notes:

#### Why This Problem is Important:
- Kadane's Algorithm is a foundational dynamic programming concept.
- It teaches how to identify patterns in greedy algorithms and optimize subarray problems.

#### Prerequisites for Practicing This Problem:
- Familiarity with dynamic programming.
- Understanding of prefix sums and how to maintain running totals.

#### Industry Relevance:
- Subarray problems are frequently asked in interviews.
- Variants of this problem can appear in fields like financial data analysis or real-time system monitoring.

#### Follow-Up Practice Problems:
1. **LeetCode 152:** Maximum Product Subarray
2. **LeetCode 918:** Maximum Sum Circular Subarray
3. **LeetCode 862:** Shortest Subarray with Sum at Least K
