# LeetCode Problem 448: Find All Numbers Disappeared in an Array

### Problem Statement

Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

#### Example:

##### Input:
```python
nums = [4,3,2,7,8,2,3,1]
```
##### Output:
```python
[5,6]
```

##### Constraints:
- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs:**
- An integer array `nums` of length `n`.

**Outputs:**
- An array containing all integers in the range `[1, n]` that are missing from `nums`.

**Constraints:**
- All integers in `nums` are between `1` and `n`.
- Duplicate numbers may exist in `nums`.

**Clarifications:**
- What should we return if all numbers are present? Return an empty list.
- Can `nums` be empty? No, based on the constraints (`1 <= n`).

---

### 2. Match

**Pattern:**
- This problem relates to **index manipulation** and **in-place marking** to identify missing values.

**Relevant Techniques:**
- Use the index of the array as a marker to flag the presence of a number by making the value at that index negative.
- Iterate over the array to find indices with positive values, which correspond to the missing numbers.

---

### 3. Plan

**Step-by-Step Approach:**
1. **Mark Present Numbers:**
   - Iterate through each number in `nums`.
   - For each number `n`, calculate its target index `i = abs(n) - 1`.
   - Make `nums[i]` negative to indicate that the number `i + 1` is present.

2. **Identify Missing Numbers:**
   - Iterate through the array.
   - For each index `i`, if `nums[i] > 0`, append `i + 1` to the result since `i + 1` is missing.

3. **Return the Result:**
   - Return the list of missing numbers.

**Edge Cases:**
- Array contains all numbers (e.g., `[1,2,3,4]`).
- Array contains duplicates (e.g., `[1,1,2,2]`).

**Time Complexity:** O(n)
- Single pass to mark the numbers.
- Single pass to find missing numbers.

**Space Complexity:** O(1) (in-place modification of the array).

---

### 4. Implement

```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        # Step 1: Mark present numbers
        for n in nums:
            index = abs(n) - 1  # Find the target index
            nums[index] = -abs(nums[index])  # Mark as negative

        # Step 2: Identify missing numbers
        result = []
        for i, num in enumerate(nums):
            if num > 0:  # Positive value means missing number
                result.append(i + 1)

        # Step 3: Return the result
        return result
```

**Step-by-Step Implementation Notes:**
1. Use `abs(n)` to avoid interference from previously marked numbers.
2. Use the index as a marker.
3. Collect all indices where the value remains positive after marking.

---

### 5. Review

**Testing with Edge Cases:**
1. **Case: All numbers present**
   ```python
   nums = [1,2,3,4]
   Output: []
   ```
2. **Case: All numbers missing**
   ```python
   nums = [4,4,4,4]
   Output: [1,2,3]
   ```
3. **Case: Mix of present and missing numbers**
   ```python
   nums = [4,3,2,7,8,2,3,1]
   Output: [5,6]
   ```

**Dry Run:**

Input: `nums = [4,3,2,7,8,2,3,1]`

**Marking Phase:**
1. `n = 4` -> Mark index `3` -> `nums = [4,3,2,-7,8,2,3,1]`
2. `n = 3` -> Mark index `2` -> `nums = [4,3,-2,-7,8,2,3,1]`
3. `n = -2` -> Mark index `1` -> `nums = [4,-3,-2,-7,8,2,3,1]`
4. `n = -7` -> Mark index `6` -> `nums = [4,-3,-2,-7,8,2,-3,1]`
5. `n = 8` -> Mark index `7` -> `nums = [4,-3,-2,-7,8,2,-3,-1]`
6. `n = 2` -> Mark index `1` -> No change (already marked negative).
7. `n = -3` -> Mark index `2` -> No change (already marked negative).
8. `n = -1` -> Mark index `0` -> `nums = [-4,-3,-2,-7,8,2,-3,-1]`

**Result Collection Phase:**
1. Index `4` -> Positive -> Add `5` to result.
2. Index `5` -> Positive -> Add `6` to result.

Output: `[5,6]`

---

### 6. Evaluate

**Time Complexity:** O(n)
- Each loop runs in linear time.

**Space Complexity:** O(1)
- In-place marking, no extra space required.

**Optimizations:**
- The solution is already optimal in terms of time and space complexity.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Understanding in-place array manipulation is a key skill for many coding interviews.

#### Prerequisites for Practicing This Problem
- Knowledge of array traversal and index manipulation.
- Understanding of absolute values to manage sign flipping.

#### Industry Relevance
- Efficient data analysis in scenarios with constrained memory.

#### Follow-up Practice Problems
- LeetCode 442: Find All Duplicates in an Array
- LeetCode 41: First Missing Positive
- LeetCode 645: Set Mismatch
