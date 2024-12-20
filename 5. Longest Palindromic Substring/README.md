
# LeetCode Problem 5: Longest Palindromic Substring

### Problem Statement

Given a string `s`, return the longest palindromic substring in `s`. A string is called a palindrome if it reads the same backward as forward. The substring is a contiguous sequence of characters within a string.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs:**

- A string `s` of length `n` where `1 <= n <= 1000`.

**Outputs:**

- The longest palindromic substring.

**Constraints:**

- The input string consists of only printable ASCII characters.
- If there are multiple results, return any one of them.

**Ambiguities Clarified:**

- If the input is an empty string, return an empty string (not applicable here as `n >= 1`).

---

### 2. Match

This problem is related to:
- Substring problems.
- Two-pointer approach (center expansion).

The **center expansion approach** is a suitable match for this problem because:
- It efficiently identifies palindromic substrings by expanding around each possible center.
- It avoids the space complexity of a full dynamic programming table.

We will use a **modified center expansion approach** that checks both odd- and even-length palindromes in a single loop iteration for efficiency.

---

### 3. Plan

1. Initialize variables to store the result (`result`) and its length (`result_len`).
2. Iterate through each index in the string as a potential center of a palindrome.
3. For each index:
   - Check for both odd-length palindromes (center at `i`) and even-length palindromes (center between `i` and `i+1`).
4. Expand outward from the center (`l` and `r`) while characters at both ends are equal.
5. Update the longest palindrome if the current palindrome is longer.
6. Return the longest palindrome substring.

**Time Complexity: O(n^2)**  
**Space Complexity: O(1)**

---

### 4. Implement

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        result = ""
        result_len = 0

        for i in range(len(s)):
            for odd_or_even in [0, 1]:  # Check both odd and even palindromes
                l, r = i, i + odd_or_even
                while l >= 0 and r < len(s) and s[l] == s[r]:
                    # Update result if the current palindrome is longer
                    if (r - l + 1) > result_len:
                        result = s[l:r + 1]
                        result_len = r - l + 1
                    l -= 1
                    r += 1

        return result
```

---

### 5. Review

**Dry Run Example:**

Input: `s = "babad"`

1. `i = 0`, odd-length check (`l = 0, r = 0`):
   - Expands to `"b"` (valid).
   - Updates result to `"b"`, result length = 1.

2. `i = 0`, even-length check (`l = 0, r = 1`):
   - No palindrome.

3. `i = 1`, odd-length check (`l = 1, r = 1`):
   - Expands to `"a"`, then `"bab"` (valid).
   - Updates result to `"bab"`, result length = 3.

4. `i = 1`, even-length check (`l = 1, r = 2`):
   - No palindrome.

5. `i = 2`, odd-length check (`l = 2, r = 2`):
   - Expands to `"aba"` (valid).
   - Result remains `"bab"` since `"aba"` is of the same length.

6. `i = 2`, even-length check (`l = 2, r = 3`):
   - No palindrome.

Continue similarly for all indices.

Output: `"bab"` or `"aba"`.

**Edge Cases Tested:**

1. Single character: `"a"` → `"a"`.
2. Entire string is a palindrome: `"aaaa"` → `"aaaa"`.
3. No palindromic substring longer than one: `"abc"` → `"a"`, `"b"`, or `"c"`.

---

### 6. Evaluate

**Time Complexity: O(n^2)**  
- For each character, we potentially expand up to the length of the string.

**Space Complexity: O(1)**  
- We only use a few variables to store indices and results.

---

### Additional Notes

#### Why This Problem Is Important

- Teaches efficient handling of string manipulations.
- Demonstrates the use of two-pointer techniques in substring problems.
- Serves as a stepping stone to understand more advanced substring problems.

#### Prerequisites

- Familiarity with two-pointer techniques.
- Basic understanding of substrings and how to manipulate them.

#### Industry Relevance

- String manipulation is a common task in backend systems, data validation, and text processing pipelines.
- Efficiently solving substring problems is critical for search engines and bioinformatics.

#### Follow-Up Practice Problems

1. **LeetCode 647: Palindromic Substrings**
   - Count all palindromic substrings in a string.
2. **LeetCode 516: Longest Palindromic Subsequence**
   - Find the longest palindromic subsequence instead of a substring.
3. **LeetCode 125: Valid Palindrome**
   - Check if a string is a palindrome, ignoring spaces and punctuation.
