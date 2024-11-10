
# Problem 682: Baseball Game

#### [UMPIRE Method]


### Understanding

- **Problem Statement**:  
  You are given a list of strings, where each string represents an operation in a baseball game. The goal is to calculate the total score after all operations are performed.
  
- **Operations**:
  - Integer (`x`): Add `x` points to the record.
  - `"+"`: Record the sum of the previous two scores.
  - `"D"`: Record double the previous score.
  - `"C"`: Remove the last recorded score.

- **Input**:
  - A list of strings, `operations`, with the possible operations as described.
  
- **Output**:
  - An integer representing the total score.

- **Constraints**:
  - The size of `operations` is between `1` and `1000`.
  - Each operation is either an integer or one of the strings `"+"`, `"D"`, or `"C"`.
  
- **Assumptions**:
  - Input values are valid and follow the rules of the game.
  - It is safe to assume the operations will have enough history to perform actions like adding the previous two scores or doubling the previous score.
---
### Match

- **Problem Pattern**: Stack-based simulation
- **Reasoning**:
  - Each operation affects only the most recent scores or entries.
  - We can manage operations with a stack data structure, as it allows easy access to recent entries for actions like adding, doubling, or removing scores.

---
### Plan

1. **Initialize**:
   - Create an empty list, `record`, to store the valid scores.
   
2. **Process Each Operation**:
   - **If the operation is an integer**: Convert to an integer and add to `record`.
   - **If the operation is `"+"`**: Sum the last two elements of `record` and append the result to `record`.
   - **If the operation is `"D"`**: Append double the last element of `record`.
   - **If the operation is `"C"`**: Remove the last element from `record`.
   
3. **Calculate Total Score**:
   - Sum all values in `record` to get the final score.

4. **Time Complexity**:  
   - Each operation is performed in O(1) time, and summing the list is O(n), making the overall complexity O(n).
   
5. **Space Complexity**:
   - The `record` list requires O(n) space for storing scores.
---
### Implementation

```python
def calPoints(operations):
    # Initialize an empty list to store the record of scores
    record = []

    # Loop through each operation in the operations list
    for op in operations:
        if op == '+':
            # Add the sum of the last two scores
            record.append(record[-1] + record[-2])
        elif op == 'D':
            # Double the last score and add it to the record
            record.append(2 * record[-1])
        elif op == 'C':
            # Remove the last score from the record
            record.pop()
        else:
            # Convert the operation to an integer and add it to the record
            record.append(int(op))

    # Sum all elements in the record to get the total score
    return sum(record)
```

#### Step-by-Step Explanation of Code

1. **Initialize `record`**:  
   `record` keeps track of valid scores after each operation.

2. **Loop through `operations`**:
   - **"+" operation**:
     - Access the last two scores using `record[-1]` and `record[-2]`.
     - Sum them and append to `record`.
   - **"D" operation**:
     - Double the last score (`record[-1]`) and append the result to `record`.
   - **"C" operation**:
     - Use `pop()` to remove the most recent score from `record`.
   - **Integer operation**:
     - Convert `op` to integer and append to `record`.

3. **Calculate Total**:
   - Use `sum(record)` to return the total score.
---
### Review

- **Edge Cases**:
  - Single operation (e.g., `["5"]`): Should correctly return `5`.
  - Consecutive `"C"` operations: Should remove multiple previous scores.
  - Minimal number of elements (like empty record handling): Confirm that each operation requiring past scores is handled only when valid.
  
- **Testing**:
  - Test cases with only integers, only operations (`+`, `D`, `C`), and a mix of all types.
  - Ensure results match expected scores after each operation.
---
### Evaluate

- **Time Complexity**: O(n), where `n` is the number of operations.
- **Space Complexity**: O(n), for the `record` list to store the scores.
---
### Why This Problem is Important

This problem is relevant in scenarios requiring cumulative record tracking, such as finance or scoring systems, where each action depends on prior entries. Understanding stack-based operations can be beneficial for applications requiring similar history-dependent operations.

### Industry Relevance

Simulations of record tracking and stack-based manipulation are common in scenarios like version control, game scoring, and event logs, making this problem a practical example of stack utility in software development.

### Prerequisites for Practicing This Problem

1. Familiarity with stack operations (push, pop).
2. Understanding list manipulation in Python.
3. Basic knowledge of time and space complexity.

### Follow-Up Practice Problems

- LeetCode Problem 394: Decode String
- LeetCode Problem 844: Backspace String Compare
- LeetCode Problem 150: Evaluate Reverse Polish Notation
