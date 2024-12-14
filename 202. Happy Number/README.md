# LeetCode Problem 202: Happy Number

### Problem Description

A **happy number** is a number defined by the following process:
1. Starting with any positive integer, replace the number by the sum of the squares of its digits.
2. Repeat the process until the number equals `1` (where it will stay), or it loops endlessly in a cycle that does not include `1`.
3. Those numbers for which this process ends in `1` are happy numbers.

### Example:
#### Input: `19`
#### Output: `true`

Explanation:
- 1² + 9² = 82
- 8² + 2² = 68
- 6² + 8² = 100
- 1² + 0² + 0² = 1 (Happy number)

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
**Goal**: Determine if a given number is a happy number.

**Input**:
- A positive integer `n`.

**Output**:
- A boolean (`true` or `false`), indicating whether `n` is a happy number.

**Constraints**:
- The process will always eventually reach a loop or terminate at `1`.

**Ambiguities**:
- Handle large numbers efficiently.
- Ensure the detection of cycles to prevent infinite loops.

---

### 2. Match
This problem can be matched to the **Cycle Detection** pattern:
- The process of repeatedly calculating the sum of squares of digits may lead to a loop. Detecting such a loop efficiently is essential.
- The **Floyd’s Cycle Detection Algorithm (fast and slow pointers)** is ideal here.

---

### 3. Plan
We will use the following steps:
1. Define a helper function `get_next(number)` to calculate the sum of the squares of the digits of `number`.
2. Use two pointers (`slow` and `fast`) to traverse the sequence:
   - `slow` moves one step at a time.
   - `fast` moves two steps at a time.
3. If `fast` pointer reaches `1`, the number is happy.
4. If `slow` equals `fast` in any other position, there is a cycle, and the number is not happy.

#### Pseudocode:
```text
function isHappy(n):
    define helper get_next(number):
        calculate sum of squares of digits
    slow = n
    fast = get_next(n)
    while fast != 1 and slow != fast:
        slow = get_next(slow)
        fast = get_next(get_next(fast))
    return fast == 1
```

**Time Complexity**: O(logn)  
**Space Complexity**: O(1)

---

### 4. Implement
Here is the Python implementation with detailed notes:

```python
def isHappy(n):
    # Helper function to calculate the sum of squares of digits
    def get_next(number):
        total_sum = 0
        while number > 0:
            digit = number % 10  # Extract the last digit
            total_sum += digit ** 2  # Square the digit and add to the sum
            number //= 10  # Remove the last digit
        return total_sum

    slow = n  # Slow pointer starts at the input number
    fast = get_next(n)  # Fast pointer starts at the next number

    # Continue until fast pointer reaches 1 or slow meets fast (indicating a cycle)
    while fast != 1 and slow != fast:
        slow = get_next(slow)  # Slow pointer moves one step
        fast = get_next(get_next(fast))  # Fast pointer moves two steps

    # If fast reaches 1, it's a happy number
    return fast == 1
```

---

### 5. Review

#### Dry Run:
Example Input: `n = 19`

**Step-by-step Execution**:
1. Initial values:
   - `slow = 19`
   - `fast = get_next(19) = 82`
2. First iteration:
   - `slow = get_next(19) = 82`
   - `fast = get_next(get_next(82)) = get_next(68) = 100`
3. Second iteration:
   - `slow = get_next(82) = 68`
   - `fast = get_next(get_next(100)) = get_next(1) = 1`
4. `fast` reaches `1`. The number `19` is happy.

**Edge Cases**:
- Input: `1`  
  Output: `true` (1 is always happy).
- Input: `2`  
  Output: `false` (2 enters a cycle: 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4).

---

### 6. Evaluate

#### Time Complexity:
- Each call to `get_next` processes the digits of the number.
- For a number with `d` digits, the number of operations is proportional to \( O(d) \).
- The sum of the squares of the digits reduces the number quickly, leading to a logarithmic reduction in number size.  
  **Overall Complexity**: O(logn)

#### Space Complexity:
- We use constant space for the pointers and variables.  
  **Overall Complexity**: O(1)

#### Optimizations:
- Use a set to store visited numbers instead of the fast-slow pointer approach (but at the cost of O(n) space).

---

### Additional Notes

#### Why This Problem is Important:
- Tests your ability to implement Floyd’s Cycle Detection, a fundamental algorithm used in many applications.
- Helps you understand iterative processes and how to detect cycles efficiently.

#### Prerequisites for Practicing This Problem:
- Understanding of modular arithmetic (`%` and `//`).
- Familiarity with Floyd’s Cycle Detection Algorithm.

#### Industry Relevance:
- Cycle detection algorithms are widely used in computer science, including:
  - Detecting loops in linked lists.
  - Identifying repetitive sequences in data processing.

#### Follow-up Practice Problems:
1. [LeetCode 141: Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
2. [LeetCode 142: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
3. [LeetCode 457: Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)

---

#### Key Takeaways:
- Floyd’s Cycle Detection is a powerful tool for identifying cycles in iterative processes.
- Understanding the mathematical properties of digit sums can help optimize numerical problems.
