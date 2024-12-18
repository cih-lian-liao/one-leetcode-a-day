# LeetCode Problem 287: Find the Duplicate Number

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Problem Statement:**
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive, there is only one repeated number. Find and return this repeated number.

**Inputs:**
- `nums`: A list of integers of size `n + 1`, where integers are in the range `[1, n]`.

**Outputs:**
- An integer representing the repeated number.

**Constraints:**
1. The input list `nums` contains exactly one repeated number.
2. The repeated number may appear more than once.
3. Must solve the problem without modifying the array.
4. Must use constant extra space.

**Assumptions:**
- There is always a single duplicate in the array.

---

### 2. Match

This problem involves identifying a duplicate in an array while adhering to specific constraints (constant space and unmodifiable array). 

**Related Patterns:**
- Cycle detection (Floyd's Tortoise and Hare algorithm).
- Linked list cycle problems.

**Reason to Match:**
- Treat the array as a directed graph where each value points to the index it represents. The presence of a duplicate implies a cycle exists in this graph.

---

### 3. Plan

We use Floyd's Tortoise and Hare algorithm to detect the cycle and find the duplicate number.

**Steps:**
1. Initialize two pointers, `slow` and `fast`, both set to the starting index `0`.
2. Use a loop to move `slow` one step and `fast` two steps at a time until they meet (cycle detected).
3. Reset one of the pointers (`slow2`) to the start of the array, and keep the other pointer (`slow`) where they met.
4. Move both pointers one step at a time. When they meet again, the meeting point is the duplicate number.

**Edge Cases:**
- Smallest input size (`nums = [1, 1]`).
- Duplicate number appears multiple times.

**Time Complexity:** O(n)
- Traverses the array in linear time.

**Space Complexity:** O(1)
- Uses only a constant amount of space for pointers.

---

### 4. Implement

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # Step 1: Detect cycle using slow and fast pointers
        slow, fast = 0, 0
        
        while True:
            slow = nums[slow]  # Move slow pointer one step
            fast = nums[nums[fast]]  # Move fast pointer two steps
            if slow == fast:  # Cycle detected
                break

        # Step 2: Find entrance of the cycle
        slow2 = 0  # Reset one pointer to the start
        
        while True:
            slow = nums[slow]  # Move both pointers one step
            slow2 = nums[slow2]
            if slow == slow2:  # Meeting point is the duplicate number
                return slow
```

**Implementation Notes:**
- `slow` and `fast` pointers are initially set to `0` (start of the array).
- `nums[slow]` simulates moving to the next node in a linked list.
- The meeting point inside the cycle gives an entry point to locate the duplicate.

---

### 5. Review

#### Test Cases:
1. **Input:** `nums = [1, 3, 4, 2, 2]`  
   **Output:** `2`

2. **Input:** `nums = [3, 1, 3, 4, 2]`  
   **Output:** `3`

3. **Input:** `nums = [1, 1]`  
   **Output:** `1`

4. **Input:** `nums = [1, 1, 2]`  
   **Output:** `1`

#### Dry Run:
For `nums = [1, 3, 4, 2, 2]`:
- **Initialization:**
  - `slow = 0`, `fast = 0`.
- **Cycle Detection:**
  - First iteration: `slow = nums[slow] = nums[0] = 1`, `fast = nums[nums[fast]] = nums[nums[0]] = nums[1] = 3`.
  - Second iteration: `slow = nums[1] = 3`, `fast = nums[nums[3]] = nums[2] = 4`.
  - Third iteration: `slow = nums[3] = 2`, `fast = nums[nums[4]] = nums[2] = 2`.
  - `slow == fast`, cycle detected.
- **Finding Duplicate:**
  - Reset `slow2 = 0`.
  - First iteration: `slow = nums[2] = 4`, `slow2 = nums[0] = 1`.
  - Second iteration: `slow = nums[4] = 2`, `slow2 = nums[1] = 3`.
  - Third iteration: `slow = nums[2] = 2`, `slow2 = nums[2] = 2`.
  - `slow == slow2`, duplicate is `2`.

---

### 6. Evaluate

#### Time Complexity:
- O(n): Each pointer traverses the array a linear number of times.

#### Space Complexity:
- O(1): Uses constant space for pointers.

#### Optimizations:
- This is the most optimal solution given the constraints (constant space and unmodifiable array).

---

### Additional Notes

#### Importance of This Problem:
- Illustrates how cycle detection algorithms can be applied outside of traditional linked list problems.
- Combines graph theory and array manipulation.

#### Prerequisites:
- Understanding of Floyd's Tortoise and Hare algorithm.
- Familiarity with pointers and array traversal.

#### Industry Relevance:
- Demonstrates problem-solving techniques for array manipulation and graph representation.
- Useful in scenarios like detecting loops in data structures or navigation systems.

#### Follow-Up Practice Problems:
1. [LeetCode 142: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
2. [LeetCode 141: Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
3. [LeetCode 442: Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
