# LeetCode Problem 128: Longest Consecutive Sequence

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

- **Problem Statement:**  
  Given an unsorted array of integers `nums`, find the length of the longest sequence of consecutive elements. The sequence doesn't have to be contiguous in the array but should be consecutive in value.

- **Input:**  
  An integer array `nums`.

- **Output:**  
  An integer representing the length of the longest consecutive elements sequence.

- **Constraints:**  
  - `0 <= nums.length <= 10^5`
  - `-10^9 <= nums[i] <= 10^9`
  - Time complexity requirement: O(n)

- **Ambiguities Clarified:**  
  - An empty array returns `0`.
  - Duplicate numbers are allowed in `nums` but do not extend the sequence.

---

### **2. Match**

- **Problem Type:**  
  This is a sequence detection problem requiring efficient lookup operations.

- **Relevant Patterns:**  
  Use a **HashSet** for fast element lookups. This helps in identifying the start of a sequence in O(1) time per operation.

- **Data Structures/Algorithms to Use:**  
  - HashSet for constant-time lookups.
  - Iterative approach to traverse the elements.

---

### **3. Plan**

1. **Convert the array into a set:**  
   This helps to eliminate duplicates and allows quick access to any element.

2. **Iterate through the set:**  
   For each element, check if it is the start of a sequence. A number is the start if `num - 1` is not in the set.

3. **Calculate the sequence length:**  
   From the start of the sequence, count the consecutive numbers (`num + 1`, `num + 2`, etc.) in the set.

4. **Track the longest sequence length:**  
   Keep updating the maximum length encountered.

5. **Edge Case Handling:**  
   - If `nums` is empty, return `0`.
   - For arrays with one element, the result is `1`.

6. **Time Complexity Analysis:**  
   - Converting the array to a set: O(n)
   - Iterating and checking sequences: O(n)
   - Total: O(n)

7. **Space Complexity Analysis:**  
   - Storing elements in a set: O(n)

---

### **4. Implement**

```python
def longestConsecutive(nums):
    # Edge case: Empty array
    if not nums:
        return 0
    
    # Step 1: Convert array to a set for O(1) lookups
    num_set = set(nums)
    longest_streak = 0

    # Step 2: Iterate through the set
    for num in num_set:
        # Step 3: Check if the number is the start of a sequence
        if num - 1 not in num_set:  # num is the start of a sequence
            current_num = num
            current_streak = 1

            # Step 4: Count the length of the sequence
            while current_num + 1 in num_set:
                current_num += 1
                current_streak += 1
            
            # Step 5: Update the longest streak
            longest_streak = max(longest_streak, current_streak)

    return longest_streak
```

---

### **5.Review**

**Debugging:**  
1. **Edge Case Testing:**  
   - `nums = []` → Output: `0`
   - `nums = [1]` → Output: `1`
   - `nums = [1, 2, 0, 1]` → Output: `3`

2. **Complex Case Dry Run:**  
   Input: `nums = [100, 4, 200, 1, 3, 2]`  
   - `num_set = {1, 2, 3, 4, 100, 200}`
   - Iteration:  
     - `num = 100` → Length = `1`
     - `num = 4` → Skip (not the start)
     - `num = 200` → Length = `1`
     - `num = 1` → Length = `4`  
   - Result: `4`

---

### **6. Evaluate**

- **Time Complexity:**  
  O(n) - Iterates over each number once, and each sequence lookup is O(1).

- **Space Complexity:**  
  O(n) - Space required for the HashSet.

---

### **Additional Notes**

1. **Why This Problem is Important:**  
   - Demonstrates efficient usage of HashSet for sequence problems.
   - Frequently tested in coding interviews to assess problem-solving and optimization skills.

2. **Prerequisites for Practicing This Problem:**  
   - HashSet operations (insertion, lookup).
   - Understanding of time complexity.

3. **Industry Relevance:**  
   - Useful for applications requiring analysis of unordered data.
   - Example: Tracking user login streaks in web services.

4. **Follow-up Practice Problems:**  
   - LeetCode 121: Best Time to Buy and Sell Stock.
   - LeetCode 76: Minimum Window Substring.
   - LeetCode 523: Continuous Subarray Sum.
