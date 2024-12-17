
# LeetCode Problem 163: Missing Ranges

### Problem Statement

You are given a **sorted integer array** `nums` and two integers `lower` and `upper`. The array represents a range of numbers, but it may have missing numbers. Your task is to return all the missing ranges between `lower` and `upper`.

Each range should be represented as a list `[start, end]` where:
- `start` is the beginning of the missing range.
- `end` is the end of the missing range.

If only one number is missing, `start` and `end` will be the same.

### Example:

**Input**:
```
nums = [2, 5, 8]
lower = 0
upper = 10
```

**Output**:
```
[[0, 1], [3, 4], [6, 7], [9, 10]]
```

---
##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

- **Inputs**:
  - A sorted list of integers: `nums`.
  - Two integers `lower` and `upper`, representing the range.

- **Output**:
  - A list of missing ranges `[start, end]`.

- **Constraints**:
  - `nums` is sorted.
  - `lower <= upper`.
  - `nums` can be empty.

**Key Questions**:
- What if `nums` is empty? → We return the range `[lower, upper]`.
- What if all numbers are present? → Return an empty list.

---

### 2. Match

This problem relates to:
- **Array Traversal**: Iterate through the list while comparing consecutive numbers.
- **Edge Case Handling**: Handle `lower` and `upper` separately.

We will:
- Use a **for loop** to traverse `nums`.
- Compare adjacent numbers to find missing ranges.

---

### 3. Plan

We will use the following approach:

1. **Initialize Variables**:
   - Use a variable `prev` to keep track of the previous number, starting at `lower - 1`.
   - Create an empty list `result` to store the missing ranges.

2. **Iterate Through `nums`**:
   - For each number in `nums`, check if there is a missing range between `prev` and the current number.
   - If so, append `[prev + 1, current - 1]` to `result`.

3. **Handle the Upper Bound**:
   - After iterating through `nums`, check for a missing range between the last number and `upper`.

4. **Return the Result**.

---

#### Pseudocode

```plaintext
Initialize result as an empty list
Initialize prev as lower - 1

For i from 0 to len(nums) inclusive:
    If i < len(nums): 
        current = nums[i]
    Else:
        current = upper + 1  # Handle upper bound at the end

    If prev + 1 <= current - 1:
        Add range [prev + 1, current - 1] to result

    Set prev = current

Return result
```

---

### 4. Implement

Here is the Python implementation with detailed comments:

```python
from typing import List

class Solution:
    def findMissingRanges(self, nums: List[int], lower: int, upper: int) -> List[List[int]]:
        result = []  # To store the missing ranges
        prev = lower - 1  # Initialize 'prev' as lower - 1

        # Traverse through nums and include an additional iteration for upper
        for i in range(len(nums) + 1):
            # Handle the current number
            if i < len(nums):
                current = nums[i]  # Normal case: current number in nums
            else:
                current = upper + 1  # Extra step: Handle the 'upper' range

            # Check if there is a missing range
            if prev + 1 <= current - 1:
                result.append([prev + 1, current - 1])  # Append the range to result

            prev = current  # Update 'prev' to the current number

        return result
```

---

### 5. Review

#### Step-by-Step Execution

**Input**:
```
nums = [2, 5, 8], lower = 0, upper = 10
```

| Iteration | `i` | `current` | `prev` | Missing Range Check (`prev + 1 <= current - 1`) | `result`          |
|-----------|-----|-----------|--------|-----------------------------------------------|------------------|
| 0         | 0   | 2         | -1     | 0 <= 1 → True                                  | [[0, 1]]         |
| 1         | 1   | 5         | 2      | 3 <= 4 → True                                  | [[0, 1], [3, 4]] |
| 2         | 2   | 8         | 5      | 6 <= 7 → True                                  | [[0, 1], [3, 4], [6, 7]] |
| 3         | 3   | 11        | 8      | 9 <= 10 → True                                 | [[0, 1], [3, 4], [6, 7], [9, 10]] |

**Output**:
```
[[0, 1], [3, 4], [6, 7], [9, 10]]
```

#### Edge Cases:
1. `nums = []`:
   - Output: `[[lower, upper]]`.

2. `nums = [lower, upper]`:
   - Output: `[]`.

3. Single missing range:
   - Input: `nums = [1], lower = 0, upper = 2`.
   - Output: `[[0, 0], [2, 2]]`.

---

### 6. Evaluate

#### Time Complexity:
- `O(n)` where `n` is the length of `nums`.
- We iterate through the `nums` list once.

#### Space Complexity:
- `O(1)` additional space, excluding the result list.

---

### Additional Notes

#### **Why This Problem is Important**:
- It strengthens your understanding of array traversal and edge case handling.
- It teaches you to handle boundaries (`lower` and `upper`) effectively.

#### **Prerequisites for Practicing**:
- Understanding of arrays.
- Conditional checks and iteration using loops.

#### **Industry Relevance**:
- This problem often appears in system validation and missing data scenarios in software engineering.

#### **Follow-up Practice Problems**:
1. [LeetCode 228: Summary Ranges](https://leetcode.com/problems/summary-ranges/)
2. [LeetCode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
3. [LeetCode 57: Insert Interval](https://leetcode.com/problems/insert-interval/)
