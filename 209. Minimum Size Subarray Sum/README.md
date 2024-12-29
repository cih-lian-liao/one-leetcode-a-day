
# LeetCode 209: Minimum Size Subarray Sum

### Problem Description
Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a contiguous subarray `[nums[l], nums[l+1], ..., nums[r-1], nums[r]]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
**Inputs**:
- `target`: Integer, the target sum.
- `nums`: List of positive integers.

**Outputs**:
- An integer representing the minimum length of the subarray whose sum is ≥ `target`.

**Constraints**:
- 1 ≤ `target` ≤ 10⁹.
- 1 ≤ `nums.length` ≤ 10⁵.
- 1 ≤ `nums[i]` ≤ 10⁴.

**Clarifications**:
- If no such subarray exists, return `0`.

---

### 2. Match
**Pattern**:
This problem aligns with the sliding window technique because it involves identifying a contiguous subarray with a specific property.

**Why Sliding Window**:
- We want to minimize the window size while maintaining a valid sum ≥ `target`.
- Sliding window efficiently adjusts the start (`l`) and end (`r`) of the window.

---

### 3. Plan
**Step-by-Step Plan**:
1. Initialize two pointers `l` (left) and `r` (right) to represent the sliding window.
2. Maintain a running total `cur_total` to track the sum of the current window.
3. Start with `res = len(nums) + 1`, a value larger than any possible subarray length.
4. Iterate through `nums` using the right pointer (`r`):
   - Add `nums[r]` to `cur_total`.
   - While `cur_total` ≥ `target`:
     - Update `res` to the minimum of its current value and the current window size (`r - l + 1`).
     - Shrink the window by subtracting `nums[l]` and incrementing `l`.
5. After the loop, return `0` if `res` was not updated; otherwise, return `res`.

**Edge Cases**:
- No subarray satisfies the condition: Example, `nums = [1, 2, 3]`, `target = 10`.
- Single element greater than `target`: Example, `nums = [11]`, `target = 10`.

**Time Complexity**:
- O(n), where `n` is the length of `nums`. Each element is processed at most twice (once when expanding the window, once when contracting it).

**Space Complexity**:
- O(1), as only variables are used for calculations.

---

### 4. Implement

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l, cur_total = 0, 0
        res = len(nums) + 1

        for r in range(len(nums)):
            cur_total += nums[r]
            while cur_total >= target:
                res = min(r - l + 1, res)
                cur_total -= nums[l]
                l += 1

        return 0 if res == len(nums) + 1 else res
```

**Explanation of Implementation**:
- `l`: Tracks the start of the sliding window.
- `cur_total`: Keeps the current sum of the window.
- `res`: Stores the minimum subarray length; initialized to a large value to ensure updates.
- The `while` loop ensures that the window shrinks to its minimal valid size whenever the condition is met.

---

### 5. Review

**Debugging and Testing**:
1. **Test Case 1**:
   - Input: `target = 7`, `nums = [2, 3, 1, 2, 4, 3]`
   - Execution:
     - Sliding window grows and shrinks dynamically to find the minimal subarray `[4, 3]`.
   - Output: `2`
   - Verify step-by-step with dry-run.

2. **Edge Case**:
   - Input: `target = 15`, `nums = [1, 2, 3, 4, 5]`
   - Execution:
     - Total sum never reaches `15`.
   - Output: `0`

3. **Complex Case**:
   - Input: `target = 100`, `nums = [1]*50 + [100]`
   - Execution:
     - Minimal subarray is `[100]`.
   - Output: `1`

**Dry Run Example**:
For `target = 7`, `nums = [2, 3, 1, 2, 4, 3]`:
- Window expands: `cur_total` grows as `r` increments.
- Once `cur_total` ≥ `target`, shrink the window by incrementing `l`.
- Track the smallest window size during the process.

---

### 6. Evaluate

**Time Complexity**:
- O(n), each element in `nums` is visited at most twice.

**Space Complexity**:
- O(1), no additional data structures used.

**Optimizations**:
- Current approach is optimal for the given problem constraints.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It tests the ability to apply sliding window efficiently.
- Commonly encountered in scenarios like streaming data or contiguous subarray problems.

#### Prerequisites for Practicing This Problem
- Understanding of sliding window technique.
- Familiarity with two-pointer approach.

#### Industry Relevance
- Sliding window is widely used in real-time data streaming and analytics.

#### Follow-up Practice Problems
1. [LeetCode 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
2. [LeetCode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
3. [LeetCode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
