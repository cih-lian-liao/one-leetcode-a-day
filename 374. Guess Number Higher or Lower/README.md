
# LeetCode 374: Guess Number Higher or Lower

### Problem Description

You are given a function `guess(num)` that determines if the guessed number `num` is:
- `-1` if `num` is higher than the target.
- `1` if `num` is lower than the target.
- `0` if `num` is the target.

Your task is to implement the function `guessNumber(n)` to find the correct number between `1` and `n`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input**: An integer `n`, representing the range `[1, n]` of numbers.
- **Output**: An integer, the correct number guessed.
- **Constraints**:
  - 1 ≤ `n` ≤ 2³¹ - 1 (very large range).
  - The target number is always within the range.
  - You are given a `guess()` API to determine the relationship between `num` and the target.
- **Clarifications**:
  - The problem guarantees the existence of a target number.
  - The `guess()` function behaves as described, so no need to handle invalid inputs.

---

### 2. Match

This problem is a classic **binary search** problem. The target can be efficiently found by repeatedly halving the search range. 

- **Algorithm**: Binary Search.
- **Key Pattern**: Divide and conquer to narrow down the range.
- **Data Structures**: None required for this problem.

---

### 3. Plan

We will use binary search to solve the problem:
1. Start with `l = 1` and `r = n` as the search range.
2. Find the middle number `m = (l + r) // 2`.
3. Use the `guess(m)` function to check:
   - If `guess(m) == 0`, return `m` as the target number.
   - If `guess(m) == -1`, adjust the range to `[l, m - 1]`.
   - If `guess(m) == 1`, adjust the range to `[m + 1, r]`.
4. Repeat until the correct number is found.
5. Return the remaining number when `l == r`.

**Edge Cases**:
- Smallest range (`n = 1`): Directly return `1`.
- Large range (`n` is very large): Algorithm should still work in O(log n) time.

**Time Complexity**: O(log n).  
**Space Complexity**: O(1).

---

### 4. Implement

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        l, r = 1, n  # Initialize the search range
        
        while l <= r:  # Continue while the range is valid
            m = (l + r) // 2  # Find the midpoint
            
            result = guess(m)  # Call the API with the midpoint
            
            if result == 0:  # Found the target
                return m
            elif result == -1:  # Target is smaller
                r = m - 1
            else:  # Target is larger
                l = m + 1
        
        # The loop guarantees we find the target, no need for additional return
```

**Implementation Notes**:
1. **Binary Search**:
   - Divide the range in half each iteration to reduce the problem size.
2. **Guess API**:
   - Save the result of `guess(m)` to avoid multiple calls.
3. **Edge Cases**:
   - The loop will exit correctly when the target is found, so no additional checks are required.

---

### 5. Review

**Debugging**:  
Example Dry Run:

Input: `n = 10`, Target = 6  
- Initial range: `l = 1`, `r = 10`
- Midpoint `m = (1 + 10) // 2 = 5`
  - `guess(5) == 1` → Adjust range to `[6, 10]`
- New midpoint `m = (6 + 10) // 2 = 8`
  - `guess(8) == -1` → Adjust range to `[6, 7]`
- New midpoint `m = (6 + 7) // 2 = 6`
  - `guess(6) == 0` → Found target, return `6`.

**Edge Cases**:
- `n = 1`, Target = 1:
  - Initial range: `l = 1`, `r = 1`
  - Midpoint `m = 1`, `guess(1) == 0`
  - Return `1`.

**Validation**:
- Works for smallest and largest ranges.
- Handles constraints efficiently.

---

### 6. Evaluate

**Time Complexity**:  
- Binary search reduces the range by half each iteration, resulting in **O(log n)** time.

**Space Complexity**:  
- No additional data structures are used, so space complexity is **O(1)**.

**Optimizations**:  
- Already optimal due to the binary search approach.

---

### Additional Notes

**The Reason Why This Problem is Important**:
- Demonstrates mastery of binary search, a foundational algorithm.
- Frequently used in real-world applications like database queries and search engines.

**Prerequisites for Practicing This Problem**:
- Understanding binary search.
- Familiarity with conditional statements and loops.

**Industry Relevance**:
- Commonly used in interview settings to evaluate algorithmic thinking.
- Binary search is integral in many software systems, from searching sorted datasets to load balancing in distributed systems.

**Follow-up Practice Problems**:
- [LeetCode 278: First Bad Version](https://leetcode.com/problems/first-bad-version/)
- [LeetCode 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)
- [LeetCode 69: Sqrt(x)](https://leetcode.com/problems/sqrtx/)
