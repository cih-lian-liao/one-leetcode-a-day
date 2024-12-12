# LeetCode Problem 451: Sort Characters By Frequency

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

**Problem Statement:**
Given a string `s`, sort it in decreasing order based on the frequency of the characters. Return the sorted string.

**Inputs:**
- A string `s` containing characters (both uppercase and lowercase letters are possible).

**Outputs:**
- A string where characters are sorted by frequency in descending order.

**Constraints:**
- `1 <= s.length <= 5 * 10^5`
- Characters are case-sensitive (e.g., 'A' and 'a' are treated as different characters).

**Assumptions:**
- If two characters have the same frequency, they can appear in any order in the output.

### **2. Match**

This problem can be matched to the **frequency counting and sorting** pattern:
- Use a **hashmap (dictionary)** to count the frequency of each character.
- Use a **heap** or **sorting algorithm** to order characters based on frequency.

### **3. Plan**

**Steps to Solve:**

1. **Frequency Count:**
   - Traverse the string `s` and count the frequency of each character using a dictionary.

2. **Heap for Sorting:**
   - Use a max heap to sort characters by frequency. In Python, simulate a max heap by pushing the negative frequency.

3. **Build the Result String:**
   - Extract elements from the heap and construct the result string by repeating characters based on their frequency.

4. **Alternative Solution with Sorting:**
   - Count character frequencies using a dictionary.
   - Convert the dictionary to a list of tuples and sort it by frequency in descending order.
   - Build the result string based on the sorted list.

**Edge Cases:**
- Single character strings: "a" -> "a".
- Strings with equal frequency for all characters: "abc" -> Any permutation of "abc".

**Estimated Time Complexity:**
- Using Heap: O(n + k log k), where `n` is the length of the string and `k` is the number of unique characters.
- Using Sorting: O(n + k log k).

**Estimated Space Complexity:**
- Dictionary for frequency: O(k).
- Heap or sorted list: O(k).
- Result string: O(n).
- **Total:** O(n + k).

### **4. Implement**

#### Solution 1: Using Heap
```python
import heapq

def frequencySort(s: str) -> str:
    # Step 1: Count frequency of each character
    freq_map = {}
    for char in s:
        freq_map[char] = freq_map.get(char, 0) + 1

    # Step 2: Build a max heap
    max_heap = [(-freq, char) for char, freq in freq_map.items()]
    heapq.heapify(max_heap)

    # Step 3: Construct the result string
    result = []
    while max_heap:
        freq, char = heapq.heappop(max_heap)
        result.append(char * -freq)

    return ''.join(result)
```

#### Solution 2: Using Sorting
```python
def frequencySort(s: str) -> str:
    # Step 1: Count frequency of each character
    freq_map = {}
    for char in s:
        freq_map[char] = freq_map.get(char, 0) + 1

    # Step 2: Sort and copy by frequency in descending order
    sorted_chars = sorted(freq_map.items(), key=lambda x: x[1], reverse=True)

    # Step 3: Construct the result string
    result = []
    for char, freq in sorted_chars:
        result.append(char * freq)

    return ''.join(result)
```

**Notes for Implementation:**
1. In Solution 1, the `max_heap` stores negative frequencies to simulate a max heap using Python's `heapq`.
2. Solution 2 uses Python's `sorted` function to achieve the same goal without a heap.
3. Both solutions construct the result string efficiently using list comprehensions or loops.

### **5. Review**

#### **Debugging and Testing:**

- Test Case 1:
  ```python
  s = "tree"
  print(frequencySort(s))  # Expected Output: "eert" or "eetr"
  ```

- Test Case 2:
  ```python
  s = "cccaaa"
  print(frequencySort(s))  # Expected Output: "cccaaa" or "aaaccc"
  ```

- Test Case 3:
  ```python
  s = "Aabb"
  print(frequencySort(s))  # Expected Output: "bbAa" or "bbaA"
  ```

#### **Complex Case Dry Run:**

Input: `s = "aaaabbbbccccdddd"`

- Frequency Map: `{ 'a': 4, 'b': 4, 'c': 4, 'd': 4 }`
- Max Heap: `[(-4, 'a'), (-4, 'b'), (-4, 'c'), (-4, 'd')]`
- Extracting from heap:
  1. Pop `(-4, 'a')` -> Append "aaaa" to result.
  2. Pop `(-4, 'b')` -> Append "bbbb" to result.
  3. Pop `(-4, 'c')` -> Append "cccc" to result.
  4. Pop `(-4, 'd')` -> Append "dddd" to result.
- Final Output: "aaaabbbbccccdddd".

### **6. Evaluate**

#### **Time Complexity:**
- Using Heap: O(n + k log k).
- Using Sorting: O(n + k log k).

#### **Space Complexity:**
- Frequency map: O(k).
- Heap or sorted list: O(k).
- Result string: O(n).
- **Total:** O(n + k).

#### **Optimizations:**
- If `k` (number of unique characters) is small, using a bucket sort could reduce complexity.

---

### **Additional Notes**

#### **The Reason Why This Problem is Important**
- Tests your ability to work with frequency counting and priority queues.
- Demonstrates skills in sorting and constructing results efficiently.

#### **Prerequisites for Practicing This Problem**
- Understanding of dictionaries and heaps in Python.
- Familiarity with basic string operations.

#### **Industry Relevance**
- Relevant in scenarios involving text analysis, e.g., finding most common elements or ranking items by frequency.

#### **Follow-up Practice Problems**
1. [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
2. [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
3. [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)
4. [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
