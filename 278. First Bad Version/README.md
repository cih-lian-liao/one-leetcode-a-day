
# LeetCode 278: First Bad Version

### Problem Description

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Input:**  
- An integer `n`, the total number of versions.  
- API `isBadVersion(version)` which determines if a version is bad.

**Output:**  
- The integer representing the first bad version.

**Constraints:**  
- 1 <= bad <= n <= 2^31 - 1
- The solution must run in O(log n) time.

**Assumptions:**  
- There is at least one bad version.  
- `isBadVersion(version)` is always callable.

---

### 2. Match

- The problem can be matched to the **Binary Search** pattern:
  - We need to minimize the number of API calls.
  - Binary search reduces the search space by half in each iteration.

---

### 3. Plan

**Approach:**  
1. Initialize two pointers: `left = 1` and `right = n`.
2. Perform binary search:
   - Calculate the middle version: `mid = left + (right - left) // 2`.
   - If `isBadVersion(mid)` is `True`, move the right pointer to `mid` (because this version might be the first bad version).
   - Otherwise, move the left pointer to `mid + 1`.
3. When `left == right`, return `left` as the first bad version.

**Edge Cases:**  
- If `n = 1`, directly return 1 since it must be bad.  
- If the first bad version is at the very beginning (`1`), ensure the algorithm doesn't overshoot.

**Time Complexity:** O(log n).  
**Space Complexity:** O(1).

---

### 4. Implement

```python
def firstBadVersion(n):
    """
    Finds the first bad version in the range 1 to n using binary search.
    :type n: int
    :rtype: int
    """
    left, right = 1, n  # Step 1: Initialize pointers
    
    while left < right:  # Step 2: Binary search until left == right
        mid = left + (right - left) // 2  # Calculate mid to prevent overflow
        if isBadVersion(mid):  # Step 3: Check if mid is bad
            right = mid  # Narrow search space to left half
        else:
            left = mid + 1  # Narrow search space to right half
    
    return left  # Step 4: Return the first bad version
```

**Notes on Implementation:**  
- The calculation of `mid` uses `left + (right - left) // 2` to avoid potential overflow for large values of `n`.
- The condition `while left < right` ensures that the pointers converge correctly.
- Returning `left` at the end is valid because `left` and `right` will point to the same value.

---

### 5. Review

**Example Dry Run:**  
**Input:** `n = 5`, `first_bad_version = 4`.  

1. Initialize: `left = 1`, `right = 5`.  
2. Iteration 1:  
   - `mid = (1 + 5) // 2 = 3`.  
   - `isBadVersion(3)` returns `False`.  
   - Update: `left = 4`.  
3. Iteration 2:  
   - `mid = (4 + 5) // 2 = 4`.  
   - `isBadVersion(4)` returns `True`.  
   - Update: `right = 4`.  
4. Convergence: `left == right == 4`.  
**Output:** `4`.

**Edge Case Testing:**  
- If `n = 1` and the first version is bad: `isBadVersion(1)` returns `True`, and the function directly returns `1`.

---

### 6. Evaluate

**Time Complexity:**  
- Each iteration halves the search space: \( O(\log n) \).

**Space Complexity:**  
- Only constant extra space is used for pointers: \( O(1) \).

**Optimizations:**  
- The implementation is already optimal using binary search.

---

### Additional Notes

#### Why This Problem is Important
- Teaches the fundamental use of binary search for optimization.  
- Helps practice minimizing API calls or expensive operations.

#### Prerequisites for Practicing This Problem
- Understanding of binary search and its application.
- Familiarity with pointer manipulation and edge case handling.

#### Industry Relevance
- Frequently asked in technical interviews to test binary search knowledge.
- Highlights skills in problem optimization under constraints.

#### Follow-up Practice Problems
- [LeetCode 374: Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
- [LeetCode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)
- [LeetCode 852: Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)
