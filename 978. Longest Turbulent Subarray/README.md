

# LeetCode 978: Longest Turbulent Subarray

### Problem Statement

Given an integer array `arr`, return the length of a maximum size turbulent subarray of `arr`.

A subarray is turbulent if the comparison between every two adjacent elements alternates between greater than and less than.

For example:
- If `arr[i-1] > arr[i] < arr[i+1]`, the subarray is turbulent.
- If `arr[i-1] < arr[i] > arr[i+1]`, the subarray is turbulent.

A single element is also considered a turbulent subarray.

### Constraints
- `1 <= arr.length <= 4 * 10^4`
- `0 <= arr[i] <= 10^9`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input:** An integer array `arr` of size `n`.
- **Output:** An integer representing the length of the longest turbulent subarray.
- **Constraints:** Must compute the result within reasonable time complexity given the constraints.

#### Clarifications
- A subarray can include only one element and still be valid.
- The elements in `arr` can be equal, in which case they break the turbulence.

---

### 2. Match

This problem involves finding a subarray with specific conditions, which relates to:
- **Sliding Window Technique:** Efficiently expanding and shrinking a window to track valid segments.
- The sliding window approach helps maintain a range of indices `[l, r]` that represents the current turbulent subarray.

Key observation:
- Use a pointer `l` for the left boundary of the window and increment a pointer `r` to expand the right boundary.
- Reset `l` when turbulence breaks.

---

### 3. Plan

#### Steps
1. Initialize two pointers `l` and `r` for the sliding window and a variable `res` to store the maximum length.
2. Use a variable `prev` to track the last comparison result (`>` or `<`).
3. Traverse the array:
   - If the current element and the previous element satisfy turbulence conditions:
     - Update `res` with the maximum length of the current window.
     - Update `prev` to store the current comparison result.
   - If turbulence breaks:
     - Reset `l` appropriately based on the type of failure.
4. Return `res` after traversing the array.

#### Pseudocode
```
l = 0, res = 1
prev = ""
For r from 1 to len(arr) - 1:
    If turbulence conditions satisfied:
        Update res
        Update prev
    Else:
        Reset l
        Reset prev
Return res
```

---

### 4. Implement

```python
class Solution:
    def maxTurbulenceSize(self, arr: List[int]) -> int:
        l, r = 0, 1  # Initialize sliding window boundaries
        res, prev = 1, ""  # Initialize result and previous comparison state

        while r < len(arr):
            if arr[r - 1] > arr[r] and prev != ">":
                # Valid turbulence: arr[r-1] > arr[r]
                res = max(res, r - l + 1)  # Update max length
                r += 1
                prev = ">"  # Update previous state
            elif arr[r - 1] < arr[r] and prev != "<":
                # Valid turbulence: arr[r-1] < arr[r]
                res = max(res, r - l + 1)  # Update max length
                r += 1
                prev = "<"  # Update previous state
            else:
                # Turbulence breaks
                r = r + 1 if arr[r] == arr[r - 1] else r  # Move right pointer
                l = r - 1  # Reset left pointer
                prev = ""  # Reset state
        
        return res
```

---

### Implementation Notes

- **Initialization:**
  - `l` and `r` form the sliding window to track turbulent subarrays.
  - `res` tracks the maximum length found so far.

- **Conditions for turbulence:**
  - `arr[r-1] > arr[r]` and the previous condition was not `">"`.
  - `arr[r-1] < arr[r]` and the previous condition was not `"<"`.

- **Reset Logic:**
  - If `arr[r] == arr[r-1]`, reset the left pointer to `r`.
  - Otherwise, reset to `r-1`.

---

### 5. Review

#### Edge Cases
- Single element array, e.g., `[1]`.
- All elements are equal, e.g., `[5, 5, 5, 5]`.
- Strictly increasing or decreasing array, e.g., `[1, 2, 3, 4]`.

#### Dry Run Example

Input:
```python
arr = [9, 4, 2, 10, 7, 8, 8, 1, 9]
```

Execution:

| Step | `l` | `r` | `prev` | Subarray        | `res` |
|------|-----|-----|--------|----------------|-------|
| Init | 0   | 1   | ""     | [9]            | 1     |
| 1    | 0   | 2   | ">"    | [9, 4]         | 2     |
| 2    | 0   | 3   | "<"    | [9, 4, 2]      | 3     |
| 3    | 0   | 4   | ">"    | [4, 2, 10]     | 4     |
| 4    | 0   | 5   | "<"    | [2, 10, 7, 8]  | 5     |
| Reset| 6   | 7   | ""     | [1, 9]         | 5     |

Final result: `5`.

---

### 6. Evaluate

#### Time Complexity
- Sliding window traverses the array once: **O(n)**.

#### Space Complexity
- Only constant space is used: **O(1)**.

---

### Additional Notes

#### Why This Problem is Important
- Trains understanding of sliding window techniques.
- Builds problem-solving skills for subarray problems.

#### Prerequisites
- Basic understanding of sliding window.
- Familiarity with comparison operators.

#### Industry Relevance
- Useful for real-world problems involving subarray constraints.
- Appears in competitive programming and coding interviews.

#### Follow-Up Practice Problems
- LeetCode 53: Maximum Subarray
- LeetCode 209: Minimum Size Subarray Sum
- LeetCode 3: Longest Substring Without Repeating Characters
