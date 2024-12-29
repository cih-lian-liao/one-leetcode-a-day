
# LeetCode 1343: Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

**Problem Statement**:  
Given an integer array `arr` and two integers `k` and `threshold`, return the number of sub-arrays of size `k` whose average is greater than or equal to `threshold`.

**Key Points**:
- **Input**:
  - `arr`: List of integers.
  - `k`: Integer representing the size of the sub-array.
  - `threshold`: Integer representing the threshold average.
- **Output**:
  - An integer indicating the count of sub-arrays satisfying the condition.

- **Constraints**:
  - `1 <= arr.length <= 10^5`
  - `1 <= arr[i] <= 10^4`
  - `1 <= k <= arr.length`
  - `0 <= threshold <= 10^4`

**Clarifications**:
- A sub-array is a contiguous part of the array.
- The condition can be rewritten as `sum(subarray) >= threshold * k`.

---

### **2. Match**

**Pattern**:  
This problem is a classic example of a **sliding window** technique:
- Fixed-size sliding window to efficiently compute the sum of sub-arrays of size `k`.

**Relevant Data Structures/Algorithms**:
- Array traversal.
- Sliding window for efficient computation.

---

### **3. Plan**

**Step-by-Step Plan**:
1. Compute the **required sum**: `required_sum = threshold * k`.
2. Use the **sliding window** method:
   - Initialize the first window (sum of the first `k` elements).
   - Slide the window across the array:
     - Subtract the element that is sliding out.
     - Add the element that is sliding in.
     - Check if the updated window sum meets or exceeds `required_sum`.
   - Increment the counter for each valid window.
3. Return the count.

**Edge Cases**:
- Small array where `len(arr) == k`.
- Array with all identical elements.
- High `threshold` that no sub-array can satisfy.

**Time Complexity**:  
- O(n), where `n` is the length of the array (single traversal using sliding window).

**Space Complexity**:  
- O(1), no additional data structures are used.

---

### **4. Implement**

```python
from typing import List

class Solution:
    def numOfSubarrays(self, arr: List[int], k: int, threshold: int) -> int:
        # Step 1: Calculate the required sum
        required_sum = threshold * k
        count = 0  # Initialize the count for valid sub-arrays

        # Step 2: Initialize the first window sum
        window_sum = sum(arr[:k])  # Sum of the first 'k' elements

        # Step 3: Check the first window
        if window_sum >= required_sum:
            count += 1

        # Step 4: Slide the window across the array
        for i in range(k, len(arr)):
            # Update the window sum: Subtract outgoing element, add incoming element
            window_sum = window_sum - arr[i - k] + arr[i]
            
            # Check if the current window meets the condition
            if window_sum >= required_sum:
                count += 1

        # Step 5: Return the count of valid sub-arrays
        return count
```

**Implementation Notes**:
- `window_sum`: Keeps track of the sum of the current window.
- `i - k`: Index of the outgoing element when the window slides.

---

### **5. Review**

**Dry Run with Example**:  
**Input**:
```python
arr = [2, 1, 3, 4, 1, 2, 5, 8]
k = 3
threshold = 4
```

**Execution**:
1. `required_sum = 4 * 3 = 12`.
2. Initialize `window_sum = sum([2, 1, 3]) = 6`.
   - First window `[2, 1, 3]`: Does not satisfy `window_sum >= 12`.
3. Slide window:
   - Update: `window_sum = 6 - 2 + 4 = 8`.
     - Second window `[1, 3, 4]`: Does not satisfy.
   - Update: `window_sum = 8 - 1 + 1 = 8`.
     - Third window `[3, 4, 1]`: Does not satisfy.
   - Update: `window_sum = 8 - 3 + 2 = 7`.
     - Fourth window `[4, 1, 2]`: Does not satisfy.
   - Update: `window_sum = 7 - 4 + 5 = 8`.
     - Fifth window `[1, 2, 5]`: Does not satisfy.
   - Update: `window_sum = 8 - 1 + 8 = 15`.
     - Sixth window `[2, 5, 8]`: **Satisfies** `window_sum >= 12`. Increment count.
4. Total valid sub-arrays: 1.

**Output**:
```python
Solution().numOfSubarrays(arr, k, threshold)  # Output: 1
```

---

### **6. Evaluate**

**Time Complexity**:
- O(n): Single traversal of the array using the sliding window technique.

**Space Complexity**:
- O(1): Constant space usage.

**Optimizations**:
- Avoid repeated summation by maintaining a running sum for the sliding window.

---

### **Additional Notes**

**Why This Problem is Important**:
- Demonstrates the efficient use of sliding window for fixed-size problems.
- Strengthens understanding of array traversal and optimization techniques.

**Prerequisites for Practicing**:
- Basic understanding of arrays and loops.
- Familiarity with the sliding window technique.

**Industry Relevance**:
- Common in real-world applications like signal processing, time-series analysis, and rolling computations.

**Follow-Up Practice Problems**:
1. LeetCode 209: Minimum Size Subarray Sum.
2. LeetCode 1004: Max Consecutive Ones III.
3. LeetCode 76: Minimum Window Substring.
