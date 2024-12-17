
# LeetCode 12: Integer to Roman

### Problem Description

Given an integer, convert it to a Roman numeral.

#### Inputs:
- `num` (integer): A positive integer in the range [1, 3999].

#### Outputs:
- A string representing the Roman numeral equivalent of the input integer.

#### Example:
| Input | Output  |
|-------|---------|
| 3     | "III"   |
| 4     | "IV"    |
| 9     | "IX"    |
| 58    | "LVIII" |
| 1994  | "MCMXCIV" |

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

#### Problem Understanding:
- Roman numerals use the following characters:
  - I (1), V (5), X (10), L (50), C (100), D (500), M (1000)
- For specific cases, subtractive notation is used:
  - IV (4), IX (9), XL (40), XC (90), CD (400), CM (900)
- We need to build the Roman numeral representation for any given integer between 1 and 3999.

---

### 2. Match

This problem relates to:
- **Greedy Algorithms**: Always subtract the largest possible Roman numeral value.
- **Pattern Matching**: Use pre-defined value-symbol pairs to determine the result efficiently.

#### Solution Selection:
1. **Solution 1 (Greedy + Dictionary):**
   - Use a map to store Roman numeral values and symbols.
   - Repeatedly subtract the largest value possible while appending the symbol.
2. **Solution 2 (Hardcoded Lists):**
   - Use pre-defined lists for thousands, hundreds, tens, and units.
   - Directly index into these lists based on `num`.

---

### 3. Plan

#### **Solution 1: Greedy Algorithm (Map-Based Approach)**

#### Steps:
1. Create a dictionary of Roman numeral values and their corresponding symbols.
2. Start with the largest value in the dictionary.
3. While the current `num` is greater than or equal to the value:
   - Append the corresponding symbol to the result.
   - Subtract the value from `num`.
4. Repeat for all values in the dictionary.
5. Return the concatenated result string.

#### Time Complexity:
- O(1): The Roman numeral values are fixed, and we iterate through a constant number of operations.

#### Space Complexity:
- O(1): Constant space for the dictionary and result string.

---

#### **Solution 2: Predefined Lists for Each Decimal Place**

#### Steps:
1. Define four lists for:
   - Thousands: ["", "M", "MM", "MMM"]
   - Hundreds: ["", "C", ..., "CM"]
   - Tens: ["", "X", ..., "XC"]
   - Units: ["", "I", ..., "IX"]
2. Extract each digit from the input number:
   - Thousands place: `num // 1000`
   - Hundreds place: `(num % 1000) // 100`
   - Tens place: `(num % 100) // 10`
   - Units place: `num % 10`
3. Use the extracted digits as indices to concatenate the corresponding Roman symbols.
4. Return the concatenated result.

#### Time Complexity:
- O(1): Constant time to extract digits and index into predefined lists.

#### Space Complexity:
- O(1): Constant space for predefined lists and result string.

---

### 4. Implement

#### **Solution 1: Greedy Algorithm (Map-Based)**

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Define Roman numeral values and symbols in a dictionary
        roman_map = {1000: "M", 900: "CM", 500: "D", 400: "CD",
                     100: "C", 90: "XC", 50: "L", 40: "XL",
                     10: "X", 9: "IX", 5: "V", 4: "IV", 1: "I"}
        
        result = ""  # Final Roman numeral string
        
        # Iterate through the dictionary
        for value, symbol in roman_map.items():
            while num >= value:  # While num is larger or equal to the value
                result += symbol  # Append the Roman numeral
                num -= value      # Subtract the value from num
        
        return result
```

#### **Solution 2: Predefined Lists**

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Predefined lists for each place value
        M = ['', 'M', 'MM', 'MMM']  # Thousands
        C = ['', 'C', 'CC', 'CCC', 'CD', 'D', 'DC', 'DCC', 'DCCC', 'CM']  # Hundreds
        X = ['', 'X', 'XX', 'XXX', 'XL', 'L', 'LX', 'LXX', 'LXXX', 'XC']  # Tens
        I = ['', 'I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX']  # Units
        
        # Combine the Roman numeral representation
        return M[num // 1000] + C[(num % 1000) // 100] + X[(num % 100) // 10] + I[num % 10]
```

---

### 5. Review

#### Dry Run with `num = 1994`:

#### Solution 1:
1. Start with `1994`.
2. Largest value is `1000 (M)`: Subtract 1000 → `994`, append "M".
3. Next largest value is `900 (CM)`: Subtract 900 → `94`, append "CM".
4. Next largest value is `90 (XC)`: Subtract 90 → `4`, append "XC".
5. Next largest value is `4 (IV)`: Subtract 4 → `0`, append "IV".
6. Result: `"MCMXCIV"`.

#### Solution 2:
1. Thousands: `1994 // 1000 = 1` → "M".
2. Hundreds: `(1994 % 1000) // 100 = 9` → "CM".
3. Tens: `(1994 % 100) // 10 = 9` → "XC".
4. Units: `1994 % 10 = 4` → "IV".
5. Result: `"MCMXCIV"`.

---

### 6. Evaluate

#### Time Complexity:
- Solution 1: O(1) (Fixed number of operations).
- Solution 2: O(1) (Fixed number of lookups).

#### Space Complexity:
- Solution 1: O(1) (Dictionary storage and result string).
- Solution 2: O(1) (Predefined lists and result string).

---

### Additional Notes

#### Why This Problem is Important:
- Understanding greedy algorithms and lookup-based solutions.
- Helps with string concatenation and modular problem-solving.

#### Prerequisites:
- Knowledge of Roman numeral rules.
- Understanding of modular arithmetic.

#### Industry Relevance:
- Frequently asked in coding interviews.
- Demonstrates an ability to optimize and use precomputed data.

#### Follow-Up Practice Problems:
1. **LeetCode 13: Roman to Integer**  
   Reverse this process to convert a Roman numeral back to an integer.
2. **LeetCode 273: Integer to English Words**  
   Convert integers into their English word representation.
