# LeetCode 271: Encode and Decode Strings

### Problem Statement
Design an algorithm to encode a list of strings to a single string. The encoded string should be able to be decoded back to the original list of strings.

- **Encode**: Converts a list of strings into a single string.
- **Decode**: Reverts the encoded string back into the original list of strings.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs:**
- A list of strings `strs`.

**Outputs:**
- A single string (encoded) for the `encode` function.
- A list of strings for the `decode` function.

**Constraints:**
- Strings in the list can include any characters, including special characters and spaces.
- The encoded string must be uniquely decodable.

**Assumptions:**
- The input list can be empty.
- Each string in the list can be empty.

---

### 2. Match

This problem relates to:
- **String Manipulation**: Encoding and decoding strings.
- **Delimiter-Based Parsing**: Use of a delimiter (e.g., `#`) to separate components.

Key techniques include:
- Using the length of each string as a prefix to ensure unambiguous decoding.
- Iterative parsing for decoding.

---

### 3. Plan

**Encoding:**
1. Iterate over the list of strings.
2. For each string, append the length of the string followed by a delimiter `#` and the string itself.
3. Return the concatenated result as the encoded string.

**Decoding:**
1. Initialize an empty list to store the decoded strings.
2. Use a pointer `i` to iterate through the encoded string.
3. For each segment:
   - Parse the length of the next string until the `#` delimiter.
   - Extract the string using the parsed length.
   - Append the string to the result list and update the pointer.
4. Return the decoded list of strings.

---

### 4. Implement

```python
class Codec:
    def encode(self, strs: List[str]) -> str:
        """
        Encodes a list of strings to a single string.
        """
        encoded_string = ""
        for s in strs:
            # Append length of the string, followed by '#' and the string itself
            encoded_string += f"{len(s)}#{s}"
        return encoded_string

    def decode(self, s: str) -> List[str]:
        """
        Decodes a single string to a list of strings.
        """
        decoded_strings = []
        i = 0  # Initialize pointer

        while i < len(s):
            # Find the delimiter '#'
            j = s.find('#', i)
            
            # Extract the length of the next string
            length = int(s[i:j])
            
            # Extract the string of the given length
            decoded_strings.append(s[j+1:j+1+length])
            
            # Update pointer to the next segment
            i = j + 1 + length

        return decoded_strings
```

**Step-by-step Implementation Notes:**
- **Encoding:**
  - Use `f"{len(s)}#{s}"` to format the encoded string.
  - Iterate through all strings in `strs` and concatenate them with their lengths.
- **Decoding:**
  - Use `find` to locate the delimiter `#`.
  - Convert the substring before `#` to an integer to get the length.
  - Extract the substring of the given length and append to the result list.
  - Move the pointer to the next encoded segment.

---

### 5. Review

**Test Cases:**

#### Test Case 1
Input:
```python
strs = ["hello", "world"]
```
Execution:
```python
codec = Codec()
encoded = codec.encode(strs)
decoded = codec.decode(encoded)
print(encoded)  # Output: "5#hello5#world"
print(decoded)  # Output: ["hello", "world"]
```

#### Test Case 2
Input:
```python
strs = ["", "a", "#", "longerstring"]
```
Execution:
```python
codec = Codec()
encoded = codec.encode(strs)
decoded = codec.decode(encoded)
print(encoded)  # Output: "0#1#a1##13#longerstring"
print(decoded)  # Output: ["", "a", "#", "longerstring"]
```

#### Complex Test Case
Input:
```python
strs = ["123", "abc", "special#characters"]
```
Execution:
```python
codec = Codec()
encoded = codec.encode(strs)
decoded = codec.decode(encoded)
print(encoded)  # Output: "3#1233#abc20#special#characters"
print(decoded)  # Output: ["123", "abc", "special#characters"]
```

---

### 6. Evaluate

**Time Complexity:**
- **Encoding:** O(n), where `n` is the total length of all strings in `strs`.
- **Decoding:** O(n), where `n` is the length of the encoded string.

**Space Complexity:**
- **Encoding:** O(1) additional space (output string is not counted).
- **Decoding:** O(n), where `n` is the size of the decoded list.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It teaches delimiter-based parsing, a fundamental concept in handling strings in real-world applications like file I/O, data serialization, and networking.

#### Prerequisites for Practicing This Problem
- Understanding of string manipulation.
- Familiarity with basic iteration and pointer manipulation.

#### Industry Relevance
- Common in designing APIs, serialization, and deserialization tasks.
- Essential in building robust data parsers for web applications and distributed systems.

#### Follow-up Practice Problems
1. Serialize and Deserialize Binary Tree (LeetCode 297).
2. Basic Calculator II (LeetCode 227).
3. Simplify Path (LeetCode 71).
