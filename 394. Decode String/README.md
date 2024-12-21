
# LeetCode 394: Decode String

### Problem Statement

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is repeated exactly `k` times. You may assume that the input string is always valid and does not contain extra spaces.

### Examples

**Example 1**:  
Input: `s = "3[a]2[bc]"`  
Output: `"aaabcbc"`

**Example 2**:  
Input: `s = "3[a2[c]]"`  
Output: `"accaccacc"`

**Example 3**:  
Input: `s = "2[abc]3[cd]ef"`  
Output: `"abcabccdcdcdef"`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Inputs**:  
- A string `s` containing digits, letters, and square brackets in the format described above.  

**Outputs**:  
- A decoded string following the described encoding rule.

**Constraints**:  
- 1 <= s.length <= 30  
- The string `s` consists of lowercase English letters, digits, and square brackets `'[]'`.  
- All integers are in the range [1, 300].  
- It is guaranteed that `s` is a valid encoding.

**Clarifications**:  
- Nested encoding is allowed, e.g., `"3[a2[c]]"`.  
- Ensure correct handling of multi-digit integers like `"12[abc]"`.

---

### 2. Match

This problem is related to **stack-based decoding**. The following key insights apply:
- Stacks help in managing nested operations where the most recent "open" task (e.g., `[` and its content) needs resolution first.
- Common patterns:
  - Push operations to save intermediate states.
  - Pop operations to resolve nested strings when encountering `]`.

---

### 3. Plan

#### High-Level Steps
1. Initialize an empty stack.
2. Traverse the string `s` character by character:
   - If the character is not `]`, push it onto the stack.
   - If the character is `]`:
     - Pop elements to form the substring until encountering `[` (this is the encoded string).
     - Pop the number before `[` to determine the repetition count.
     - Decode the substring by repeating it `k` times and push the result back to the stack.
3. Finally, join all characters in the stack to form the final decoded string.

#### Edge Cases
- Nested encoding: `"3[a2[c]]"`.
- Multiple sequences: `"2[abc]3[cd]ef"`.
- Multi-digit integers: `"10[a]"`.

#### Time Complexity
- O(n): Each character is pushed and popped from the stack once.

#### Space Complexity
- O(n): The stack stores up to all characters in the string.

---

### 4. Implement

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []  # Initialize the stack to store characters and decoded segments.

        for i in range(len(s)):
            if s[i] != "]":  # If the current character is not a closing bracket:
                stack.append(s[i])  # Push it onto the stack.
            else:  # When encountering a closing bracket:
                substr = ""  # Initialize the substring.
                # Build the substring by popping until encountering an open bracket.
                while stack and stack[-1] != "[":
                    substr = stack.pop() + substr
                stack.pop()  # Pop the open bracket "[".

                k = ""  # Initialize the multiplier (number).
                # Build the multiplier by popping digits.
                while stack and stack[-1].isdigit():
                    k = stack.pop() + k
                
                # Decode the substring by repeating it k times and push back.
                stack.append(int(k) * substr)

        # Join all parts of the stack to form the final decoded string.
        return "".join(stack)
```

#### Implementation Notes
1. Use a stack to handle nested structures efficiently.
2. Build substrings and numbers from the stack in reverse order.
3. Avoid modifying the input string directly; work on stack operations for simplicity.

---

### 5. Review

#### Debugging
Use the following test cases to debug and verify correctness:

1. Input: `"3[a]2[bc]"`  
   Dry Run:
   - Traverse `3[a]` → Decode to `"aaa"`.
   - Traverse `2[bc]` → Decode to `"bcbc"`.
   - Result: `"aaabcbc"`.

2. Input: `"3[a2[c]]"`  
   Dry Run:
   - Decode `2[c]` → `"cc"`.
   - Decode `3[a2[c]]` → `"accaccacc"`.
   - Result: `"accaccacc"`.

3. Input: `"10[a]"`  
   Dry Run:
   - Decode `10[a]` → `"aaaaaaaaaa"`.
   - Result: `"aaaaaaaaaa"`.

#### Edge Case
Input: `"2[abc]3[cd]ef"`  
- Decode `2[abc]` → `"abcabc"`.
- Decode `3[cd]` → `"cdcdcd"`.
- Append `ef`.
- Result: `"abcabccdcdcdef"`.

---

### 6. Evaluate

#### Time Complexity
O(n): Each character in the string is processed exactly once (pushed and popped from the stack).

#### Space Complexity
O(n): The stack can grow to the size of the input string in the worst case.

---

### Additional Notes

#### Why This Problem is Important
1. It demonstrates the application of **stack data structures** for nested problems.
2. It tests the ability to break down complex problems into manageable parts.
3. It’s a common pattern in coding interviews to decode or evaluate nested structures.

#### Prerequisites for Practicing This Problem
1. Understanding of stack data structure.
2. Basic knowledge of string manipulation in Python.

#### Industry Relevance
- Parsing expressions or commands in compilers.
- Decoding hierarchical data like JSON or XML.
- String manipulation problems in general.

#### Follow-up Practice Problems
1. [LeetCode 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
2. [LeetCode 385: Mini Parser](https://leetcode.com/problems/mini-parser/)
3. [LeetCode 726: Number of Atoms](https://leetcode.com/problems/number-of-atoms/)
