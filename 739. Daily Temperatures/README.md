
# 739. Daily Temperatures

### Problem Statement
Given an array `temperatures` representing daily temperatures, return an array `answer` where `answer[i]` is the number of days until a warmer temperature. If no warmer temperature exists, set `answer[i]` to `0`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
- **Input**:
  - An integer array `temperatures` where `1 <= len(temperatures) <= 10^5` and `30 <= temperatures[i] <= 100`.
- **Output**:
  - An integer array `answer` of the same length, where each element represents the number of days until a warmer temperature or `0` if no such day exists.
- **Constraints**:
  - Must return results in a time-efficient manner, given the constraints on input size.

- **Clarifications**:
  - If there are multiple days with the same temperature, we only consider the next warmer day.
  - The input array will always have valid temperatures within the specified range.

---

### 2. Match
- **Pattern**:
  - This is a "Next Greater Element" problem, but adapted to arrays with specific constraints.
- **Data Structures**:
  - A **monotonic stack** will be used to efficiently find the next greater element for each day.
  - The stack will store either (temperature, index) pairs or just indices.

---

### 3. Plan
#### **Solution 1: Using a Separate Result Array**
1. Initialize a `result` array with zeros of the same length as `temperatures`.
2. Create an empty stack to store pairs of `(temp, index)`.
3. Iterate through `temperatures`:
   - For each temperature, check if it is greater than the temperature at the top of the stack:
     - If true, calculate the difference between the current index and the stored index. Update `result[stored_index]`.
     - Pop the stack and repeat until the condition is false.
   - Append the current `(temp, index)` to the stack.
4. Return the `result` array.

#### **Solution 2: Modifying the Input Array Directly**
1. Use the input array `temperatures` as the result array, eliminating the need for an extra space.
2. Create an empty stack to store indices of `temperatures`.
3. Iterate through `temperatures`:
   - While the stack is not empty and the current temperature is greater than the temperature at the index stored at the top of the stack:
     - Pop the top index from the stack.
     - Update the `temperatures` array at the popped index to the difference between the current index and the popped index.
   - Append the current index to the stack.
4. After the loop, set all indices in the stack to `0`, as they do not have any warmer days.
5. Return the modified `temperatures` array.

---

### 4. Implement

#### **Solution 1: Using a Separate Result Array**
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        result = [0] * len(temperatures)
        stack = []  # pair: (temp, index)

        for i, t in enumerate(temperatures):
            # While stack is not empty and the current temperature is greater than the top stack temperature
            while stack and t > stack[-1][0]:
                stack_temp, stack_ind = stack.pop()
                result[stack_ind] = i - stack_ind  # Calculate days difference
            # Add current (temperature, index) to the stack
            stack.append((t, i))

        return result
```

#### **Solution 2: Modifying the Input Array Directly**
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack = []  # Stores indices

        for i, t in enumerate(temperatures):
            # While stack is not empty and the current temperature is greater than the temperature at top index
            while stack and t > temperatures[stack[-1]]:
                stack_ind = stack.pop()
                temperatures[stack_ind] = i - stack_ind  # Update input array directly
            stack.append(i)
        
        # For remaining indices in the stack, set their value to 0
        while stack:
            temperatures[stack.pop()] = 0

        return temperatures
```

---

### 5. Review
#### **Dry Run Example**
**Input**:
```python
temperatures = [73, 74, 75, 71, 69, 72, 76, 73]
```

**Dry Run for Solution 1**:
- Initialize: `result = [0, 0, 0, 0, 0, 0, 0, 0]`, `stack = []`.
- Step through the array:
  - Day 0: Add `(73, 0)` to stack.
  - Day 1: `74 > 73`, pop `(73, 0)`, update `result[0] = 1 - 0 = 1`. Add `(74, 1)`.
  - Day 2: `75 > 74`, pop `(74, 1)`, update `result[1] = 2 - 1 = 1`. Add `(75, 2)`.
  - Day 3: `71 <= 75`, add `(71, 3)`.
  - Day 4: `69 <= 71`, add `(69, 4)`.
  - Day 5: `72 > 69`, pop `(69, 4)`, update `result[4] = 5 - 4 = 1`. Repeat for `(71, 3)`, update `result[3] = 5 - 3 = 2`. Add `(72, 5)`.
  - Day 6: `76 > 72`, pop `(72, 5)`, update `result[5] = 6 - 5 = 1`. Repeat for `(75, 2)`, update `result[2] = 6 - 2 = 4`. Add `(76, 6)`.
  - Day 7: `73 <= 76`, add `(73, 7)`.
- Final result: `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Edge Cases**:
1. All temperatures are the same:
   - Input: `[70, 70, 70]`.
   - Output: `[0, 0, 0]`.
2. Strictly decreasing temperatures:
   - Input: `[100, 90, 80, 70]`.
   - Output: `[0, 0, 0, 0]`.

---

### 6. Evaluate
#### **Time Complexity**:
- Both solutions iterate through the array once and each element is pushed/popped from the stack once.
- Time complexity: O(n).

#### **Space Complexity**:
1. Solution 1:
   - Extra space for `result`: O(n).
   - Stack space: O(n).
   - Total: O(n).
2. Solution 2:
   - Reuses input array, only requires stack space: O(n).

---

### Additional Notes
#### **Why This Problem is Important**:
- Frequently appears in coding interviews as it combines problem-solving and understanding of data structures like stacks.
- Reinforces the "Next Greater Element" pattern, useful for many variations.

#### **Prerequisites**:
- Knowledge of stacks.
- Familiarity with array indexing and loops.

#### **Industry Relevance**:
- Common in real-world scenarios like weather forecasting, stock price prediction, and data analysis.

#### **Follow-up Practice Problems**:
1. LeetCode 496: Next Greater Element I
2. LeetCode 503: Next Greater Element II
3. LeetCode 84: Largest Rectangle in Histogram
4. LeetCode 907: Sum of Subarray Minimums
``` 
