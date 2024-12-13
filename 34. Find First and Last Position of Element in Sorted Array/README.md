
# **LeetCode 34: Find First and Last Position of Element in Sorted Array**


### Problem Statement

Given an array of integers `nums` sorted in ascending order, and a target value `target`, find the starting and ending position of `target` in the array. If `target` is not found, return `[-1, -1]`.

**Example Input**:
```plaintext
nums = [5,7,7,8,8,10], target = 8
```

**Example Output**:
```plaintext
[3, 4]
```

---

##### **UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate**


### **1. Understand**

- **Input**:
  - A sorted list `nums` of integers.
  - An integer `target`.
- **Output**:
  - A list `[start, end]` where:
    - `start` is the index of the first occurrence of `target`.
    - `end` is the index of the last occurrence of `target`.
    - Return `[-1, -1]` if `target` is not in `nums`.
- **Constraints**:
  - \(0 \leq nums.length \leq 10^5\)
  - \(-10^9 \leq nums[i], target \leq 10^9\)
- **Ambiguity**:
  - What happens if the list is empty? → Return `[-1, -1]`.

---

### **2. Match**

- **Problem Type**:
  - Search for elements in a sorted list.
- **Pattern**:
  - Utilize **binary search** for efficiency due to sorted order.
- **Data Structures**:
  - No additional data structures are needed; use indices to track results.

---

### **3. Plan**

1. Implement a helper function `findFirst` to locate the first occurrence of `target` using binary search:
   - Narrow the search space to the leftmost occurrence of `target`.
2. Implement another helper function `findLast` to locate the last occurrence of `target`:
   - Narrow the search space to the rightmost occurrence of `target`.
3. Use these two functions to populate the `start` and `end` indices:
   - If `target` is not found, both functions should return `-1`.

**Pseudocode**:
```plaintext
Define findFirst(nums, target):
    Initialize left = 0, right = len(nums) - 1
    While left <= right:
        Compute mid = (left + right) // 2
        If nums[mid] == target:
            Update right = mid - 1 (move left to find the first occurrence)
        Else if nums[mid] < target:
            Update left = mid + 1
        Else:
            Update right = mid - 1
    Return the index or -1

Define findLast(nums, target):
    Initialize left = 0, right = len(nums) - 1
    While left <= right:
        Compute mid = (left + right) // 2
        If nums[mid] == target:
            Update left = mid + 1 (move right to find the last occurrence)
        Else if nums[mid] < target:
            Update left = mid + 1
        Else:
            Update right = mid - 1
    Return the index or -1

Call findFirst and findLast, and return their results as [start, end].
```

---

### **4. Implement**

```python
def searchRange(nums, target):
    def findFirst(nums, target):
        left, right = 0, len(nums) - 1
        first = -1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                first = mid
                right = mid - 1  # Move left to find the first occurrence
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return first

    def findLast(nums, target):
        left, right = 0, len(nums) - 1
        last = -1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                last = mid
                left = mid + 1  # Move right to find the last occurrence
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return last

    start = findFirst(nums, target)
    end = findLast(nums, target)
    return [start, end]
```

---

### **5. Review**

**Test Case**:
```plaintext
nums = [5,7,7,8,8,10], target = 8
```

**Dry Run**:
- Call `findFirst`:
  - Binary search narrows to `mid = 3` (first occurrence of 8).
- Call `findLast`:
  - Binary search narrows to `mid = 4` (last occurrence of 8).

**Output**:
```plaintext
[3, 4]
```

---

### **6. Evaluate**

- **Time Complexity**:
  - Binary search is \(O(\log n)\).
  - Two binary searches result in \(O(\log n) + O(\log n) = O(\log n)\).
- **Space Complexity**:
  - No extra space used; \(O(1)\).

---

### Additional Notes

#### **Why This Problem is Important**
- Tests binary search skills.
- Common in search-related problems in large datasets.

#### **Prerequisites**
- Binary search understanding.
- List indexing.

#### **Industry Relevance**
- Searching large, sorted datasets efficiently.
- Applications in database indexing and range queries.

#### **Follow-up Practice Problems**
1. LeetCode 33: Search in Rotated Sorted Array
2. LeetCode 74: Search a 2D Matrix
3. LeetCode 81: Search in Rotated Sorted Array II
