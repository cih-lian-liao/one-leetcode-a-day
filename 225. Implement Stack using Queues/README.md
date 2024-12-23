# LeetCode Problem 225: Implement Stack using Queues

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**

#### Problem Statement

Implement a **stack** (Last-In-First-Out, LIFO) using **queues** (First-In-First-Out, FIFO). The stack must support the following operations:

1. `push(x)`: Pushes element `x` onto the stack.
2. `pop()`: Removes the element on the top of the stack and returns it.
3. `top()`: Returns the element on the top of the stack without removing it.
4. `empty()`: Returns whether the stack is empty.

#### Inputs and Outputs

- **Inputs:**
  - Operations: `push`, `pop`, `top`, and `empty`.
  - Values: Integer elements to push.

- **Outputs:**
  - `pop()` and `top()` return the value at the stack's top.
  - `empty()` returns a boolean.

#### Constraints

- The number of operations will be in the range [1, 100].
- All values are integers.
- All operations are valid (e.g., no `pop()` on an empty stack).

#### Assumptions

- The stack is implemented using one or two queues.
- Follow the expected stack behavior.

---

### **2. Match**

#### Problem Type

This problem focuses on **data structure simulation** and involves implementing one data structure (stack) using another (queue).

#### Related Problems

- Implementing queues using stacks.
- Simulating deque behavior with stacks or queues.

#### Data Structures

- **Queue:** The FIFO structure will simulate the LIFO behavior of a stack.

---

### **3. Plan**


#### **Solution 1: Single Queue Implementation**

1. Use a single queue (`queue`) to store the stack elements.
2. When `push(x)` is called:
   - Add `x` to the queue.
   - Rotate the queue (move all elements except the new one to the back).
3. `pop()` removes the front of the queue.
4. `top()` returns the front of the queue.
5. `empty()` checks if the queue is empty.


#### **Solution 2: Dual Queue Implementation**

1. Use two queues (`queue1` and `queue2`).
2. When `push(x)` is called:
   - Add `x` to `queue2`.
   - Move all elements from `queue1` to `queue2`.
   - Swap `queue1` and `queue2`.
3. `pop()` removes the front of `queue1`.
4. `top()` returns the front of `queue1`.
5. `empty()` checks if `queue1` is empty.

---

### **4. Implementation**

#### **Solution 1: Single Queue Implementation**

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        # Add the element to the queue
        self.queue.append(x)
        # Rotate the queue to make the last element the first
        for _ in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())

    def pop(self) -> int:
        # Remove the first element (stack top)
        return self.queue.popleft()

    def top(self) -> int:
        # Return the first element without removing
        return self.queue[0]

    def empty(self) -> bool:
        # Check if the queue is empty
        return not self.queue
```


#### **Solution 2: Dual Queue Implementation**

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.queue1 = deque()
        self.queue2 = deque()

    def push(self, x: int) -> None:
        # Add to the secondary queue
        self.queue2.append(x)
        # Move all elements from queue1 to queue2
        while self.queue1:
            self.queue2.append(self.queue1.popleft())
        # Swap the queues
        self.queue1, self.queue2 = self.queue2, self.queue1

    def pop(self) -> int:
        # Remove the first element from queue1
        return self.queue1.popleft()

    def top(self) -> int:
        # Return the first element of queue1
        return self.queue1[0]

    def empty(self) -> bool:
        # Check if queue1 is empty
        return not self.queue1
```

---

### **5. Review**

#### Dry Run Example

Given operations:
```python
stack = MyStack()
stack.push(1)
stack.push(2)
stack.push(3)
print(stack.top())  # Expected: 3
print(stack.pop())  # Expected: 3
print(stack.empty())  # Expected: False
```

1. **Single Queue Implementation**:
   - `push(1)`: `queue = [1]`
   - `push(2)`: `queue = [2, 1]`
   - `push(3)`: `queue = [3, 2, 1]`
   - `top()`: Returns `3`.
   - `pop()`: Removes `3`, leaving `queue = [2, 1]`.
   - `empty()`: Returns `False`.

2. **Dual Queue Implementation**:
   - `push(1)`: `queue1 = [1]`, `queue2 = []`
   - `push(2)`: `queue2 = [2, 1]`, `queue1 = []` after swapping.
   - `push(3)`: `queue2 = [3, 2, 1]`, `queue1 = []` after swapping.
   - `top()`: Returns `3`.
   - `pop()`: Removes `3`, leaving `queue1 = [2, 1]`.
   - `empty()`: Returns `False`.

---

### **6. Evaluate**

#### Complexity Analysis

1. **Single Queue**:
   - `push`: O(n) due to rotation.
   - `pop`: O(1).
   - `top`: O(1).
   - `empty`: O(1).

2. **Dual Queue**:
   - `push`: O(n) due to moving elements.
   - `pop`: O(1).
   - `top`: O(1).
   - `empty`: O(1).

#### Space Complexity

- Both solutions use O(n) space for storing elements.

---

### **Additional Notes**

#### Why This Problem is Important

- It demonstrates how to simulate one data structure using another.
- Highlights the concept of "queue manipulation" to achieve stack-like behavior.

#### Prerequisites

- Familiarity with stack and queue data structures.
- Understanding of their operational differences.

#### Industry Relevance

- Simulation problems often appear in coding interviews.
- Related to implementing efficient data structures in real-world scenarios.

#### Follow-up Practice Problems

1. LeetCode 232: Implement Queue using Stacks
2. LeetCode 622: Design Circular Queue
3. LeetCode 146: LRU Cache
