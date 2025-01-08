
# LeetCode 26: Remove Duplicates from Sorted Array

### Problem Statement

Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Return the new length of the array.

Do not allocate extra space for another array. You must modify the input array in place with O(1) extra memory.

#### Input
- `nums`: An array of integers sorted in non-decreasing order.

#### Output
- An integer representing the new length of the array with no duplicates.

#### Constraints
- `0 <= nums.length <= 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in non-decreasing order.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Inputs:
- A sorted integer array `nums`.

#### Outputs:
- The new length of the array after removing duplicates.

#### Constraints:
- No extra space allocation; in-place modification only.
- The order of elements must remain the same.

#### Assumptions:
- If `nums` has a length of 0 or 1, it already satisfies the conditions.

#### Clarifications:
- Any part of `nums` beyond the new length is irrelevant.

---

### 2. Match

This is a **two-pointer problem**, where:
- One pointer (`slow`) keeps track of the position for the next unique element.
- The other pointer (`fast`) traverses the array to find unique elements.

**Pattern**: Array manipulation with two pointers.  
**Algorithm**: Iterate with one pointer, copy values to the position indicated by the other pointer.

---

### 3. Plan

#### Step-by-step Plan:
1. If the array length is 0, return 0 immediately.
2. Initialize two pointers:
   - `slow = 0`: Keeps track of the position of the last unique element.
   - `fast = 1`: Traverses the array to find unique elements.
3. Iterate through the array using `fast`:
   - If `nums[fast] != nums[slow]`, update `nums[slow + 1]` with `nums[fast]` and move `slow` one step forward.
4. After the iteration, the new length is `slow + 1`.
5. Return `slow + 1` as the result.

#### Edge Cases:
- Empty array (`nums = []`).
- Single element array (`nums = [1]`).
- All elements are duplicates (`nums = [1, 1, 1]`).
- No duplicates (`nums = [1, 2, 3]`).

#### Time Complexity:
O(n) - We traverse the array once.

#### Space Complexity:
O(1) - We modify the array in place without additional storage.

---

### 4. Implement

#### Python Code:

```python
def remove_duplicates(nums):
    """
    Removes duplicates from a sorted array in-place.

    Args:
    nums (List[int]): A sorted array of integers.

    Returns:
    int: The new length of the array with unique elements.
    """
    # Edge case: empty array
    if not nums:
        return 0

    # Initialize slow pointer
    slow = 0

    # Iterate with fast pointer
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]  # Update the next unique position

    # The length of the array with unique elements
    return slow + 1
```

#### Notes for Code Implementation:
1. Start with edge cases (`len(nums) == 0`).
2. Use the `slow` pointer to track the last unique position.
3. Compare elements using the `fast` pointer.
4. Update the array in place when a new unique element is found.

---

### 5. Review

#### Debugging:
- Test basic cases such as empty arrays and arrays with only one element.
- Run through complex cases where the array contains alternating duplicates.

#### Example Walkthrough:

##### Example 1:
Input: `nums = [1, 1, 2]`

- Start: `slow = 0`, `fast = 1`
- Iteration:
  - `fast = 1`: `nums[fast] == nums[slow]` (skip).
  - `fast = 2`: `nums[fast] != nums[slow]` → Update `nums[slow + 1] = nums[fast]` and move `slow`.
- End: `nums = [1, 2, 2]`
- Result: Return `2`.

##### Example 2:
Input: `nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]`

- Start: `slow = 0`, `fast = 1`
- Iteration:
  - Skip duplicates until `fast = 2`.
  - `fast = 2`: `nums[fast] != nums[slow]` → Update and move `slow`.
  - Repeat for the entire array.
- End: `nums = [0, 1, 2, 3, 4, _, _, _, _, _]`
- Result: Return `5`.

---

### 6. Evaluate

#### Time Complexity:
- O(n): Single traversal of the array.

#### Space Complexity:
- O(1): In-place modification of the array.

#### Optimization:
This is already optimal for the given constraints.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Demonstrates array manipulation techniques.
- Essential for learning in-place algorithms and space optimization.
- Common in real-world scenarios where memory usage is critical.

#### Prerequisites for Practicing This Problem
- Understanding of two-pointer technique.
- Basic array traversal and conditionals.

#### Industry Relevance
- Used in scenarios requiring data deduplication, such as database queries or sorting results.

#### Follow-up Practice Problems
1. **LeetCode 80**: Remove Duplicates from Sorted Array II (allow at most two duplicates).
2. **LeetCode 283**: Move Zeroes.
3. **LeetCode 27**: Remove Element.
4. **LeetCode 977**: Squares of a Sorted Array.
