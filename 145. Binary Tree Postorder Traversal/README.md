# LeetCode 145: Binary Tree Postorder Traversal

### Problem Description

Given the `root` of a binary tree, return the postorder traversal of its nodes' values.

**Postorder Traversal Order:** Left subtree -> Right subtree -> Root.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

- **Input:** A binary tree represented by its root node.
- **Output:** A list of integers representing the postorder traversal of the binary tree.
- **Constraints:**
  - The number of nodes in the tree is in the range `[0, 100]`.
  - `-100 <= Node.val <= 100`.
- **Edge Cases:**
  - Empty tree (`root = None`).
  - Tree with only one node.
  - Skewed tree (all nodes on one side).
  - Large tree with multiple levels.

---

### 2. Match

- **Pattern:** Binary tree traversal.
- **Relevant Algorithm:** Postorder traversal is typically recursive, but we’ll use an iterative approach with a stack for non-recursive traversal.
- **Data Structures Used:**
  - A stack to manage the traversal process.
  - A result list to store the postorder sequence.
---
### 3. Plan

1. **Initialization:**
   - If the tree is empty, return an empty list.
   - Use a stack to keep track of nodes and their visited status. Start by adding the root node with a `visited` flag set to `False`.
   - Use a result list to store the traversal sequence.

2. **Traversal Logic:**
   - While the stack is not empty:
     - Pop a node and its `visited` status from the stack.
     - If the node is `None`, skip it.
     - If the node is marked as `visited`, add its value to the result list.
     - Otherwise:
       - Push the current node back to the stack, marked as `visited`.
       - Push the right child (if it exists) to the stack.
       - Push the left child (if it exists) to the stack.

3. **Return Result:**
   - Once the stack is empty, return the result list.

4. **Time and Space Complexity:**
   - Time Complexity: O(n), where `n` is the number of nodes in the binary tree.
   - Space Complexity: O(h), where `h` is the height of the tree (stack space).

---

### 4. Implement

```python
def postorderTraversal(root):
    if not root:
        return []
    
    stack = [(root, False)]  # Stack to store nodes and visited status
    result = []  # List to store the postorder traversal
    
    while stack:
        node, visited = stack.pop()  # Pop the top element from the stack
        if node:  # Check if the node is not None
            if visited:
                # If visited, add the node's value to the result
                result.append(node.val)
            else:
                # Push the node back with visited=True
                stack.append((node, True))
                # Push the right and left children (if they exist)
                stack.append((node.right, False))
                stack.append((node.left, False))
    
    return result
```

#### Implementation Notes

1. **Initialization:**
   - We use a tuple `(node, visited)` to track whether the node’s children have been processed.

2. **Traversal Details:**
   - Mark a node as visited before adding its children to the stack.
   - Left and right children are added in the order required to ensure correct traversal.

3. **Edge Case Handling:**
   - An empty tree is handled by the initial check `if not root`.

4. **Code Clarity:**
   - Use meaningful variable names like `node` and `visited`.
   - Modularize traversal logic for reusability and readability.

---

### 5. Review

#### Dry Run Example

**Example Input:**

```
        1
       / \
      2   3
     / \    \
    4   5    6
```

**Execution Steps:**

1. Initialize: `stack = [(1, False)]`, `result = []`.
2. Process 1:
   - Pop `(1, False)`. Push `(1, True)`, `(3, False)`, `(2, False)`.
3. Process 2:
   - Pop `(2, False)`. Push `(2, True)`, `(5, False)`, `(4, False)`.
4. Process 4:
   - Pop `(4, False)`. Push `(4, True)`.
   - Pop `(4, True)`. Add `4` to result: `result = [4]`.
5. Process 5:
   - Pop `(5, False)`. Push `(5, True)`.
   - Pop `(5, True)`. Add `5` to result: `result = [4, 5]`.
6. Process 2:
   - Pop `(2, True)`. Add `2` to result: `result = [4, 5, 2]`.
7. Process 3:
   - Pop `(3, False)`. Push `(3, True)`, `(6, False)`.
8. Process 6:
   - Pop `(6, False)`. Push `(6, True)`.
   - Pop `(6, True)`. Add `6` to result: `result = [4, 5, 2, 6]`.
9. Process 3:
   - Pop `(3, True)`. Add `3` to result: `result = [4, 5, 2, 6, 3]`.
10. Process 1:
    - Pop `(1, True)`. Add `1` to result: `result = [4, 5, 2, 6, 3, 1]`.

**Final Output:** `[4, 5, 2, 6, 3, 1]`.

---

### 6. Evaluate

- **Time Complexity:** O(n), as each node is processed once.
- **Space Complexity:** O(h), where `h` is the height of the tree, due to the stack.

---

### Additional Notes

### Why This Problem is Important
- Teaches iterative traversal of binary trees.
- Reinforces understanding of stack-based algorithms.

### Industry Relevance
- Postorder traversal is used in algorithms like expression tree evaluation.

### Prerequisites for Practicing This Problem
- Understanding of binary tree structures.
- Familiarity with stack operations.

### Follow-Up Practice Problems
- LeetCode 94: Binary Tree Inorder Traversal
- LeetCode 144: Binary Tree Preorder Traversal
- LeetCode 102: Binary Tree Level Order Traversal
