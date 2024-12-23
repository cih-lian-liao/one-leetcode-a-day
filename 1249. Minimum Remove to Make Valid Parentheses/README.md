
# LeetCode 1249: Minimum Remove to Make Valid Parentheses

## Problem Description

Given a string `s` of `'('`, `')'`, and lowercase English characters, remove the minimum number of parentheses to make the resulting string valid. Return any valid string.

### Inputs:
- `s`: A string containing `'('`, `')'`, and lowercase English letters.

### Outputs:
- A valid string with the minimum number of parentheses removed.

### Constraints:
- `1 <= s.length <= 10^5`
- `s` consists of `'('`, `')'`, and lowercase English letters.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
**What is the problem asking?**
- Remove the **minimum number of parentheses** to make the string valid.
- A valid string must have **matched parentheses**, with each `(` having a corresponding `)` in the correct order.

**Key points to clarify:**
- Letters are not removed.
- The output string order must remain the same as the input string.

**Assumptions:**
- The input always contains a valid combination of characters, even if parentheses are unmatched.

### 2. Match
**What pattern does this problem relate to?**
- Balancing parentheses is a common problem type.
- Known techniques: Use a **stack** to track unmatched parentheses.

**Best data structures to use:**
- A **stack** to store indices of unmatched parentheses.
- A **set** to store the indices of parentheses to remove for quick lookup.

---

### 3. Plan
**Step-by-step Plan:**
1. **First Pass (Left-to-Right):**
   - Use a stack to record the indices of unmatched `'('`.
   - If a `')'` is encountered:
     - If the stack is not empty, pop one `'('` from the stack (valid match).
     - Otherwise, record the index of the unmatched `')'`.

2. **Handle Remaining Unmatched `'('`:**
   - Add all indices in the stack to a set of invalid indices.

3. **Second Pass (Build Result):**
   - Traverse the string and build a result by skipping all indices in the invalid set.

**Edge Cases:**
- Empty string → return `""`.
- No parentheses (only letters) → return the original string.
- All invalid parentheses → return a string with letters only.

**Time Complexity:**
- First pass: O(n)
- Second pass: O(n)
- Overall: O(n)

**Space Complexity:**
- Stack and set space: O(n)

---

### 4. Implement

#### Solution 1
```python
def minRemoveToMakeValid(s: str) -> str:
    # Step 1: Initialize stack and set
    stack = []  # For indices of unmatched '('
    invalid_indices = set()  # Indices of invalid parentheses
    
    # Step 2: First pass (Left-to-Right)
    for i, char in enumerate(s):
        if char == '(':
            stack.append(i)  # Record index of '('
        elif char == ')':
            if stack:
                stack.pop()  # Match with an existing '('
            else:
                invalid_indices.add(i)  # Record unmatched ')'
    
    # Step 3: Add remaining unmatched '(' from stack
    invalid_indices.update(stack)
    
    # Step 4: Build result (Second pass)
    result = []
    for i, char in enumerate(s):
        if i not in invalid_indices:  # Skip invalid indices
            result.append(char)
    
    return ''.join(result)
```

---

### 5. Review
**Debugging and Testing:**
1. **Test Case 1:**
   ```python
   s = "lee(t(c)o)de)"
   print(minRemoveToMakeValid(s))  # Output: "lee(t(c)o)de"
   ```
2. **Test Case 2:**
   ```python
   s = "a)b(c)d"
   print(minRemoveToMakeValid(s))  # Output: "ab(c)d"
   ```
3. **Test Case 3 (Edge Case):**
   ```python
   s = "))(("
   print(minRemoveToMakeValid(s))  # Output: ""
   ```

---

### 6. Evaluate

**Time Complexity:**
- First traversal: O(n).
- Building result: O(n).
- Total: O(n).

**Space Complexity:**
- Stack and set for invalid indices: O(n).

---

### Additional Solution

Here is another efficient solution:

#### Solution 2
```python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        s = list(s)  # Convert to list for mutability
        stack = []   # Track indices of unmatched '('

        # First pass: Handle unmatched ')'
        for i, c in enumerate(s):
            if c == '(':
                stack.append(i)  # Record index of '('
            elif c == ')':
                if stack:
                    stack.pop()  # Match with '('
                else:
                    s[i] = ''  # Mark unmatched ')' for removal

        # Second pass: Handle unmatched '('
        while stack:
            s[stack.pop()] = ''  # Mark unmatched '(' for removal

        return ''.join(s)  # Build final string
```

**Notes on Solution 2:**
- This approach directly modifies the string (converted to a list for mutability).
- Instead of using a set, it marks invalid characters with an empty string (`''`) for efficient removal.
- Space complexity is slightly reduced as no extra set is used.

---

### Additional Notes

#### Why This Problem is Important
- Frequently used in parsing problems and compiler design.
- Tests your ability to use stacks effectively for problem-solving.

#### Prerequisites for Practicing This Problem
- Understanding stacks and their use cases.
- Familiarity with string traversal and data structures like sets.

#### Industry Relevance
- String manipulation and parsing are essential for frontend/backend developers, especially in handling user inputs or designing compilers.

#### Follow-Up Practice Problems
1. **Valid Parentheses** (LeetCode 20)
2. **Longest Valid Parentheses** (LeetCode 32)
3. **Remove All Adjacent Duplicates in String** (LeetCode 1047)
4. **Check if Parentheses String Can Be Valid** (LeetCode 2116)
