
# LeetCode Problem 290: Word Pattern

### Problem Statement

Given a `pattern` and a string `s`, determine if `s` follows the same pattern.

Each character in the `pattern` corresponds to a single word in `s`. Each character maps to a single word, and the mapping must be bijective (one-to-one and onto).

### Examples

**Example 1:**

- Input: `pattern = "abba", s = "dog cat cat dog"`
- Output: `true`

**Example 2:**

- Input: `pattern = "abba", s = "dog cat cat fish"`
- Output: `false`

**Example 3:**

- Input: `pattern = "aaaa", s = "dog cat cat dog"`
- Output: `false`

**Example 4:**

- Input: `pattern = "abba", s = "dog dog dog dog"`
- Output: `false`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

- **Inputs**:
  - `pattern`: A string of lowercase letters, e.g., `"abba"`.
  - `s`: A string of words separated by spaces, e.g., `"dog cat cat dog"`.
  
- **Output**:
  - A boolean value (`true` or `false`) indicating if `s` follows the same pattern as `pattern`.

- **Constraints**:
  - `1 <= pattern.length <= 300`
  - `1 <= s.length <= 3000`
  - `pattern` contains only lowercase English letters.
  - `s` contains only lowercase English letters and spaces.

- **Clarifications**:
  - Each character in `pattern` must map to one unique word in `s`.
  - The mapping must be consistent (bijective).

---

### 2. Match

- **Known Problem Type**:
  - This is a **mapping problem** with constraints of **bijectiveness**.

- **Relevant Data Structures**:
  - Use two dictionaries (hash maps) to ensure bidirectional mapping:
    - `char_to_word`: Maps each character in `pattern` to a word in `s`.
    - `word_to_char`: Maps each word in `s` to a character in `pattern`.

---

### 3. Plan

1. **Split the string `s` into words**:
   - Use the `split()` function to separate `s` into a list of words.

2. **Check the length of `pattern` and words**:
   - If `len(pattern) != len(words)`, return `false`.

3. **Create two dictionaries**:
   - `char_to_word`: Maps characters to words.
   - `word_to_char`: Maps words to characters.

4. **Iterate through `pattern` and `words` simultaneously**:
   - For each character `char` in `pattern` and word `word` in `words`:
     - If `char` is already in `char_to_word`:
       - Check if the mapped word matches `word`.
       - If not, return `false`.
     - If `word` is already in `word_to_char`:
       - Check if the mapped character matches `char`.
       - If not, return `false`.
     - Otherwise:
       - Add `char -> word` to `char_to_word`.
       - Add `word -> char` to `word_to_char`.

5. **Return `true` if all checks pass**.

---

### 4. Implement

```python
def wordPattern(pattern: str, s: str) -> bool:
    # Step 1: Split the string into words
    words = s.split()

    # Step 2: Check if the lengths of pattern and words match
    if len(pattern) != len(words):
        return False

    # Step 3: Create dictionaries for bidirectional mapping
    char_to_word = {}
    word_to_char = {}

    # Step 4: Iterate through pattern and words
    for char, word in zip(pattern, words):
        # Check if char is already mapped
        if char in char_to_word:
            if char_to_word[char] != word:
                return False
        else:
            char_to_word[char] = word

        # Check if word is already mapped
        if word in word_to_char:
            if word_to_char[word] != char:
                return False
        else:
            word_to_char[word] = char

    # Step 5: If all checks pass, return True
    return True
```

---

### 5. Review

#### Debugging Steps
1. Test the function with the given examples.
2. Include edge cases:
   - Empty strings (`pattern = "", s = ""`).
   - Single character and word (`pattern = "a", s = "dog"`).
   - Repeated characters with different words (`pattern = "aa", s = "dog cat"`).
   - All unique mappings (`pattern = "abcd", s = "dog cat fish bird"`).

#### Dry Run Example:
Input: `pattern = "abba", s = "dog cat cat dog"`

1. Split `s`: `words = ["dog", "cat", "cat", "dog"]`.
2. Length check: `len(pattern) == len(words)` → `4 == 4` (valid).
3. Dictionaries:
   - `char_to_word = {}`.
   - `word_to_char = {}`.
4. Iteration:
   - `char = 'a', word = 'dog'` → Add to dictionaries.
   - `char = 'b', word = 'cat'` → Add to dictionaries.
   - `char = 'b', word = 'cat'` → Mapping is consistent.
   - `char = 'a', word = 'dog'` → Mapping is consistent.
5. Return `True`.

---

### 6. Evaluate

- **Time Complexity**: O(n)
  - Splitting `s` into words: O(n).
  - Iterating through `pattern` and `words`: O(n).
  - Total: O(n), where `n` is the length of `pattern` or the number of words in `s`.

- **Space Complexity**: O(k)
  - Two dictionaries for mappings: O(k), where `k` is the number of unique characters in `pattern` and unique words in `s`.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It tests the ability to implement bidirectional mappings and manage constraints.

#### Prerequisites for Practicing This Problem
- Understanding of dictionaries (hash maps) in Python.
- Familiarity with string manipulation (`split()`).

#### Industry Relevance
- Used in scenarios requiring unique mappings, such as compilers, interpreters, and data validation systems.

#### Follow-up Practice Problems
- LeetCode 205: Isomorphic Strings
- LeetCode 291: Word Pattern II
- LeetCode 49: Group Anagrams
- LeetCode 266: Palindrome Permutation
