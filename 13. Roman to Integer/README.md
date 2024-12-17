# LeetCode 13: Roman to Integer

### Problem Description
Given a string `s` representing a Roman numeral, convert it to an integer.

### **Example 1**
Input: `s = "III"`  
Output: `3`

### **Example 2**
Input: `s = "IV"`  
Output: `4`

### **Example 3**
Input: `s = "MCMXCIV"`  
Output: `1994`

### Constraints:
- `1 <= s.length <= 15`
- `s` contains valid Roman numerals only.
- Roman numerals: I, V, X, L, C, D, M.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
Fully grasp the problem and clarify:
- **Input**: A string `s` (Roman numeral).
- **Output**: An integer converted from the Roman numeral.
- **Constraints**: `s` is valid, so no invalid characters or formatting.

#### Roman Numeral Rules:
| Symbol | Value |
|--------|-------|
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

- Roman numerals follow two rules:
  1. **Addition**: Numerals are summed if the current numeral is greater than or equal to the next one.
     - Example: `VI` = 5 + 1 = 6
  2. **Subtraction**: Numerals subtract if the current numeral is smaller than the next one.
     - Example: `IV` = 5 - 1 = 4

---

### 2. Match
Relate the problem to:
- **Patterns**: Sequential traversal with conditional logic.
- **Data Structures**: Use a dictionary to store Roman numeral values for O(1) lookups.
- **Algorithm**: Iterate through the string and decide to either subtract or add the current value.

---

### 3. Plan
**Step-by-step approach**:
1. Create a dictionary `roman_map` to store Roman numeral values.
2. Initialize `result = 0` to hold the final integer value.
3. Iterate through the string `s`:
   - Compare the current numeral with the next numeral:
     - If the current numeral is smaller than the next one, subtract its value.
     - Otherwise, add its value.
4. Return the final result.

**Pseudocode**:
```plaintext
Initialize roman_map = {I: 1, V: 5, X: 10, L: 50, C: 100, D: 500, M: 1000}
Initialize result = 0

For i in range(0, len(s)):
    If i < len(s) - 1 and roman_map[s[i]] < roman_map[s[i+1]]:
        Subtract current value from result
    Else:
        Add current value to result

Return result
```

---

### 4. Implement
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        # Step 1: Define the Roman numeral map
        roman_map = {
            'I': 1, 'V': 5, 'X': 10, 'L': 50,
            'C': 100, 'D': 500, 'M': 1000
        }
        
        # Step 2: Initialize result variable
        result = 0
        n = len(s)
        
        # Step 3: Traverse the string
        for i in range(n):
            # Step 4: Check subtraction condition
            if i < n - 1 and roman_map[s[i]] < roman_map[s[i + 1]]:
                result -= roman_map[s[i]]
            else:
                result += roman_map[s[i]]
        
        # Step 5: Return the final result
        return result
```

#### **Code Notes**:
1. `roman_map`: A dictionary for O(1) value lookup.
2. `if i < n - 1`: Ensures `i+1` does not go out of bounds.
3. Subtraction logic: `roman_map[s[i]] < roman_map[s[i+1]]` ensures the subtraction rule.

---

### 5. Review
#### **Test Cases**:
1. **Simple Case**:
   - Input: `s = "III"`
   - Output: `3`
   - Explanation: `1 + 1 + 1 = 3`

2. **Subtraction Case**:
   - Input: `s = "IV"`
   - Output: `4`
   - Explanation: `5 - 1 = 4`

3. **Complex Case**:
   - Input: `s = "MCMXCIV"`
   - Output: `1994`
   - Explanation: `1000 + (1000 - 100) + (100 - 10) + (5 - 1)`

4. **Edge Case**:
   - Input: `s = "M"`
   - Output: `1000`

#### **Dry Run**
Input: `s = "MCMXCIV"`  
| i   | s[i] | Next Value Check    | Action            | Result |
|------|------|---------------------|-------------------|--------|
| 0    | M    | C < M (False)       | Add 1000          | 1000   |
| 1    | C    | M > C (True)        | Subtract 100      | 900    |
| 2    | M    | X < M (False)       | Add 1000          | 1900   |
| 3    | X    | C > X (True)        | Subtract 10       | 1890   |
| 4    | C    | I < C (False)       | Add 100           | 1990   |
| 5    | I    | V > I (True)        | Subtract 1        | 1989   |
| 6    | V    | End                 | Add 5             | 1994   |

Output: `1994`

---

### 6. Evaluate
- **Time Complexity**: O(n)
  - Traverse the string `s` once.
- **Space Complexity**: O(1)
  - The dictionary `roman_map` takes constant space.

#### **Why This Problem is Important**:
1. It tests understanding of conditional logic and edge cases.
2. It demonstrates efficient use of data structures (e.g., dictionary).
3. It is a common interview question for testing problem-solving skills.

#### **Prerequisites**:
1. String traversal techniques.
2. Basic understanding of hash maps/dictionaries.

#### **Industry Relevance**:
1. Fundamental for coding interviews.
2. Reinforces logical problem-solving with real-world applications.

#### **Follow-up Practice Problems**:
1. LeetCode 12 - Integer to Roman
2. LeetCode 14 - Longest Common Prefix
3. LeetCode 171 - Excel Sheet Column Number
4. LeetCode 168 - Excel Sheet Column Title
