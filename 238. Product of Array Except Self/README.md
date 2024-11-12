# Problem 238. Product of Array Except Self

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### U - Understand the Problem
The goal of this problem is to return an array `output` where each element `output[i]` represents the product of all elements in the input array `nums` except the element `nums[i]`. Important constraints are as follows:
- **No Division**: We are not allowed to use division to solve this problem.
- **Linear Time Complexity**: The solution must be achieved in O(n) time.

#### Example Walkthrough
Let’s consider an example input:
- **Input**: `nums = [1, 2, 3, 4]`
- **Expected Output**: `[24, 12, 8, 6]`

For each `output[i]`:
- `output[0]` should be the product of all numbers except `nums[0]`: 2 * 3 * 4 = 24
- `output[1]` should be the product of all numbers except `nums[1]`: 1 * 3 * 4 = 12
- `output[2]` should be the product of all numbers except `nums[2]`: 1 * 2 * 4 = 8
- `output[3]` should be the product of all numbers except `nums[3]`: 1 * 2 * 3 = 6

This indicates that we need a way to calculate products from both **left** and **right** of each element without including the element itself.

---

### M - Match with Known Problems
This problem is a classic example of accumulating products for each position by excluding the current element without division, which requires efficient use of **prefix** (left product) and **suffix** (right product) calculations:
- **Prefix (Left Product)**: Cumulative product of elements to the left of each position.
- **Suffix (Right Product)**: Cumulative product of elements to the right of each position.

---

### P - Plan
To solve this problem, we’ll use a two-pass approach:
1. **Calculate the Prefix/Left Product**:
   - Initialize a result array, `output`, with `1`s for each position.
   - Traverse `nums` from left to right, calculating the **prefix product** for each position and storing it in `output`.
   
2. **Calculate the Suffix/Right Product and Update `output`**:
   - Initialize a variable to track the **suffix product**.
   - Traverse `nums` from right to left, multiplying each element in `output` by the suffix product, then updating the suffix product by multiplying it with the current element in `nums`.

---

### I - Implement
Here’s the code implementation based on this plan. This code combines prefix and suffix (left and right products) into a single `output` array without using extra space beyond the initial array, achieving O(1) additional space complexity.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result = [1] * len(nums)  # Step 1: Initialize the result array with 1s

        # Calculate prefix (left product) for each element
        prefix = 1
        for i in range(len(nums)):
            result[i] = prefix
            prefix *= nums[i]  # Update prefix as we move right

        # Calculate suffix (right product) for each element and multiply
        suffix = 1
        for i in range(len(nums) - 1, -1, -1):
            result[i] *= suffix
            suffix *= nums[i]  # Update suffix as we move left
        
        return result
```

---

### R - Review
Let’s review our code to ensure it meets the requirements:
- **Correctness**: Each element `output[i]` in the final array contains the product of all elements in `nums` except `nums[i]`.
- **Time Complexity**: O(n) for two linear passes, one for prefix and one for suffix calculations.
- **Space Complexity**: O(1) additional space, as we modify `output` in-place with prefix and suffix products.

---

### E - Evaluate / Example Run-through
Let’s test our code with `nums = [1, 2, 3, 4]` step-by-step to validate the solution:

1. **Initialize `result` Array**:
   - `result = [1, 1, 1, 1]`

2. **First Loop (Prefix/Left Product Calculation)**:
   - `prefix = 1`
   - For each index, we assign the current `prefix` to `result[i]`, then update `prefix` by multiplying it with `nums[i]`:
     - `i = 0`: `result[0] = 1`, `prefix = prefix * nums[0] = 1 * 1 = 1`
     - `i = 1`: `result[1] = 1`, `prefix = prefix * nums[1] = 1 * 2 = 2`
     - `i = 2`: `result[2] = 2`, `prefix = prefix * nums[2] = 2 * 3 = 6`
     - `i = 3`: `result[3] = 6`, `prefix = prefix * nums[3] = 6 * 4 = 24`
   - After this pass, `result = [1, 1, 2, 6]`, where each `result[i]` contains the left product up to that point.

3. **Second Loop (Suffix/Right Product Calculation)**:
   - `suffix = 1`
   - For each index from right to left, we multiply `result[i]` by `suffix`, then update `suffix` by multiplying it with `nums[i]`:
     - `i = 3`: `result[3] = result[3] * suffix = 6 * 1 = 6`, `suffix = suffix * nums[3] = 1 * 4 = 4`
     - `i = 2`: `result[2] = result[2] * suffix = 2 * 4 = 8`, `suffix = suffix * nums[2] = 4 * 3 = 12`
     - `i = 1`: `result[1] = result[1] * suffix = 1 * 12 = 12`, `suffix = suffix * nums[1] = 12 * 2 = 24`
     - `i = 0`: `result[0] = result[0] * suffix = 1 * 24 = 24`, `suffix = suffix * nums[0] = 24 * 1 = 24`
   - After this pass, `result = [24, 12, 8, 6]`, which matches our expected output.

The code produces the correct result, `[24, 12, 8, 6]`, ensuring that each element in `output` is the product of all elements in `nums` except for the element at that position.

---

### Summary
This approach effectively uses prefix (left product) and suffix (right product) calculations to achieve the required result in O(n) time and O(1) space (excluding the output array). This solution meets all problem constraints and is optimized for both time and space efficiency.
