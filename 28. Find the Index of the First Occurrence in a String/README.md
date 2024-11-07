
# Problem 28. Find the Index of the First Occurrence in a String

### Problem Description
Given two strings `haystack` and `needle`, we need to return the index of the first occurrence of `needle` in `haystack`. If `needle` does not exist in `haystack`, we return `-1`.

For example:
- `haystack = "hello"`, `needle = "ll"` -> Output: `2` (since "ll" starts at index 2 in "hello").
- `haystack = "hello"`, `needle = "world"` -> Output: `-1` (since "world" is not in "hello").

---

### Approach: Brute Force Solution
1. **Goal**: We want to find the first position in `haystack` where `needle` starts. If there’s no such position, we return `-1`.

2. **Explanation**:
   - We will slide `needle` over `haystack` one character at a time, checking each substring of `haystack` to see if it matches `needle`.
   - To accomplish this, we loop from the beginning of `haystack` up to a point where `needle` could still fit.

3. **Detailed Steps**:
   - **Step 1**: Define a loop that iterates from index `i = 0` up to `len(haystack) - len(needle)`. This ensures we don’t go out of bounds since `needle` needs space to fit within `haystack`.
   - **Step 2**: For each index `i`, extract a substring of `haystack` that starts at `i` and has a length equal to `needle`.
     - This can be done using `haystack[i:i + len(needle)]`.
   - **Step 3**: Compare this substring to `needle`.
     - If they are equal, it means `needle` starts at index `i` in `haystack`, so we return `i`.
   - **Step 4**: If the loop finishes without finding `needle`, return `-1`.

4. **Code**:
   ```python
   def strStr(haystack: str, needle: str) -> int:
       for i in range(len(haystack) - len(needle) + 1):
           if haystack[i:i + len(needle)] == needle:
               return i
       return -1
   ```

5. **Complexity Analysis**:
   - **Time Complexity**: O(n * m), where `n` is the length of `haystack` and `m` is the length of `needle`.
   - **Space Complexity**: O(1), as this solution uses a fixed amount of space.

---

### Approach: Optimized Solution Using Python’s `find()` Method
1. **Explanation**: Python provides a built-in method called `find()` for strings, which efficiently finds the first occurrence of a substring.
2. **Steps**:
   - Call `haystack.find(needle)`.
     - If `needle` is found, it returns the index of the first occurrence.
     - If `needle` is not found, it returns `-1`.
   - Using this is a quick way to solve the problem with a single line of code, leveraging Python’s internal optimizations.
   
3. **Code**:
   ```python
   def strStr(haystack: str, needle: str) -> int:
       return haystack.find(needle)
   ```

4. **Complexity**:
   - **Time Complexity**: O(n), as `find()` is optimized internally.
   - **Space Complexity**: O(1).

---

### Advanced Approach: Knuth-Morris-Pratt (KMP) Algorithm (for Large Inputs)
1. **Explanation**: KMP is an efficient string matching algorithm that avoids redundant comparisons.
   - Instead of rechecking characters that have already matched, KMP uses a **partial match table** (or prefix table) to track possible shifts in the pattern.

2. **Steps**:
   - **Step 1**: Build the **prefix table** for `needle`.
     - The prefix table stores the lengths of the longest proper prefix which is also a suffix for each position in `needle`.
   - **Step 2**: Use this table to skip unnecessary comparisons during the search in `haystack`.
     - If there’s a mismatch, the table tells us where to continue matching without rechecking matched characters.
   
3. **Detailed Steps**:
   - **Building the Prefix Table**:
     - Initialize the prefix table with zeros: `prefix_table = [0] * len(needle)`.
     - Use two pointers: `i` (current position in `needle`) and `j` (length of the previous longest prefix suffix).
     - For each position `i` in `needle`:
       - If `needle[i] == needle[j]`, increment `j` and set `prefix_table[i] = j`.
       - If they do not match and `j > 0`, set `j = prefix_table[j - 1]` (move to a shorter prefix).
       - Otherwise, move to the next character.
   - **Using the Prefix Table for Matching**:
     - Initialize a pointer `j` for `needle`.
     - For each character `haystack[i]`:
       - If `haystack[i] == needle[j]`, increment `j`.
       - If `j` equals the length of `needle`, return the starting index, calculated as `i - j + 1`.
       - If there’s a mismatch, use the prefix table to reset `j`.
   - If no match is found, return `-1`.

4. **Code**:
   ```python
   def strStr(haystack: str, needle: str) -> int:
       if not needle:
           return 0
       
       # Build the KMP table for the pattern (needle)
       prefix_table = [0] * len(needle)
       j = 0  # Length of the previous longest prefix suffix
       
       for i in range(1, len(needle)):
           while j > 0 and needle[i] != needle[j]:
               j = prefix_table[j - 1]
           if needle[i] == needle[j]:
               j += 1
               prefix_table[i] = j

       # KMP search
       j = 0  # index for needle
       for i in range(len(haystack)):
           while j > 0 and haystack[i] != needle[j]:
               j = prefix_table[j - 1]
           if haystack[i] == needle[j]:
               j += 1
           if j == len(needle):
               return i - j + 1
       return -1
   ```

5. **Complexity**:
   - **Time Complexity**: O(n + m), where `n` is the length of `haystack` and `m` is the length of `needle`.
   - **Space Complexity**: O(m), due to the prefix table.

---

### Interview Tips
1. **Start Simple**: In an interview, it’s good to start with a straightforward solution like the brute-force approach, showing your understanding of the problem and willingness to optimize.
2. **Optimize**: If time allows, mention the Python `find()` method for efficiency.
3. **Discuss Advanced Options**: If asked about large input cases or efficiency, explain the KMP algorithm as an optimal solution for complex patterns.
4. **Code Clarity**: Practice explaining each step in the code and testing with examples like `haystack = "hello", needle = "ll"` to make sure the logic is clear.