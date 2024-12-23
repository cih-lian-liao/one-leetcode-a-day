
# LeetCode Problem 71: Simplify Path

### Problem Description

Given a string `path`, representing an absolute path (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

### Inputs:
- A string `path`, consisting of English letters, digits, periods (`.`), slashes (`/`), or underscores (`_`).

### Outputs:
- A string representing the simplified canonical path.

### Constraints:
1. `1 <= path.length <= 3000`
2. `path` contains only English letters, digits, periods, slashes, or underscores.
3. The canonical path must start with a single slash `'/'`.
4. The canonical path must not end with a trailing slash.
5. The canonical path must not have multiple consecutive slashes.
6. The canonical path must not include `..` (parent directory) or `.` (current directory).

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**
- The goal is to simplify the Unix-style path:
  - Ignore `.` (current directory).
  - Handle `..` (parent directory) by moving up one level if possible.
  - Remove extra or redundant slashes.
- Examples:
  - Input: `"/a/./b/../../c/"` → Output: `"/c"`
  - Input: `"/../"` → Output: `"/"`
  - Input: `"/home//foo/"` → Output: `"/home/foo"`

---

### **2. Match**
- This problem involves **string processing**.
- A **stack** is well-suited for managing directory traversal:
  - Push valid directory names onto the stack.
  - Pop from the stack when encountering `..`.

---

### **3. Plan**

#### Plan for Solution 1 (Using `split`):
1. Split the string `path` using the `/` delimiter to get components.
2. Initialize an empty stack.
3. Iterate through the components:
   - Skip `.` and empty strings.
   - If `..` is encountered:
     - Pop from the stack if it's not empty.
   - Otherwise, push the component onto the stack.
4. Join the stack contents with `/` to form the canonical path.
5. Add a leading `/` to the result.

#### Plan for Solution 2 (Manual Parsing Without `split`):
1. Initialize an empty stack and a pointer `i` for traversal.
2. Traverse the string character by character:
   - Skip consecutive slashes `/`.
   - Identify a directory or command (`..`, `.`, or valid directory name) by tracking its start and end.
   - Process each command:
     - If `..`, pop the stack if it's not empty.
     - If `.`, ignore it.
     - Otherwise, push the valid directory name onto the stack.
3. Combine the stack contents with `/` to form the canonical path, prefixed by a leading `/`.

---

### **4. Implement**

#### Implementation of Solution 1 (Using `split`):
```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        components = path.split("/")
        
        for component in components:
            if component == "..":
                if stack:
                    stack.pop()
            elif component == "." or component == "":
                continue
            else:
                stack.append(component)
        
        return "/" + "/".join(stack)
```

#### Implementation of Solution 2 (Manual Parsing Without `split`):
```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        i = 0
        n = len(path)
        
        while i < n:
            # Skip consecutive slashes
            while i < n and path[i] == '/':
                i += 1
            
            # Identify a directory or command
            start = i
            while i < n and path[i] != '/':
                i += 1
            component = path[start:i]
            
            if component == "..":
                if stack:
                    stack.pop()
            elif component == "." or component == "":
                continue
            else:
                stack.append(component)
        
        return "/" + "/".join(stack)
```

---

### **5. Review**

#### Test Cases:
```python
solution = Solution()

# Test Case 1
assert solution.simplifyPath("/home/") == "/home"

# Test Case 2
assert solution.simplifyPath("/../") == "/"

# Test Case 3
assert solution.simplifyPath("/home//foo/") == "/home/foo"

# Test Case 4
assert solution.simplifyPath("/a/./b/../../c/") == "/c"

# Test Case 5
assert solution.simplifyPath("/a/../../b/../c//.//") == "/c"

# Test Case 6
assert solution.simplifyPath("/a//b////c/d//././/..") == "/a/b/c"
```

#### Dry Run:
Input: `"/a/./b/../../c/"`
1. Split into components: `["", "a", ".", "b", "..", "..", "c", ""]`
2. Stack operations:
   - `"a"` → Push `"a"`.
   - `"."` → Ignore.
   - `"b"` → Push `"b"`.
   - `".."` → Pop `"b"`.
   - `".."` → Pop `"a"`.
   - `"c"` → Push `"c"`.
3. Result: `"/c"`

---

### **6. Evaluate**

#### Time Complexity:
- Solution 1: O(n) (string traversal and stack operations).
- Solution 2: O(n) (manual traversal and stack operations).

#### Space Complexity:
- Solution 1: O(n) (stack stores directory names).
- Solution 2: O(n) (stack stores directory names).

---

### **Additional Notes**

#### Why This Problem is Important:
- Frequently tested in interviews to assess **string manipulation** and **stack usage**.
- Helps build proficiency in handling edge cases.

#### Prerequisites:
- Understanding of stacks and their operations (push/pop).
- Familiarity with string manipulation techniques.

#### Industry Relevance:
- Parsing file paths is a common operation in operating systems and web development.

#### Follow-up Practice Problems:
1. [LeetCode 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
2. [LeetCode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
3. [LeetCode 394: Decode String](https://leetcode.com/problems/decode-string/)
