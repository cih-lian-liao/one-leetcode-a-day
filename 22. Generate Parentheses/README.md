# LeetCode Problem 22: Generate Parentheses

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. **Understand**

**Problem Statement:**

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

**Inputs:**
- An integer `n` representing the number of pairs of parentheses.

**Outputs:**
- A list of strings, where each string represents a valid combination of `n` pairs of parentheses.

**Constraints:**
- 1 ≤ `n` ≤ 8.
- Each string must have exactly `2 * n` characters.
- Parentheses must be balanced, meaning every opening parenthesis `(` must have a corresponding closing parenthesis `)`.

**Assumptions:**
- Input `n` is always valid and within the given range.
- Parentheses combinations must be unique.

---

### 2. **Match**

**Pattern:**
- This problem involves **backtracking** to generate all valid combinations while exploring potential solutions.

**Key Characteristics:**
- Decision-making at each step: add either `(` or `)` based on the remaining counts.
- Constraints to ensure parentheses are balanced:
  - Add `(` if the count of `(` used so far is less than `n`.
  - Add `)` if the count of `)` used so far is less than the count of `(`.

**Data Structures:**
- Use a **list** to store results.
- Use a **string** to build each valid combination during recursion.

---

### 3. **Plan**

1. Define a helper function `backtrack` with arguments:
   - `current`: the current string being formed.
   - `open`: count of `(` used so far.
   - `close`: count of `)` used so far.

2. Base Case:
   - If the length of `current` is `2 * n`, append it to the results list.

3. Recursive Case:
   - Add `(` if `open < n`.
   - Add `)` if `close < open`.

4. Initialize the recursive function with an empty string and `open = close = 0`.

5. Return the results list.

**Pseudocode:**
```
function generateParenthesis(n):
    result = []
    function backtrack(current, open, close):
        if length of current == 2 * n:
            add current to result
            return
        if open < n:
            backtrack(current + "(", open + 1, close)
        if close < open:
            backtrack(current + ")", open, close + 1)
    backtrack("", 0, 0)
    return result
```

**Time Complexity:**
- O(4^n / sqrt(n)) for generating valid combinations (Catalan number sequence).

**Space Complexity:**
- O(n) for recursion stack.

---

### 4. **Implement**

```python
def generateParenthesis(n):
    """
    Generate all combinations of well-formed parentheses for n pairs.

    Parameters:
    n (int): Number of parentheses pairs.

    Returns:
    List[str]: List of all valid combinations.
    """
    result = []

    def backtrack(current, open_count, close_count):
        # Base Case: If the current string is of length 2 * n, it's complete
        if len(current) == 2 * n:
            result.append(current)
            return

        # Recursive Case: Add '(' if we have not used all '('
        if open_count < n:
            backtrack(current + "(", open_count + 1, close_count)

        # Add ')' if the number of ')' is less than '('
        if close_count < open_count:
            backtrack(current + ")", open_count, close_count + 1)

    # Start the recursion with an empty string
    backtrack("", 0, 0)
    return result
```

---

### 5. **Review**

**Test Case:**
Input: `n = 3`  
Output: `["((()))", "(()())", "(())()", "()(())", "()()()"]`

**Dry Run for `n = 2`:**

1. Start with `""`, `open = 0`, `close = 0`.
2. Add `(` → `"("`, `open = 1`, `close = 0`.
3. Add `(` → `"(("`, `open = 2`, `close = 0`.
4. Add `)` → `"(()"`, `open = 2`, `close = 1`.
5. Add `)` → `"(())"`, `open = 2`, `close = 2` → Append to `result`.

Repeat for other valid combinations.

---

### 6. **Evaluate**

**Time Complexity:**
- O(4^n / sqrt(n)) to generate all valid combinations.

**Space Complexity:**
- O(n) for recursion stack.

**Optimizations:**
- The constraints make this approach efficient; no further optimizations are needed.

**Alternative Approaches:**
- Dynamic Programming: Use a table to store results for smaller `n` and build up solutions.

---

### **Additional Notes**

**Why This Problem is Important:**
- Tests recursion and backtracking skills, which are essential for algorithm design.
- Understanding constraints and ensuring valid outputs are critical in real-world applications.

**Industry Relevance:**
- Useful for generating configurations, such as network packets or balanced structures in parsing tasks.

**Prerequisites:**
- Recursion.
- Backtracking.
- Understanding of constraints in combinatorial problems.

**Follow-up Practice Problems:**
1. LeetCode 17: Letter Combinations of a Phone Number.
2. LeetCode 39: Combination Sum.
3. LeetCode 77: Combinations.
