# Problem: LeetCode 75 - Sort Colors

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Problem Description**  
You are given an array `nums` with `n` integers where each element is `0`, `1`, or `2`. Sort the array in-place so that integers of the same value are adjacent, with the order `0`, `1`, `2`.

**Inputs**  
- An integer array `nums` containing only `0`, `1`, and `2`.

**Outputs**  
- A sorted array `nums` where all `0`s come first, followed by `1`s, and then `2`s.

**Constraints**  
1. The array must be sorted in-place.
2. The algorithm must have a linear runtime complexity (O(n)).

**Clarifications**  
- The sorting should be performed without using any extra space (O(1) space complexity).

---

### 2. Match

**Problem Type**  
- This problem is a variant of the "Dutch National Flag" problem, commonly solved using the two-pointer technique.

**Algorithm/Data Structure**  
- Two pointers (`low` and `high`) and a current index pointer (`current`).

---

### 3. Plan

**Approach**  
1. Use three pointers:
   - `low`: Points to the position where the next `0` should be placed.
   - `high`: Points to the position where the next `2` should be placed.
   - `current`: Iterates through the array.
2. Traverse the array using `current` until it surpasses `high`.
3. For each element:
   - If `nums[current] == 0`: Swap it with `nums[low]` and increment both `low` and `current`.
   - If `nums[current] == 2`: Swap it with `nums[high]` and decrement `high`. Do not increment `current` since the swapped value must be checked.
   - If `nums[current] == 1`: Simply increment `current`.

**Pseudocode**  
```text
low, current, high = 0, 0, len(nums) - 1

while current <= high:
    if nums[current] == 0:
        swap(nums[low], nums[current])
        low += 1
        current += 1
    elif nums[current] == 2:
        swap(nums[high], nums[current])
        high -= 1
    else:
        current += 1
```

**Time Complexity**  
- O(n), where n is the length of the array. Each element is processed at most twice.

**Space Complexity**  
- O(1), as no additional space is used beyond the input array.

**Edge Cases**  
- An empty array (`nums = []`).
- Arrays with all identical values, e.g., `nums = [0, 0, 0]`, `nums = [1, 1, 1]`, or `nums = [2, 2, 2]`.

---

### 4. Implement

```python
def sortColors(nums):
    # Initialize pointers
    low, current, high = 0, 0, len(nums) - 1

    # Traverse the array
    while current <= high:
        if nums[current] == 0:
            # Swap current element with the element at `low`
            nums[low], nums[current] = nums[current], nums[low]
            low += 1
            current += 1
        elif nums[current] == 2:
            # Swap current element with the element at `high`
            nums[high], nums[current] = nums[current], nums[high]
            high -= 1
        else:
            # If the element is 1, move `current` forward
            current += 1
```

**Implementation Notes**  
- **Initialization**: `low` starts at index `0`, `high` at the last index, and `current` iterates through the array.
- **Swapping**: Swapping ensures that `0` moves to the left and `2` moves to the right, reducing the sorting problem incrementally.
- **Pointer Movement**: `current` advances only when `nums[current]` is not `2`, ensuring that all elements are checked.

---

### 5. Review

**Dry Run with Complex Example**  
Input: `nums = [2, 0, 2, 1, 1, 0, 2, 1, 0]`  
Expected Output: `[0, 0, 0, 1, 1, 1, 2, 2, 2]`

| Step | `nums`                  | `low` | `current` | `high` | Operation                    |
|------|--------------------------|-------|-----------|--------|------------------------------|
| Init | [2, 0, 2, 1, 1, 0, 2, 1, 0] | 0     | 0         | 8      | Start                       |
| 1    | [0, 0, 2, 1, 1, 0, 2, 1, 2] | 1     | 1         | 7      | Swap `nums[0]` and `nums[8]`|
| 2    | [0, 0, 2, 1, 1, 0, 2, 1, 2] | 1     | 2         | 7      | Skip, move `current`        |
| 3    | [0, 0, 1, 1, 1, 0, 2, 2, 2] | 1     | 3         | 6      | Swap `nums[2]` and `nums[7]`|
| 4    | [0, 0, 1, 1, 1, 0, 2, 2, 2] | 1     | 4         | 6      | Skip, move `current`        |
| End  | [0, 0, 0, 1, 1, 1, 2, 2, 2] | -     | -         | -      | Sorting complete            |

---

### 6. Evaluate

**Time Complexity**  
- O(n): The array is traversed once, and each element is processed at most twice.

**Space Complexity**  
- O(1): Sorting is done in-place without using extra space.

**Optimization**  
This algorithm is already optimal for the given constraints.

---

### Additional Notes

**The Reason Why This Problem is Important**  
- It demonstrates the efficiency of the two-pointer technique, a fundamental algorithmic concept.

**Prerequisites for Practicing This Problem**  
- Understanding of array manipulation.
- Knowledge of pointer-based algorithms.

**Industry Relevance**  
- Sorting and in-place operations are common in coding interviews and real-world applications like memory-constrained systems.

**Follow-up Practice Problems**  
- LeetCode 26: Remove Duplicates from Sorted Array.
- LeetCode 88: Merge Sorted Array.
- LeetCode 283: Move Zeroes.
