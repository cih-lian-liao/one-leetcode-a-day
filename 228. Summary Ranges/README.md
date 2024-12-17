
# LeetCode 228: Summary Ranges

### **Problem Description**

Given a **sorted unique integer array** `nums`, return the **smallest sorted list of ranges** that cover all the numbers in the array exactly. That is, each element of `nums` is covered by exactly one of the ranges, and there is no gap in between the ranges.

A range `[a,b]` is represented as:
- `"a->b"` if `a != b`
- `"a"` if `a == b`

### **Example 1**
- **Input**: `nums = [0, 1, 2, 4, 5, 7]`
- **Output**: `["0->2", "4->5", "7"]`

### **Example 2**
- **Input**: `nums = [0, 2, 3, 4, 6, 8, 9]`
- **Output**: `["0", "2->4", "6", "8->9"]`

### **Example 3**
- **Input**: `nums = []`
- **Output**: `[]`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### **Inputs**
- `nums`: A sorted array of integers (may be empty, contains unique elements).

#### **Outputs**
- A list of strings where each string represents a range.

#### **Constraints**
- The array is sorted in **increasing order**.
- No duplicate elements are present.
- The result must have minimal ranges.

#### **Clarifications**
1. If the input is empty, the output is an empty list.
2. If an integer has no consecutive neighbor, it is treated as a range with a single element.

---

### **2. Match**

This problem can be matched to **array traversal problems**:
- Identify consecutive ranges while iterating.
- A range is "broken" when two numbers are not consecutive (`nums[i] != nums[i-1] + 1`).

**Relevant data structure**: A simple list for storing ranges.

**Algorithm pattern**: 
- Iterate through the array.
- Use pointers (`start` and `nums[i-1]`) to track ranges.

---

### **3. Plan**

#### **Steps**
1. Handle the edge case where `nums` is empty. Return `[]`.
2. Initialize an empty `result` list to store ranges and a variable `start` to track the beginning of the current range.
3. Iterate over the array starting from the second element (`i = 1`).
   - If `nums[i]` is **not consecutive** with `nums[i-1]`:
     - Add the range from `start` to `nums[i-1]` to `result`.
     - Update `start` to the current element (`nums[i]`).
4. After the loop, add the last range to the result.
5. Return the result.

#### **Edge Cases**
1. `nums = []` -> Output: `[]`
2. `nums = [1]` -> Output: `["1"]`
3. `nums = [1, 3]` -> Output: `["1", "3"]`
4. `nums = [1, 2, 3, 5, 6]` -> Output: `["1->3", "5->6"]`

#### **Time Complexity**
- `O(n)` where `n` is the length of the input array `nums`.

#### **Space Complexity**
- `O(1)` additional space, as we store results in the output list.

---

### **4. Implement**

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        # Step 1: Handle empty input
        if not nums:
            return []
        
        # Step 2: Initialize result and starting point
        result = []
        start = nums[0]
        
        # Step 3: Traverse the array
        for i in range(1, len(nums)):
            # If not consecutive
            if nums[i] != nums[i - 1] + 1:
                if start == nums[i - 1]:
                    result.append(str(start))  # Single number
                else:
                    result.append(f"{start}->{nums[i - 1]}")  # Range
                start = nums[i]  # Update start
        
        # Step 4: Add the final range
        if start == nums[-1]:
            result.append(str(start))
        else:
            result.append(f"{start}->{nums[-1]}")
        
        # Step 5: Return result
        return result
```

#### **Implementation Notes**
1. **Initialization**: Start with the first number of the range.
2. **Checking Consecutiveness**: Use the condition `nums[i] != nums[i - 1] + 1`.
3. **Adding Ranges**: Check if the range has one or multiple elements before adding it to the result.
4. **Final Range**: Always add the last range after the loop.

---

### **5. Review**

#### **Test Case 1**
Input: `nums = [0, 1, 2, 4, 5, 7]`  
Output: `["0->2", "4->5", "7"]`

**Dry Run**:
1. Initialize `start = 0`, `result = []`.
2. Loop through `nums`:
   - `i = 1`: Consecutive (`1` is `0+1`), continue.
   - `i = 2`: Consecutive (`2` is `1+1`), continue.
   - `i = 3`: Not consecutive (`4 != 2+1`), add `"0->2"` to `result`.
     - Update `start = 4`.
   - `i = 4`: Consecutive (`5` is `4+1`), continue.
   - `i = 5`: Not consecutive (`7 != 5+1`), add `"4->5"` to `result`.
     - Update `start = 7`.
3. End of loop: Add `"7"` to `result`.
4. Return `["0->2", "4->5", "7"]`.

---

### **6. Evaluate**

#### **Time Complexity**
- **O(n)**: Single traversal of the array.

#### **Space Complexity**
- **O(1)**: No additional storage beyond the output list.

#### **Optimization**
- The solution is already optimal for its constraints.

---

### **Additional Notes**

#### **Why This Problem is Important**
- Tests the ability to handle sorted data and detect patterns in arrays.
- Demonstrates edge case handling and array traversal techniques.

#### **Prerequisites for Practicing**
- Understanding of array traversal.
- Familiarity with string formatting and list manipulation in Python.

#### **Industry Relevance**
- Range processing is a common task in analytics and data pipelines (e.g., log analysis, summarization).

#### **Follow-up Practice Problems**
1. **Merge Intervals (LeetCode 56)** - Handling overlapping intervals.
2. **Missing Ranges (LeetCode 163)** - Identifying gaps in ranges.
3. **Insert Interval (LeetCode 57)** - Adding a range into an existing set of intervals.
