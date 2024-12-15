
# LeetCode 383: Ransom Note

### Problem Statement

Given two strings `ransomNote` and `magazine`, determine if it is possible to construct the `ransomNote` using the letters from `magazine`. Each letter in `magazine` can only be used once.

### Example 1:
**Input**:  
`ransomNote = "a"`  
`magazine = "b"`  
**Output**:  
`False`

### Example 2:
**Input**:  
`ransomNote = "aa"`  
`magazine = "ab"`  
**Output**:  
`False`

### Example 3:
**Input**:  
`ransomNote = "aa"`  
`magazine = "aab"`  
**Output**:  
`True`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

1. **Input**:  
   - `ransomNote` (string): The note we want to construct.  
   - `magazine` (string): The source of characters available for constructing `ransomNote`.

2. **Output**:  
   - Return `True` if `ransomNote` can be constructed using letters from `magazine`, otherwise return `False`.

3. **Constraints**:  
   - Both strings consist of lowercase English letters only.  
   - Length of `ransomNote` and `magazine` is between 1 and 10⁵.

4. **Assumptions**:  
   - Each letter in `magazine` can only be used once.  
   - Letters in `ransomNote` must exactly match or have fewer occurrences compared to `magazine`.

---

### 2. Match

1. **Pattern**:  
   - This is a **character frequency counting problem**.
   - The problem can be solved by using a hash map (dictionary) to track character counts.

2. **Algorithm/Data Structure**:  
   - Use a dictionary (`magazine_count_map`) to store character frequencies from `magazine`.
   - Iterate through `ransomNote` and check if each character is available in sufficient quantity.

---

### 3. Plan

1. **Steps**:
   1. Create a dictionary `magazine_count_map` to count character frequencies in `magazine`.
   2. Iterate through `ransomNote`:
      - If a character in `ransomNote` is not available or its count is `<= 0`, return `False`.
      - Otherwise, decrement the count of the character in `magazine_count_map`.
   3. If all characters in `ransomNote` are processed successfully, return `True`.

2. **Edge Cases**:
   - If `ransomNote` contains a character not in `magazine`, return `False`.
   - If `ransomNote` is longer than `magazine`, return `False`.

3. **Complexity Analysis**:
   - **Time Complexity**:  
     - \(O(m + n)\), where \(m\) is the length of `magazine` and \(n\) is the length of `ransomNote`.
   - **Space Complexity**:  
     - \(O(k)\), where \(k\) is the number of unique characters in `magazine`.

---

### 4. Implement

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # Step 1: Create a dictionary to count characters in magazine
        magazine_count_map = {}

        for char in magazine:
            magazine_count_map[char] = magazine_count_map.get(char, 0) + 1

        # Step 2: Check each character in ransomNote
        for char in ransomNote:
            if magazine_count_map.get(char, 0) <= 0:
                # If the character is unavailable or insufficient, return False
                return False
            
            # Decrement the character count in the map
            magazine_count_map[char] -= 1

        # Step 3: All characters matched, return True
        return True
```

---

### 5. Review

#### Dry Run Example:
**Input**:  
`ransomNote = "aab"`  
`magazine = "aabbcc"`

**Step-by-Step Execution**:
1. Initialize `magazine_count_map`: `{}`.
2. Populate the character counts from `magazine`:
   - After processing `magazine = "aabbcc"`, `magazine_count_map = {'a': 2, 'b': 2, 'c': 2}`.
3. Process `ransomNote = "aab"`:
   - For `char = 'a'`:  
     - `magazine_count_map['a'] > 0` → Decrement `magazine_count_map['a']` to 1.
   - For `char = 'a'`:  
     - `magazine_count_map['a'] > 0` → Decrement `magazine_count_map['a']` to 0.
   - For `char = 'b'`:  
     - `magazine_count_map['b'] > 0` → Decrement `magazine_count_map['b']` to 1.
4. All characters are successfully matched, return `True`.

#### Edge Case Example:
**Input**:  
`ransomNote = "xyz"`  
`magazine = "aabbcc"`

- `magazine_count_map` does not contain `x`.  
- Return `False`.

---

### 6. Evaluate

1. **Time Complexity**:
   - Constructing `magazine_count_map`: \(O(m)\), where \(m\) is the length of `magazine`.
   - Iterating through `ransomNote`: \(O(n)\), where \(n\) is the length of `ransomNote`.
   - **Total**: \(O(m + n)\).

2. **Space Complexity**:
   - Storing the dictionary `magazine_count_map`: \(O(k)\), where \(k\) is the number of unique characters in `magazine`.
   - **Total**: \(O(k)\).

---

### Additional Notes

#### Why This Problem is Important:
- It is a common pattern in string manipulation and frequency counting problems.
- Frequently used in real-world scenarios like inventory management and resource allocation.

#### Prerequisites for Practicing This Problem:
- Familiarity with string operations.
- Understanding hash maps (`dict`) and their usage.

#### Industry Relevance:
- Understanding how to efficiently use hash maps for counting is critical in many coding tasks.
- Patterns like this are building blocks for more complex problems such as substring searches.

#### Follow-up Practice Problems:
1. LeetCode 242: Valid Anagram
2. LeetCode 387: First Unique Character in a String
3. LeetCode 49: Group Anagrams
4. LeetCode 567: Permutation in String
