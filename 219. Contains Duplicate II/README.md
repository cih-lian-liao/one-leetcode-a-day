
# LeetCode 219: Contains Duplicate II

### Problem Statement

Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that:

1. `nums[i] == nums[j]`
2. `|i - j| <= k`

If no such indices exist, return `false`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs:**
- `nums`: A list of integers, where `1 <= nums.length <= 10^5` and `-10^9 <= nums[i] <= 10^9`.
- `k`: An integer where `0 <= k <= 10^5`.

**Outputs:**
- Return `true` if there exist two indices `i` and `j` such that:
  - `nums[i] == nums[j]`
  - `|i - j| <= k`
- Otherwise, return `false`.

**Constraints:**
1. Each element in `nums` can have duplicates, but the distance between their indices must not exceed `k`.
2. If `k == 0`, return `false` because no two indices can have a distance of 0.

---

### 2. Match

**Problem Type:**
- This is a **hashing problem** focused on detecting duplicate elements with a constraint on index distance.

**Patterns Recognized:**
- Use a **hash table** (dictionary) to store the last seen index of each element in `nums`.
- Compare the current index with the stored index for duplicates.

**Data Structures:**
- Dictionary: Maps each unique number to its last seen index.

---

### 3. Plan

**Approach:**
1. Initialize an empty dictionary `index_map` to store the last seen index of each number in `nums`.
2. Loop through `nums` using `enumerate` to access both index and value.
3. For each number:
   - If the number exists in `index_map` and the difference between the current index and the stored index is `<= k`, return `true`.
   - Otherwise, update the dictionary with the current index for the number.
4. If no valid pair is found, return `false`.

**Pseudocode:**
```text
Initialize index_map as an empty dictionary.
For each number (num) in nums with index (i):
    If num exists in index_map and (i - index_map[num] <= k):
        Return true
    Update index_map[num] = i
Return false
```

**Time Complexity:** O(n)  
- Each number is processed once, and dictionary operations (lookup and update) are O(1) on average.

**Space Complexity:** O(n)  
- The dictionary stores at most `n` unique numbers.

---

### 4. Implement

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        
        index_map = {}  # key: value -> num: index

        # Iterate through the array with index and value
        for i, num in enumerate(nums):
            # Check if the number exists in the dictionary and satisfies the condition
            if num in index_map and i - index_map[num] <= k:
                return True

            # Update the dictionary with the current index
            index_map[num] = i

        # No valid pair found
        return False
```

**Implementation Notes:**
1. **Initialization:** Use an empty dictionary `index_map` to store each number and its last seen index.
2. **Iteration:** Use `enumerate` to loop through both indices and values.
3. **Condition Check:** For each number, check if it exists in the dictionary and whether the index difference satisfies the condition.
4. **Update:** Update the dictionary to store the latest index for the current number.

---

### 5. Review

**Edge Cases:**
1. **Empty array or single element:** Return `false` since no duplicates exist.
2. **k = 0:** Always return `false` because the distance condition cannot be satisfied.
3. **All elements unique:** Return `false` since no duplicates exist.
4. **All elements the same:** Return `true` if `k >= len(nums) - 1`.

**Dry Run Example:**

Input:
```python
nums = [1, 2, 3, 1]
k = 3
```

Step-by-step:
1. Initialize `index_map = {}`.
2. First iteration (`i=0`, `num=1`):  
   - `1` not in `index_map`, update `index_map = {1: 0}`.
3. Second iteration (`i=1`, `num=2`):  
   - `2` not in `index_map`, update `index_map = {1: 0, 2: 1}`.
4. Third iteration (`i=2`, `num=3`):  
   - `3` not in `index_map`, update `index_map = {1: 0, 2: 1, 3: 2}`.
5. Fourth iteration (`i=3`, `num=1`):  
   - `1` in `index_map` and `i - index_map[1] = 3 - 0 = 3 <= k`.
   - Return `true`.

Output: `true`

---

### 6. Evaluate

**Time Complexity:** O(n)  
- Single loop through `nums` and O(1) dictionary operations.

**Space Complexity:** O(n)  
- Dictionary stores at most `n` unique elements.

**Optimizations:**
- This is the optimal solution for this problem. For large arrays where `k` is small, consider a sliding window approach with a set to further optimize space.

---

### Additional Notes

#### Why This Problem is Important
- It teaches efficient duplicate detection under constraints, which is essential for real-world scenarios like log monitoring, fraud detection, or caching.

#### Prerequisites for Practicing This Problem
- Basic knowledge of dictionaries in Python.
- Familiarity with array traversal and handling constraints.

#### Industry Relevance
- Used in applications requiring duplicate detection and distance-based validations, such as data deduplication or log analysis.

#### Follow-up Practice Problems
1. LeetCode 1: Two Sum
2. LeetCode 3: Longest Substring Without Repeating Characters
3. LeetCode 76: Minimum Window Substring
4. LeetCode 128: Longest Consecutive Sequence
