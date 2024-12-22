
# LeetCode Problem 232: Implement Queue using Stacks

### Problem Statement

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue: `push`, `pop`, `peek`, and `empty`.

### Operations:
1. `push(int x)`: Pushes element `x` to the back of the queue.
2. `pop()`: Removes the element from the front of the queue and returns it.
3. `peek()`: Returns the element at the front of the queue without removing it.
4. `empty()`: Returns `true` if the queue is empty, `false` otherwise.

### Constraints:
- You must use only standard operations of a stack: `push`, `pop`, `peek`, `isEmpty`.
- At most 100 calls will be made to `push`, `pop`, `peek`, and `empty`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input**: Commands for the queue (`push`, `pop`, `peek`, `empty`).
- **Output**:
  - `push`: No return value.
  - `pop`: Returns the front element of the queue.
  - `peek`: Returns the front element of the queue without removing it.
  - `empty`: Returns `true` or `false`.
- **Constraints**: The queue is implemented using only two stacks.

#### Clarifications:
1. Can we assume valid operations? Yes.
2. Are stack operations restricted to their standard methods? Yes.

---

### 2. Match

- **Known Problem Type**: Implementing a queue using stacks.
- **Relevant Data Structures**: 
  - Stack (LIFO behavior).
  - Two stacks can simulate FIFO behavior with careful transfer of elements.

---

### 3. Plan

We will use two stacks: `stack_in` and `stack_out`.
1. **`push(x)`**:
   - Push the element into `stack_in`.
   - Time complexity: O(1).

2. **`pop()`**:
   - If `stack_out` is empty:
     - Transfer all elements from `stack_in` to `stack_out` (reversing the order).
   - Pop the top element from `stack_out`.
   - Time complexity: Amortized O(1).

3. **`peek()`**:
   - If `stack_out` is empty:
     - Transfer all elements from `stack_in` to `stack_out`.
   - Return the top element of `stack_out` without removing it.
   - Time complexity: Amortized O(1).

4. **`empty()`**:
   - Return `true` if both `stack_in` and `stack_out` are empty, `false` otherwise.
   - Time complexity: O(1).

---

### 4. Implement

```python
class MyQueue:
    def __init__(self):
        # Initialize two stacks
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of the queue.
        """
        self.stack_in.append(x)

    def pop(self) -> int:
        """
        Removes the element from the front of the queue and returns it.
        """
        if not self.stack_out:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
        return self.stack_out.pop()

    def peek(self) -> int:
        """
        Get the front element of the queue.
        """
        if not self.stack_out:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
        return self.stack_out[-1]

    def empty(self) -> bool:
        """
        Returns true if the queue is empty, false otherwise.
        """
        return not self.stack_in and not self.stack_out
```

#### Implementation Notes:
1. `push` simply adds elements to `stack_in`, which is straightforward.
2. `pop` and `peek` utilize `stack_out` to reverse the order of `stack_in` when needed.
3. `empty` checks the emptiness of both stacks.

---

### 5. Review

#### Debugging:
1. **Edge Case 1**: Queue is empty, and `pop()` or `peek()` is called.  
   - These operations are valid as per the constraints, so no need for additional checks.
   
2. **Edge Case 2**: Alternating push and pop operations.
   - Example: Push(1), Push(2), Pop(), Push(3), Peek().
   - Validates the FIFO behavior.

#### Dry Run:
**Input**:
```python
myQueue = MyQueue()
myQueue.push(1)
myQueue.push(2)
print(myQueue.peek())  # Expected Output: 1
print(myQueue.pop())   # Expected Output: 1
print(myQueue.empty()) # Expected Output: False
```

**Execution**:
1. `push(1)`:
   - `stack_in`: [1], `stack_out`: []
2. `push(2)`:
   - `stack_in`: [1, 2], `stack_out`: []
3. `peek()`:
   - Transfer `stack_in` to `stack_out`: `stack_in`: [], `stack_out`: [2, 1]
   - Return top of `stack_out`: 1.
4. `pop()`:
   - Remove top of `stack_out`: 1. Result: `stack_in`: [], `stack_out`: [2].
5. `empty()`:
   - Both stacks are not empty. Return `False`.

---

### 6. Evaluate

#### Time Complexity:
1. `push`: O(1).
2. `pop`: Amortized O(1).
3. `peek`: Amortized O(1).
4. `empty`: O(1).

#### Space Complexity:
- The total space used by `stack_in` and `stack_out` is proportional to the number of elements in the queue.
- Space complexity: O(n).

#### Optimizations:
- This implementation is already optimal with amortized O(1) for all operations.

---

### Additional Notes

#### Why This Problem is Important:
- Demonstrates how to simulate one data structure using another, a fundamental concept in computer science.
- Tests understanding of stack behavior and complexity analysis.

#### Prerequisites for Practicing:
- Basic understanding of stacks and queues.
- Familiarity with time complexity analysis.

#### Industry Relevance:
- Such techniques are useful in system design when a direct implementation is not feasible, requiring adaptation with existing structures.

#### Follow-up Practice Problems:
1. LeetCode 225: Implement Stack using Queues.
2. LeetCode 622: Design Circular Queue.
3. LeetCode 146: LRU Cache (to practice data structure adaptation).
