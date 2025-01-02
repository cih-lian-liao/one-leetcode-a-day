# LeetCode 422: Valid Word Square

### Problem Statement
Given an array of strings `words`, determine whether it forms a valid word square.

A sequence of strings forms a valid word square if the `k-th` row and column of the grid formed by the strings are identical, for all valid `k`.

#### Example 1
```plaintext
Input: words = [
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]
Output: true
```

#### Example 2
```plaintext
Input: words = [
  "abcd",
  "bnrt",
  "crm",
  "dt"
]
Output: false
```

#### Example 3
```plaintext
Input: words = [
  "ball",
  "area",
  "read",
  "lady"
]
Output: false
```

#### Constraints
- `1 <= words.length <= 500`
- `1 <= words[i].length <= 500`
- Each string consists only of lowercase English letters.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand
Fully grasp the problem requirements:
- Inputs:
  - `words`: A list of strings.
- Output:
  - A boolean indicating whether the grid forms a valid word square.
- Constraints:
  - Ensure characters in the `k-th` row match the `k-th` column for all valid `k`.
  
Key Observations:
- Each row's length must not exceed the total number of rows.
- Out-of-bound indices in either rows or columns immediately disqualify the square.

---

### 2. Match
This problem relates to matrix traversal, specifically row-column symmetry checks. The primary pattern involves iterating over rows and comparing corresponding characters in columns.

---

### 3. Plan
**Step-by-Step Plan:**
1. Iterate through the `words` list.
2. For each row index `i`, iterate through the characters in `words[i]`.
3. Compare the character at `words[i][j]` with `words[j][i]`.
4. Return `false` if any mismatch is found.
5. Return `true` if all comparisons pass.

**Edge Cases:**
- Single string in `words` (trivial valid case).
- Strings of varying lengths.
- Empty strings or empty input (not possible due to constraints).

**Time Complexity:**
- **O(n * m)** where `n` is the number of rows and `m` is the maximum string length.

**Space Complexity:**
- **O(1)** as no additional data structures are used.

---

### 4. Implement
Here is the implementation:

```python
def validWordSquare(words):
    n = len(words)
    for i in range(n):
        for j in range(len(words[i])):
            # Check bounds and character equality
            if j >= n or i >= len(words[j]) or words[i][j] != words[j][i]:
                return False
    return True
```

**Implementation Notes:**
1. `n` stores the number of rows.
2. Outer loop iterates over rows (`i`), inner loop iterates over characters in the row (`j`).
3. Ensure indices are within bounds and characters match.
4. Return `false` on the first mismatch.

---

### 5. Review
**Debugging Steps:**
- Test simple cases like single-row input.
- Test edge cases like rows of varying lengths.

**Complex Test Case:**
Input:
```python
words = [
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]
```
Execution:
1. `i = 0`: Compare `"abcd"` with column `"abcd"`. Match.
2. `i = 1`: Compare `"bnrt"` with column `"bnrt"`. Match.
3. `i = 2`: Compare `"crmy"` with column `"crmy"`. Match.
4. `i = 3`: Compare `"dtye"` with column `"dtye"`. Match.
Output: `true`.

---

### 6. Evaluate
**Time Complexity:** O(n * m)
- Outer loop runs for `n` rows.
- Inner loop runs for `m` characters in each row.

**Space Complexity:** O(1)
- No additional data structures.

**Optimizations:**
This implementation is already optimal given the problem constraints.

---

### Additional Notes

#### Why This Problem is Important
- Tests understanding of matrix traversal.
- Highlights edge case handling and out-of-bound checks.

#### Prerequisites for Practicing This Problem
- Basic understanding of 2D arrays.
- Familiarity with nested loops and indexing.

#### Industry Relevance
- Row-column symmetry checks are common in grid-based problems, especially in game development and simulations.

#### Follow-up Practice Problems
1. LeetCode 36: Valid Sudoku
2. LeetCode 48: Rotate Image
3. LeetCode 54: Spiral Matrix

---

#### Example Test Cases

**Example 1:**
```python
words = [
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]
assert validWordSquare(words) == True
```

**Example 2:**
```python
words = [
  "abcd",
  "bnrt",
  "crm",
  "dt"
]
assert validWordSquare(words) == False
```

**Example 3:**
```python
words = [
  "ball",
  "area",
  "read",
  "lady"
]
assert validWordSquare(words) == False
