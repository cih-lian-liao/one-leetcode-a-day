
# LeetCode 424: Longest Repeating Character Replacement

### Problem Statement

You are given a string `s` and an integer `k`. You can choose any character from the string and change it to any other character `k` times. Return the length of the longest substring that can be achieved where all characters are the same after at most `k` changes.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Inputs:
- `s`: A string consisting of uppercase English letters (e.g., "ABAB").
- `k`: An integer representing the maximum number of character replacements allowed.

#### Outputs:
- An integer representing the maximum length of the substring with at most `k` replacements.

#### Constraints:
- 1 ≤ `s.length` ≤ 10^5
- 0 ≤ `k` ≤ `s.length`

#### Clarifications:
- Characters in the string can only be replaced, not added or removed.
- We want the maximum length of any substring where the characters are the same after `k` replacements.

---

### 2. Match

#### Problem Type:
This problem can be classified under **Sliding Window** and **HashMap** problems. The sliding window allows dynamic adjustment of the substring, while the hashmap keeps track of the frequency of characters.

#### Key Observations:
1. The window must have a majority character that we replace others to match.
2. When the number of replacements exceeds `k`, the window size needs to shrink.

---

### 3. Plan

#### Step-by-Step Plan:
1. Use two pointers, `l` (left) and `r` (right), to represent the sliding window.
2. Maintain a hashmap `count` to track character frequencies in the current window.
3. Keep track of the maximum frequency of a single character in the window using `maxf`.
4. Check if the window size minus `maxf` exceeds `k`. If yes, shrink the window by moving `l` to the right.
5. Continuously update the maximum length of the substring that satisfies the condition.
6. Return the maximum length after processing the entire string.

#### Pseudocode:
```text
Initialize count as an empty hashmap
Set l = 0, maxf = 0, and res = 0
Iterate over r from 0 to len(s) - 1:
    Update count[s[r]] and maxf
    While the window size minus maxf > k:
        Decrease count[s[l]] and increment l
    Update res as the maximum of res and the current window size
Return res
```

#### Complexity:
- Time Complexity: O(n) — Both pointers traverse the string once.
- Space Complexity: O(1) — Fixed storage for the 26 English letters.

---

### 4. Implement

#### Code:
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}  # To track the frequency of characters in the current window
        res = 0  # To store the result (maximum length of substring)
        l = 0  # Left pointer of the sliding window
        maxf = 0  # Maximum frequency of a single character in the window

        for r in range(len(s)):  # Iterate with the right pointer
            count[s[r]] = 1 + count.get(s[r], 0)  # Update the character count
            maxf = max(maxf, count[s[r]])  # Update the maximum frequency

            # If the current window is invalid (replacements exceed k), shrink it
            while (r - l + 1) - maxf > k:
                count[s[l]] -= 1
                l += 1  # Move the left pointer

            # Update the result with the valid window size
            res = max(res, r - l + 1)

        return res
```

#### Implementation Notes:
1. `count` keeps track of the frequency of characters in the window.
2. `(r - l + 1) - maxf > k` ensures we do not exceed the allowed replacements.
3. `l` adjusts dynamically to maintain a valid window.

---

### 5. Review

#### Debugging and Edge Cases:
1. **Single Character String:**
   Input: `s = "A", k = 0`  
   Output: `1`
2. **All Characters Identical:**
   Input: `s = "AAAA", k = 2`  
   Output: `4`
3. **No Replacements Allowed:**
   Input: `s = "ABAB", k = 0`  
   Output: `2`
4. **String with Diverse Characters:**
   Input: `s = "ABCDE", k = 2`  
   Output: `3`

#### Dry Run Example:
**Input:**  
`s = "AABABBA", k = 1`

**Steps:**  
- Initialize: `count = {}`, `l = 0`, `res = 0`, `maxf = 0`
- `r = 0`: `count = {'A': 1}`, `maxf = 1`, `res = 1`
- `r = 1`: `count = {'A': 2}`, `maxf = 2`, `res = 2`
- `r = 2`: `count = {'A': 2, 'B': 1}`, `maxf = 2`, `res = 3`
- `r = 3`: `count = {'A': 2, 'B': 2}`, `maxf = 2`, `res = 4`
- `r = 4`: `count = {'A': 3, 'B': 2}`, `maxf = 3`, `res = 5`
- `r = 5`: `count = {'A': 3, 'B': 3}`, `maxf = 3`, `l = 1`, `res = 4`
- `r = 6`: `count = {'A': 2, 'B': 4}`, `maxf = 4`, `res = 4`

**Output:**  
`4`

---

### 6. Evaluate

#### Complexity:
- Time Complexity: O(n)
- Space Complexity: O(1)

#### Optimizations:
This solution is optimal as it processes each character only once.

---

### Additional Notes

#### Why This Problem is Important:
- **Sliding Window Mastery:** It's an excellent example of sliding window problems.
- **Real-world Applications:** Similar logic can be applied in signal processing and data stream analysis.

#### Prerequisites for Practicing:
1. Familiarity with sliding window techniques.
2. Understanding of hashmaps for counting.

#### Industry Relevance:
- Optimizing for memory usage and efficiency is critical in real-world applications such as large-scale text analysis.

#### Follow-up Practice Problems:
1. [LeetCode 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
2. [LeetCode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
3. [LeetCode 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)
