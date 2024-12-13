
# LeetCode 49: Group Anagrams

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Problem Statement**  
Given an array of strings `strs`, group the strings that are anagrams. An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

**Inputs**  
- `strs`: A list of strings of length n, where each string has a variable length k.

**Outputs**  
- A list of lists, where each inner list contains strings that are anagrams of each other.

**Constraints**  
- All strings consist of lowercase English letters.
- The length of each string will not exceed 100.
- The total number of strings will not exceed 10^4.

**Assumptions**  
- The order of groups in the output does not matter.
- Each string belongs to exactly one group.

---

### 2. Match

**Problem Type**  
- This problem relates to **hashing** and **grouping**.
- We need to identify a unique representation of anagrams to group them together.

**Relevant Data Structures**  
- Use a **dictionary (hashmap)** to group strings by a common key.
- Key can be derived by sorting the string or calculating its character frequency.

---

### 3. Plan

**Approach**  
1. Create a dictionary to store grouped anagrams.
2. For each string in the input list:
   - Count the frequency of each character using an array of size 26 (since there are 26 lowercase English letters).
   - Convert this array to a tuple, which will act as the key for the dictionary.
   - Append the string to the corresponding group in the dictionary.
3. Return the values of the dictionary as the result.

**Edge Cases**  
- An empty list `[]` should return `[]`.
- A list with only one string `["a"]` should return `[["a"]]`.
- Strings with no anagrams should each form their own group.

**Time Complexity**  
- Computing the character frequency for each string takes O(k), where k is the average length of the strings.
- Iterating through all n strings takes O(n * k).
- Total complexity: O(n * k).

**Space Complexity**  
- Space for the dictionary and character frequency arrays: O(n * k).
- Total complexity: O(n * k).

---

### 4. Implement

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs):
        # Step 1: Initialize a defaultdict to store grouped anagrams
        anagrams = defaultdict(list)
        
        # Step 2: Iterate over each string in the input list
        for s in strs:
            # Step 3: Initialize a character frequency array of size 26
            count = [0] * 26
            
            # Step 4: Populate the frequency array
            for char in s:
                count[ord(char) - ord('a')] += 1
            
            # Step 5: Convert the frequency array to a tuple (immutable)
            key = tuple(count)
            
            # Step 6: Append the string to the corresponding group
            anagrams[key].append(s)
        
        # Step 7: Return the grouped anagrams as a list of lists
        return list(anagrams.values())
```

**Implementation Notes**  
- **Defaultdict**: Automatically creates a new list for a new key, simplifying the insertion process.
- **Character Frequency**: Ensures anagrams map to the same key without sorting the strings.
- **Tuple as Key**: Tuples are immutable and hashable, making them suitable for dictionary keys.

---

### 5. Review

**Debugging and Testing**  
Test the implementation with a variety of cases to ensure correctness.

### Example 1
```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
output = [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```
**Dry Run**  
1. "eat" → frequency: `(1, 0, 0, 0, 1, 0, ..., 1)` → add to group with this key.
2. "tea" → same frequency → add to the same group.
3. "tan" → frequency: `(1, 0, ..., 1, 0, 1)` → add to a new group.
4. Continue until all strings are processed.

**Edge Case**  
```python
strs = [""]
output = [[""]]
```
Empty string maps to the frequency tuple `(0, 0, ..., 0)`.

**Complex Case**  
```python
strs = ["a"] * 10000
output = [["a"] * 10000]
```
Handles large input efficiently.

---

### 6. Evaluate

**Time Complexity**  
- O(n * k), where n is the number of strings and k is the average string length.

**Space Complexity**  
- O(n * k), for storing the dictionary and the character frequency arrays.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It tests your ability to design efficient grouping solutions.
- It emphasizes the use of hashmaps and character-based computations.

#### Prerequisites for Practicing This Problem
- Understanding hashmaps (`defaultdict`).
- Familiarity with string manipulations and character encoding (`ord`).

#### Industry Relevance
- Grouping problems are common in data processing tasks, such as categorizing log files or grouping database records.
- Efficient solutions are important for scalability in real-world applications.

#### Follow-up Practice Problems
1. LeetCode 438: Find All Anagrams in a String
2. LeetCode 242: Valid Anagram
3. LeetCode 347: Top K Frequent Elements
4. LeetCode 819: Most Common Word
