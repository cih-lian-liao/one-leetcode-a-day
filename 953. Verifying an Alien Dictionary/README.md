
# LeetCode 953: Verifying an Alien Dictionary

### Problem Statement

Given a list of words `words` written in an alien language, where the order of the alphabet is known, verify that the words are sorted lexicographically in this alien language.

### Inputs:
- `words`: A list of strings representing words in the alien language. (1 ≤ `words.length` ≤ 100, 1 ≤ `words[i].length` ≤ 20)
- `order`: A string representing the order of the alien alphabet. (Length = 26)

### Output:
- A boolean: `True` if the words are sorted according to the alien dictionary order; otherwise, `False`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**
- **Goal**: Determine whether the given list of `words` is sorted according to the alien language's order.
- **Constraints**:
  - `words` length is limited to 100, so an `O(n * m)` approach is feasible (where `n` is the number of words and `m` is the average length of the words).
  - `order` contains all 26 letters exactly once.
- **Assumptions**:
  - The `order` string is valid and represents the unique order of the alien alphabet.
  - Words consist only of lowercase English letters.

---

### **2. Match**
This problem can be solved using:
- **Mapping**: Map each character in the alien alphabet to its rank using a dictionary for efficient lookups.
- **Comparison**: Compare adjacent words based on their lexicographical order defined by the alien dictionary.

---

### **3. Plan**
1. **Map the Alien Order**:
   - Create a dictionary `order_map` that maps each character in `order` to its index.
2. **Compare Words**:
   - For every pair of consecutive words, compare their characters one by one:
     - If characters differ, use `order_map` to compare their order.
     - If all characters are the same up to the length of the shorter word, ensure the shorter word comes first.
3. **Edge Cases**:
   - If `words` contains only one word, it is always sorted.
   - Handle cases where words have different lengths (e.g., `["app", "apple"]`).
4. **Complexity**:
   - Time Complexity: `O(n * m)` where `n` is the number of words and `m` is the average word length.
   - Space Complexity: `O(26)` for the `order_map`.

---

### **4. Implement**

```python
def isAlienSorted(words, order):
    # Step 1: Map each character in the alien order to its rank
    order_map = {char: index for index, char in enumerate(order)}
    # Helper function to compare two words
    def in_order(word1, word2):
        for c1, c2 in zip(word1, word2):
            # Compare characters using order_map
            if c1 != c2:
                return order_map[c1] < order_map[c2]
        # If words are identical up to the length of the shorter word,
        # the shorter word must come first
        return len(word1) <= len(word2)
    # Step 2: Compare every consecutive pair of words
    for i in range(len(words) - 1):
        if not in_order(words[i], words[i + 1]):
            return False
    return True
```

#### **Implementation Notes**
- `order_map`: Used for efficient rank lookup.
- `zip()`: Allows parallel traversal of two strings.
- `len(word1) <= len(word2)`: Ensures shorter words come first if one word is a prefix of another.

---

### **5. Review**

#### Example Walkthrough

**Example Input:**
```python
words = ["hello", "leetcode"]
order = "hlabcdefgijkmnopqrstuvwxyz"
```

1. **Map the Alien Order**:
   ```python
   order_map = {'h': 0, 'l': 1, 'a': 2, ...}
   ```
2. **Word Pair Comparisons**:
   - Compare "hello" and "leetcode":
     - `'h' < 'l'` → Valid order.
   - All comparisons valid, return `True`.

**Example Output:**
```python
True
```

---

### **6. Evaluate**

#### Time Complexity:
- `O(n * m)` where `n` is the number of words and `m` is the average word length.

#### Space Complexity:
- `O(26)` for the `order_map`.

---

### Additional Notes

#### Why This Problem is Important
- Teaches mapping and dictionary lookups for custom ordering.
- Practical for problems involving custom-defined sorting.

#### Prerequisites
- Familiarity with Python dictionaries.
- Understanding of string comparison logic.

#### Industry Relevance
- Useful for implementing custom sort orders in applications such as localized software and games with non-standard alphabets.

#### Follow-up Practice Problems
1. **LeetCode 242** - Valid Anagram
2. **LeetCode 937** - Reorder Data in Log Files
3. **LeetCode 791** - Custom Sort String
