
# LeetCode Problem 205: Isomorphic Strings

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

#### Problem Description
Two strings `s` and `t` are **isomorphic** if the characters in `s` can be replaced to get `t`.  
Each character in `s` must map to exactly one character in `t`, and no two characters in `s` can map to the same character in `t`.

#### Inputs
- `s` (string): the source string.
- `t` (string): the target string.

#### Outputs
- A boolean: `True` if the strings are isomorphic, `False` otherwise.

#### Constraints
1. `s.length == t.length`
2. Strings consist of any valid ASCII characters.

---

### 2. Match

#### Problem Pattern
- **Mapping problem**: We need to map characters from one string to another.
- **Bijective mapping**: Each character in `s` maps to one character in `t`, and vice versa.

#### Suitable Data Structures
- Dictionaries (hashmaps): To maintain the mapping between characters of `s` and `t`.

---

### 3. Plan

1. Initialize two dictionaries:
   - `s_to_t_map`: Maps characters from `s` to `t`.
   - `t_to_s_map`: Maps characters from `t` to `s`.

2. Iterate over characters in `s` and `t` using `zip()`.

3. For each pair `(char_s, char_t)`:
   - Check if `char_s` is already in `s_to_t_map`:
     - If it maps to a different `char_t`, return `False`.
   - Otherwise, add the mapping `s_to_t_map[char_s] = char_t`.
   - Similarly, check if `char_t` is already in `t_to_s_map`:
     - If it maps to a different `char_s`, return `False`.
   - Otherwise, add the mapping `t_to_s_map[char_t] = char_s`.

4. If the loop completes without contradictions, return `True`.

#### Time Complexity
- O(n), where `n` is the length of the strings `s` and `t`.

#### Space Complexity
- O(n), for the two dictionaries used to store mappings.

---

### 4. Implement

```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        # Step 1: Initialize dictionaries for bidirectional mapping
        s_to_t_map = {}
        t_to_s_map = {}
        
        # Step 2: Iterate over characters of s and t using zip()
        for char_s, char_t in zip(s, t):
            # Step 3: Check s -> t mapping
            if char_s in s_to_t_map and s_to_t_map[char_s] != char_t:
                return False
            # Add mapping to s -> t dictionary
            s_to_t_map[char_s] = char_t
            
            # Step 4: Check t -> s mapping
            if char_t in t_to_s_map and t_to_s_map[char_t] != char_s:
                return False
            # Add mapping to t -> s dictionary
            t_to_s_map[char_t] = char_s
        
        # Step 5: Return True if no conflicts are found
        return True
```

---

### 5. Review

#### Test Cases
1. **Basic Cases**:
   - Input: `s = "egg"`, `t = "add"`
     - Expected Output: `True`
   - Input: `s = "foo"`, `t = "bar"`
     - Expected Output: `False`
2. **Edge Cases**:
   - Input: `s = "ab"`, `t = "aa"`
     - Expected Output: `False`
   - Input: `s = "paper"`, `t = "title"`
     - Expected Output: `True`

#### Dry Run (Complex Case)
- Input: `s = "paper"`, `t = "title"`

| Step | char_s | char_t | `s_to_t_map`          | `t_to_s_map`          | Output |
|------|--------|--------|-----------------------|-----------------------|--------|
| 1    | p      | t      | {p: t}               | {t: p}               | -      |
| 2    | a      | i      | {p: t, a: i}         | {t: p, i: a}         | -      |
| 3    | p      | t      | {p: t, a: i}         | {t: p, i: a}         | -      |
| 4    | e      | l      | {p: t, a: i, e: l}   | {t: p, i: a, l: e}   | -      |
| 5    | r      | e      | {p: t, a: i, e: l, r: e} | {t: p, i: a, l: e, e: r} | -      |

Final Output: `True`

---

### 6. Evaluate

#### Time Complexity
- O(n): We iterate over the strings once, where `n` is the length of the strings.

#### Space Complexity
- O(n): We use two dictionaries, each with at most `n` key-value pairs.

---

### Additional Notes

#### Why This Problem is Important
- Teaches character mapping concepts and how to handle bijections efficiently.
- Commonly encountered in string manipulation problems and coding interviews.

#### Prerequisites
- Knowledge of hashmaps (dictionaries).
- Understanding of string manipulation and iteration.

#### Industry Relevance
- Useful for pattern matching problems, such as compiler design, data transformation, and cryptography.

#### Follow-up Practice Problems
1. [290. Word Pattern](https://leetcode.com/problems/word-pattern/)
2. [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
3. [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
