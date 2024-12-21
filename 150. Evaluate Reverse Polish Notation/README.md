# LeetCode 150: Evaluate Reverse Polish Notation

### Problem Statement

Evaluate the value of an arithmetic expression in Reverse Polish Notation (RPN).  
Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression. Division should truncate towards zero.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs**:
- `tokens`: A list of strings, where each string is either an operand (integer) or an operator (`+`, `-`, `*`, `/`).

**Outputs**:
- An integer representing the evaluated result of the RPN expression.

**Constraints**:
- The expression is always valid and results in an integer.
- Division truncates towards zero.
- Operands and results fit in a 32-bit signed integer.

**Example**:

- Input: `["2", "1", "+", "3", "*"]`
  - Evaluate as: `((2 + 1) * 3) = 9`
  - Output: `9`

---

### 2. Match

- **Pattern**: Stack-based evaluation of expressions. The stack is used to handle intermediate results.
- **Data Structures**:
  - Use a stack to store operands.
  - Operators are applied to the top two operands in the stack.

---

### 3. Plan

**Step-by-Step Plan**:
1. Initialize an empty stack.
2. Iterate through each token in the input list:
   - If the token is an operand, convert it to an integer and push it onto the stack.
   - If the token is an operator:
     - Pop the top two elements from the stack.
     - Perform the operation:
       - `+`: Add the two numbers.
       - `-`: Subtract the second popped number from the first.
       - `*`: Multiply the two numbers.
       - `/`: Divide the first popped number by the second and truncate towards zero.
     - Push the result back onto the stack.
3. After processing all tokens, the stack contains one element, which is the result.

---

### 4. Implement

```python
from typing import List

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []

        for token in tokens:
            if token == "+":  # Addition
                stack.append(stack.pop() + stack.pop())
            elif token == "-":  # Subtraction
                next_token, prev_token = stack.pop(), stack.pop()
                stack.append(prev_token - next_token)
            elif token == "*":  # Multiplication
                stack.append(stack.pop() * stack.pop())
            elif token == "/":  # Division
                next_token, prev_token = stack.pop(), stack.pop()
                stack.append(int(prev_token / next_token))  # Truncate result
            else:  # Operand
                stack.append(int(token))

        return stack[0]
```

---

#### Notes for Implementation

1. **Initialization**:
   - `stack = []`: Create an empty stack to store intermediate results.

2. **Iterating Tokens**:
   - Loop through `tokens` to process each element.
   - Use `int(token)` to handle operands.

3. **Operations**:
   - For `+`, `-`, `*`, `/`, pop the last two elements in the stack.
   - Perform the operation:
     - For subtraction and division, note the order of operands (first popped is the second operand).

4. **Truncation in Division**:
   - Use `int(prev_token / next_token)` to truncate the result towards zero.

---

### 5. Review

#### Example: Input = `["10", "6", "9", "3", "/", "*", "+", "11", "-"]`

1. **Initial Stack**: `[]`
2. Process `10`: Push `10`. Stack = `[10]`
3. Process `6`: Push `6`. Stack = `[10, 6]`
4. Process `9`: Push `9`. Stack = `[10, 6, 9]`
5. Process `3`: Push `3`. Stack = `[10, 6, 9, 3]`
6. Process `/`: Pop `9, 3`, divide `9 / 3 = 3`, push `3`. Stack = `[10, 6, 3]`
7. Process `*`: Pop `6, 3`, multiply `6 * 3 = 18`, push `18`. Stack = `[10, 18]`
8. Process `+`: Pop `10, 18`, add `10 + 18 = 28`, push `28`. Stack = `[28]`
9. Process `11`: Push `11`. Stack = `[28, 11]`
10. Process `-`: Pop `28, 11`, subtract `28 - 11 = 17`, push `17`. Stack = `[17]`

**Final Stack**: `[17]`  
**Output**: `17`

---

### 6. Evaluate

**Time Complexity**:
- O(n): Each token is processed exactly once.

**Space Complexity**:
- O(n): Stack can grow up to the size of the input in the worst case.

---

### Additional Notes

**Why This Problem is Important**:
- Teaches how to use stacks for expression evaluation.
- Reinforces understanding of operator precedence and order.

**Prerequisites for Practicing**:
- Understanding of stacks.
- Familiarity with basic arithmetic operations.

**Industry Relevance**:
- Expression evaluation algorithms are foundational in compilers and interpreters.

**Follow-up Practice Problems**:
1. [LeetCode 394: Decode String](https://leetcode.com/problems/decode-string/)
2. [LeetCode 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
3. [LeetCode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
