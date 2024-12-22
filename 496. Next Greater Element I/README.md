
# LeetCode 496: Next Greater Element I

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

#### Problem Description
You are given two distinct 0-indexed integer arrays, `nums1` and `nums2`.  
- `nums1` is a subset of `nums2`.  
- For each element in `nums1`, find the **next greater element** in `nums2`.  
- If there is no next greater element, return `-1` for that element.

#### Inputs
- `nums1`: A list of integers where `nums1 ⊆ nums2`.
- `nums2`: A list of integers.

#### Outputs
- An array `ans` of length `nums1.length`, where `ans[i]` is the next greater element of `nums1[i]` in `nums2`.

#### Constraints
- The integers in `nums2` are distinct.
- Both arrays are non-empty.

---

### **2. Match**

#### Problem Pattern
- This problem can be solved using the **Monotonic Stack** pattern, which efficiently finds the next greater element for elements in an array.

#### Data Structures
- **Stack:** To maintain a decreasing sequence of elements.
- **Dictionary:** To map each number in `nums1` to its index for quick look-up.

---

### **3. Plan**

#### Steps
1. Create a dictionary `index_map` that maps elements in `nums1` to their indices.
2. Initialize the `result` array with `-1` for all elements in `nums1` (default value when no greater element exists).
3. Use a **monotonic decreasing stack** to find the next greater element in `nums2`:
   - Traverse `nums2` from left to right.
   - For each number:
     - While the stack is not empty and the current number is greater than the stack’s top:
       - Pop the stack.
       - Use `index_map` to find the corresponding index in `nums1`.
       - Update the result array with the current number as the next greater element.
     - If the current number is in `nums1`, push it onto the stack.
4. Return the `result` array.

#### Time and Space Complexity
- **Time Complexity:** O(n + m), where `n` is the length of `nums2` and `m` is the length of `nums1`.
- **Space Complexity:** O(n), for the stack and dictionary.

---

### **4. Implement**

Here is the Python implementation with step-by-step notes.

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        # Step 1: Create a mapping from numbers in nums1 to their indices
        index_map = {n: i for i, n in enumerate(nums1)}
        # Step 2: Initialize result array with -1
        result = [-1] * len(nums1)
        # Step 3: Create an empty stack to maintain a decreasing sequence
        stack = []

        # Step 4: Traverse nums2
        for current_num in nums2:
            # Step 5: Process the stack to find the next greater element
            while stack and current_num > stack[-1]:
                val = stack.pop()
                # Find the index of val in nums1 using index_map
                idx = index_map[val]
                # Update result with the next greater element
                result[idx] = current_num
            
            # Step 6: If current_num is in nums1, push it onto the stack
            if current_num in index_map:
                stack.append(current_num)
        
        # Step 7: Return the result array
        return result
```

---

### **5. Review**

#### Dry Run with a Complex Test Case
**Input:**  
`nums1 = [4,1,2]`, `nums2 = [1,3,4,2]`  
**Expected Output:**  
`[-1, 3, -1]`

**Step-by-Step Execution:**
1. Initialize `index_map = {4: 0, 1: 1, 2: 2}` and `result = [-1, -1, -1]`.
2. Start with an empty `stack = []`.

**Processing nums2:**
- `current_num = 1`:  
  - Add `1` to stack → `stack = [1]`.

- `current_num = 3`:  
  - `3 > 1`: Pop `1` from stack, update `result[1] = 3`.  
  - Add `3` to stack → `stack = [3]`.

- `current_num = 4`:  
  - `4 > 3`: Pop `3` from stack, but `3` is not in `nums1`.  
  - Add `4` to stack → `stack = [4]`.

- `current_num = 2`:  
  - Add `2` to stack → `stack = [4, 2]`.

**Final Result:**  
`result = [-1, 3, -1]`

---

### **6. Evaluate**

#### Time Complexity
- **O(n + m):**  
  - O(n) to process `nums2` with the stack.  
  - O(m) to query `nums1` indices in `index_map`.

#### Space Complexity
- **O(n):**  
  - Space for the stack and `index_map`.

---

### **Additional Notes**

#### Why This Problem is Important
- Mastering this problem builds familiarity with monotonic stacks, a key technique for efficient computation in array-based problems.

#### Prerequisites
- Understanding of:
  - Stack operations.
  - Dictionary usage for quick look-ups.
  - Array traversal.

#### Industry Relevance
- Monotonic stacks are frequently used in scenarios like:
  - Stock price analysis.
  - Temperature comparisons.
  - Sliding window problems.

#### Follow-Up Practice Problems
- LeetCode 503: Next Greater Element II  
- LeetCode 739: Daily Temperatures  
- LeetCode 42: Trapping Rain Water
