
# LeetCode Problem 503: Next Greater Element II

### Problem Description

You are given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`). For each element in `nums`, find its next greater element. If it doesn't exist, return `-1`.

The next greater element of a number `x` is the first greater number to its traversal order next in the array. Note that this circular behavior means you may need to look through the array again.

#### Example 1:
**Input:**  
`nums = [1, 2, 1]`  
**Output:**  
`[2, -1, 2]`  
**Explanation:**  
- For `nums[0] = 1`, the next greater element is `2`.  
- For `nums[1] = 2`, there is no next greater element.  
- For `nums[2] = 1`, the next greater element is `2`.

#### Example 2:
**Input:**  
`nums = [5, 4, 3, 2, 1]`  
**Output:**  
`[-1, 5, 5, 5, 5]`  

#### Constraints:
- `1 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- **Inputs:**
  - A circular array `nums` of integers.
- **Outputs:**
  - An array `res` where `res[i]` is the next greater element of `nums[i]` or `-1` if it doesn't exist.
- **Constraints:**
  - The array is circular. This means after the last element, it wraps around to the first element.
  - We must handle up to `10^4` elements efficiently.
    
---

### 2. Match
- This problem matches the **"Next Greater Element" pattern**, commonly solved using a **monotonic stack**.
- The circular nature of the array means we simulate by traversing the array twice (`2 * n` iterations).

---

### 3. Plan
1. Initialize a `res` array with `-1` for all elements.
2. Use a monotonic stack to store indices of elements in descending order of their values.
3. Traverse the array twice:
   - For each element, while the stack is not empty and the current element is greater than the element at the top index of the stack:
     - Pop the top index from the stack and update the `res` array at that index with the current element.
   - If we're in the first traversal, push the current index onto the stack.
4. Return the `res` array.

**Time Complexity:**  
O(n) – Each element is pushed and popped from the stack at most once.

**Space Complexity:**  
O(n) – The stack may contain up to `n` elements in the worst case.

---

### 4. Implement
```python
def nextGreaterElements(nums):
    n = len(nums)
    res = [-1] * n  # Step 1: Initialize result array with -1
    stack = []      # Step 2: Initialize an empty monotonic stack

    # Step 3: Traverse the array twice to simulate circular behavior
    for i in range(2 * n):  # Loop through twice
        current_index = i % n  # Map to the circular index
        # Step 4: Update result array for indices in stack
        while stack and nums[stack[-1]] < nums[current_index]:
            top_index = stack.pop()
            res[top_index] = nums[current_index]
        # Step 5: Push index onto stack only in the first traversal
        if i < n:
            stack.append(current_index)
    
    # Step 6: Return the result array
    return res
```

#### Step-by-Step Implementation Notes:
1. **Initialization:**
   - `res = [-1] * n`: Each element starts with `-1`, meaning no next greater element is found yet.
   - `stack = []`: The stack stores indices of `nums` in a descending order.
2. **Traversal:**
   - The loop runs for `2 * n` iterations to account for the circular behavior.
   - Use `i % n` to map the traversal index to the circular array.
3. **Updating Results:**
   - While the stack is not empty and the current element is greater than the stack's top element:
     - Pop the index and update `res[top_index]` with the current element.
4. **Push Indices:**
   - Only push indices during the first traversal (`i < n`) to avoid duplicating work.
5. **Return Results:**
   - After traversing, `res` contains the next greater elements.

---

### 5. Review
#### Dry Run Example:
Input: `nums = [1, 2, 1]`

1. **First Traversal:**
   - `i = 0`: Stack = `[0]`  
   - `i = 1`: Pop `0` from stack, `res[0] = 2`, Stack = `[1]`  
   - `i = 2`: Stack = `[1, 2]`  

2. **Second Traversal:**
   - `i = 3`: No updates, Stack = `[1, 2]`  
   - `i = 4`: No updates, Stack = `[1, 2]`  
   - `i = 5`: Pop `2` from stack, `res[2] = 2`, Stack = `[1]`  

Final Result: `[2, -1, 2]`

---

### 6. Evaluate
- **Time Complexity:**  
  O(n) – Each index is processed (pushed and popped) at most once.
- **Space Complexity:**  
  O(n) – Stack space for storing indices.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It practices the **monotonic stack**, a critical pattern for interview problems.
- Handling circular arrays efficiently is a valuable skill.

#### Prerequisites for Practicing This Problem
- Understanding of monotonic stacks.
- Familiarity with modular arithmetic for circular arrays.

#### Industry Relevance
- Circular array problems are relevant in scheduling and cyclic data structures.
- Monotonic stack is widely used in stock span problems, temperature monitoring, etc.

#### Follow-up Practice Problems
1. **LeetCode 496:** Next Greater Element I  
2. **LeetCode 739:** Daily Temperatures  
3. **LeetCode 901:** Online Stock Span  
4. **LeetCode 42:** Trapping Rain Water  
