
# LeetCode 303: Range Sum Query - Immutable

### Problem Statement

You are given an integer array `nums` that is immutable. Implement a data structure `NumArray` to efficiently calculate the sum of the elements of `nums` between indices `left` and `right` inclusive, where `left <= right`.

#### Input:
- `nums`: An integer array.
- `left`, `right`: Integers indicating the range of the array indices.

#### Output:
- Returns the sum of the elements in the range `[left, right]`.

#### Constraints:
- `1 <= nums.length <= 10^4`
- `-10^5 <= nums[i] <= 10^5`
- `0 <= left <= right < nums.length`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Objective:
To calculate the sum of elements in an immutable array for a given range `[left, right]` efficiently.

#### Key Points:
- The array is **immutable**, meaning its values will not change after initialization.
- Multiple `sumRange` queries will be executed, so we need a **fast solution**.
- Constraints ensure the array size and values can be large, so **time efficiency** is crucial.

#### Inputs:
- An integer array `nums`.
- Indices `left` and `right`.

#### Outputs:
- Integer representing the sum of the elements in the range `[left, right]`.

---

### 2. Match

This problem involves querying the sum of a range repeatedly. The following approach fits well:
- **Prefix Sum Technique**:
  - Precompute the cumulative sum of elements up to each index.
  - Use the prefix sum array to calculate the sum of any range in O(1) time.

---

### 3. Plan

#### Steps:
1. **Initialization**:
   - Create a prefix sum array where each element at index `i` stores the sum of `nums[0]` to `nums[i]`.
   - This allows us to calculate any range sum using the formula:
     `sumRange(left, right) = prefix[right] - prefix[left - 1]`
   - Handle the edge case for `left == 0` by returning `prefix[right]`.

2. **Range Sum Query**:
   - For any `sumRange(left, right)` call:
     - Use the prefix sum array to retrieve the sum in O(1) time.

#### Pseudocode:
```plaintext
Initialize `prefix_sum` array:
  prefix_sum[i] = prefix_sum[i-1] + nums[i]

For sumRange(left, right):
  If left == 0:
    Return prefix_sum[right]
  Else:
    Return prefix_sum[right] - prefix_sum[left - 1]
```

#### Time Complexity:
- Initialization: O(n) (for computing the prefix sum).
- Query: O(1) per `sumRange` call.

#### Space Complexity:
- O(n) for the prefix sum array.

---

### 4. Implement

```python
class NumArray:
    def __init__(self, nums: List[int]):
        """
        Initialize the NumArray with a prefix sum array.
        """
        self.prefix = []
        cur = 0
        # Build prefix sum array
        for n in nums:
            cur += n
            self.prefix.append(cur)

    def sumRange(self, left: int, right: int) -> int:
        """
        Calculate the sum of the range [left, right] using the prefix sum array.
        """
        # Get prefix sums for the right and left indices
        right_prefix = self.prefix[right]
        left_prefix = self.prefix[left - 1] if left > 0 else 0

        # Return the range sum
        return right_prefix - left_prefix
```

#### Implementation Notes:
- The `__init__` method constructs the prefix sum array by iterating through `nums` and maintaining a running total.
- The `sumRange` method uses the prefix sum array to calculate the range sum efficiently.

---

### 5. Review

#### Example 1:
Input:
```python
nums = [-2, 0, 3, -5, 2, -1]
numArray = NumArray(nums)

# Query range sums
print(numArray.sumRange(0, 2))  # Output: 1
print(numArray.sumRange(2, 5))  # Output: -1
print(numArray.sumRange(0, 5))  # Output: -3
```

#### Dry Run:
1. Initialize `prefix`:
   - Prefix array: `[-2, -2, 1, -4, -2, -3]`
     - `prefix[0] = nums[0] = -2`
     - `prefix[1] = prefix[0] + nums[1] = -2 + 0 = -2`
     - `prefix[2] = prefix[1] + nums[2] = -2 + 3 = 1`
     - ...

2. Query `sumRange(0, 2)`:
   - `right_prefix = prefix[2] = 1`
   - `left_prefix = prefix[-1] = 0` (since `left == 0`)
   - `Result = 1 - 0 = 1`

3. Query `sumRange(2, 5)`:
   - `right_prefix = prefix[5] = -3`
   - `left_prefix = prefix[1] = -2`
   - `Result = -3 - (-2) = -1`

4. Query `sumRange(0, 5)`:
   - `right_prefix = prefix[5] = -3`
   - `left_prefix = prefix[-1] = 0` (since `left == 0`)
   - `Result = -3 - 0 = -3`

---

### 6. Evaluate

#### Time Complexity:
- Initialization: O(n)
- Query: O(1)

#### Space Complexity:
- O(n)

---

### Additional Notes

#### Why This Problem is Important:
- Teaches optimization using the **Prefix Sum** technique.
- Emphasizes reducing query time for multiple operations.

#### Prerequisites:
- Familiarity with arrays and cumulative sums.

#### Industry Relevance:
- Common in database queries and analytical tools where repeated range queries are required.

#### Follow-up Practice Problems:
1. LeetCode 304: Range Sum Query 2D - Immutable
2. LeetCode 307: Range Sum Query - Mutable
3. LeetCode 238: Product of Array Except Self
4. LeetCode 53: Maximum Subarray
