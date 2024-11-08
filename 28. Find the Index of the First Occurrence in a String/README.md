
# Problem 28. Find the Index of the First Occurrence in a String

### Problem Description
Given two strings `haystack` and `needle`, we need to return the index of the first occurrence of `needle` in `haystack`. If `needle` does not exist in `haystack`, we return `-1`.

For example:
- `haystack = "hello"`, `needle = "ll"` -> Output: `2` (since `"ll"` starts at index 2 in `"hello"`).
- `haystack = "hello"`, `needle = "world"` -> Output: `-1` (since `"world"` is not in `"hello"`).

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


### Here’s how to approach KMP in a coding interview:

1. Start with a Basic Solution: Begin by writing the brute-force solution to demonstrate your understanding of the problem.  
2. Mention KMP and Explain Its Optimization Points: If time permits and the interviewer shows interest, introduce the KMP algorithm and its prefix table concept, highlighting how it optimizes the matching process. You don’t necessarily need to code it fully.  
3. Avoid Getting Stuck in Complex Code: KMP can be complex to implement, and there might not be enough time in the interview to finish it. Often, explaining the principle and optimization points is sufficient to show your knowledge.


> "To solve the problem more efficiently, I'd like to introduce the Knuth-Morris-Pratt (KMP) algorithm. The KMP algorithm is specifically designed to avoid unnecessary comparisons by utilizing a **prefix table**. This table helps us skip parts of the pattern (`needle`) that we know have already been matched, reducing the time complexity from O(n * m) to O(n + m), where `n` is the length of `haystack` and `m` is the length of `needle`."

### Explain the Prefix Table

> "The prefix table, also known as the partial match table, records the lengths of the longest prefix that is also a suffix for each position in the pattern. This way, if we encounter a mismatch while matching `needle` in `haystack`, we can use this table to jump to the appropriate position in `needle` without rechecking characters we've already compared."

### How to Build the Prefix Table

> "To build the prefix table:
> - We initialize an array `prefix_table` to hold the longest prefix-suffix lengths for each position in `needle`.
> - We use two pointers, `i` and `j`. The `i` pointer scans through `needle`, and `j` keeps track of the length of the longest matching prefix that ends at `i - 1`.
> - If `needle[i] == needle[j]`, it means we can extend our prefix match, so we increment `j` and set `prefix_table[i]` to `j`.
> - If they don't match, we set `j` to `prefix_table[j - 1]` to try a shorter prefix that we know is a potential match."

### Example of Prefix Table Construction

> "For example, if our pattern `needle` is `'ABABC'`, the prefix table would be `[0, 0, 1, 2, 0]`. Here, `1` at index `2` means the substring `'ABA'` has a matching prefix-suffix length of `1`, and `2` at index `3` means `'ABAB'` has a matching prefix-suffix length of `2`. This prefix table is what allows us to skip characters when there’s a mismatch."

### Using the Prefix Table for Efficient Matching

> "Once we have the prefix table, we can use it during the matching process:
> - We compare characters in `haystack` and `needle` using two pointers, `i` and `j`.
> - If there’s a match (`haystack[i] == needle[j]`), we increment both pointers.
> - If there’s a mismatch, we use the prefix table to shift `j` back to `prefix_table[j - 1]` and continue comparing without resetting `i`.
> - If `j` reaches the length of `needle`, it means we found the pattern, so we return `i - j + 1` as the starting index."

### Summary of KMP’s Efficiency

> "By leveraging the prefix table, KMP avoids redundant comparisons and brings the time complexity down to O(n + m), making it ideal for large-scale text matching. This optimization is especially useful when the pattern has repetitive characters or when working with long strings."
