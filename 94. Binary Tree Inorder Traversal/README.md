# LeetCode Problem 94: Binary Tree Inorder Traversal

### Problem Statement

Given the root of a binary tree, return the inorder traversal of its nodes' values.

### Input
- `root`: the root node of a binary tree.

### Output
- A list of integers representing the inorder traversal of the tree.

### Constraints
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

### Goal
Perform an inorder traversal of a binary tree and return the list of values.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

The problem requires an **inorder traversal** of a binary tree. In an inorder traversal:
1. Traverse the left subtree.
2. Visit the node.
3. Traverse the right subtree.

#### Inputs, Outputs, and Constraints
- **Input**: Root node of the binary tree.
- **Output**: List of integers in inorder traversal order.
- **Constraints**: Each node's value is an integer within the specified range.

### 2. Match

This problem aligns with the common **tree traversal** patterns. We can approach this problem using:
- **Recursive traversal**: Straightforward method using function calls to handle traversal.
- **Iterative traversal**: Uses a stack to manually traverse the nodes in the inorder sequence.

### Solution 1: Recursive Approach

### 3. Plan

1. Define a helper function, `inorder`, to perform the traversal.
2. In the helper function, check if the current node (`node`) is `None`. If it is, return immediately to stop further processing (base case).
3. Call `inorder` on the left child of the current node to traverse the left subtree.
4. After returning from the left subtree, append the current node's value to the `result` list.
5. Call `inorder` on the right child of the current node to traverse the right subtree.
6. Initialize the `result` list and start the traversal by calling `inorder` with the root node.
7. Return `result`, which will now contain the inorder traversal of the binary tree.

### 4. Implement

```python
def inorderTraversal(root):
    # Initialize an empty list to store the result of the inorder traversal
    result = []

    # Helper function to perform inorder traversal
    def inorder(node):
        # Base case: if the node is None, return to avoid further processing
        if not node:
            return
        # Recursive call on the left child to traverse left subtree
        inorder(node.left)
        # Visit the current node by appending its value to result
        result.append(node.val)
        # Recursive call on the right child to traverse right subtree
        inorder(node.right)
    
    # Start the traversal with the root node
    inorder(root)
    # Return the populated result list containing the inorder traversal
    return result
```

### Example Dry Run

Consider a more complex binary tree structured as follows:

```
        4
       / \
      2   6
     / \ / \
    1  3 5  7
```

For this tree, an inorder traversal would result in `[1, 2, 3, 4, 5, 6, 7]`.

#### Recursive Dry Run Steps
1. Call `inorderTraversal(root)`:
   - `root` is node `4`.
2. Call `inorder(4)`:
   - Traverse left child: `inorder(4.left)` (node `2`).
3. Call `inorder(2)`:
   - Traverse left child: `inorder(2.left)` (node `1`).
4. Call `inorder(1)`:
   - Traverse left child: `inorder(1.left)` (None), return immediately.
   - Visit node `1`, append `1` to `result`.
   - Traverse right child: `inorder(1.right)` (None), return immediately.
5. Return to `inorder(2)`:
   - Visit node `2`, append `2` to `result`.
   - Traverse right child: `inorder(2.right)` (node `3`).
6. Call `inorder(3)`:
   - Traverse left child: `inorder(3.left)` (None), return immediately.
   - Visit node `3`, append `3` to `result`.
   - Traverse right child: `inorder(3.right)` (None), return immediately.
7. Return to `inorder(4)`:
   - Visit node `4`, append `4` to `result`.
   - Traverse right child: `inorder(4.right)` (node `6`).
8. Call `inorder(6)`:
   - Traverse left child: `inorder(6.left)` (node `5`).
9. Call `inorder(5)`:
   - Traverse left child: `inorder(5.left)` (None), return immediately.
   - Visit node `5`, append `5` to `result`.
   - Traverse right child: `inorder(5.right)` (None), return immediately.
10. Return to `inorder(6)`:
    - Visit node `6`, append `6` to `result`.
    - Traverse right child: `inorder(6.right)` (node `7`).
11. Call `inorder(7)`:
    - Traverse left child: `inorder(7.left)` (None), return immediately.
    - Visit node `7`, append `7` to `result`.
    - Traverse right child: `inorder(7.right)` (None), return immediately.

The final `result` list is `[1, 2, 3, 4, 5, 6, 7]`.

### 6. Evaluate

- **Time Complexity**: O(n) - Each node is visited once.
- **Space Complexity**: O(h) - Call stack depth, where `h` is the height of the tree. In the worst case, the height `h` could be n if the tree is highly unbalanced (e.g., a tree where every node only has one child, resembling a linked list). In this case, the space complexity becomes O(n) because we need `n` recursive calls. In the best case (a perfectly balanced binary tree), the height `h` is log(n), and thus the space complexity is O(log(n)).

---

### Solution 2: Iterative Approach

### 3. Plan

1. Initialize an empty list, `result`, to store the traversal result.
2. Use a stack to keep track of nodes to be visited. Initialize an empty stack.
3. Set `current` to `root`. This variable will be used to navigate through the tree.
4. Loop while `current` is not `None` or there are nodes in the stack:
   - **Inner loop**: While `current` is not `None`, push `current` onto the stack and move to its left child.
     - This helps in traversing down the leftmost path, which is the first step in inorder traversal.
   - **Visit node**: Once there are no more left children, pop the node from the stack, visit it by appending its value to `result`, and set `current` to the right child.
     - Setting `current` to the right child allows traversal to continue on the right subtree after visiting the left subtree and the current node.
5. The loop will end when all nodes have been visited and `stack` is empty. Return `result`.

### 4. Implement

```python
def inorderTraversal(root):
    # Initialize an empty list to store the result of the inorder traversal
    result = []
    # Initialize an empty stack to help with iterative traversal
    stack = []
    # Start with the root as the current node
    current = root

    # Loop until there are no more nodes to process
    while current or stack:
        # Traverse left subtree by going to the leftmost node
        while current:
            # Push current node to stack to revisit after left subtree
            stack.append(current)
            # Move to the left child
            current = current.left
        
        # Current is None here, so pop from stack to visit the node
        current = stack.pop()
        # Visit the node by appending its value to result
        result.append(current.val)

        # Move to the right subtree of the visited node
        current = current.right
    
    # Return the populated result list containing the inorder traversal
    return result
```

### Example Dry Run

Using the same tree as in the recursive example:

```
        4
       / \
      2   6
     / \ / \
    1  3 5  7
```

The inorder traversal would result in `[1, 2, 3, 4, 5, 6, 7]`.

#### Iterative Dry Run Steps
1. Initialize `result = []`, `stack = []`, and `current = root` (node `4`).
2. **First loop**:
   - **Inner loop**: Push `4` to `stack`, move `current` to `4.left` (node `2`).
   - **Inner loop**: Push `2` to `stack`, move `current` to `2.left` (node `1`).
   - **Inner loop**: Push `1` to `stack`, move `current` to `1.left` (None).
3. Pop from `stack`, visit node `1`, append `1` to `result`.
   - Set `current = 1.right` (None).
4. Pop from `stack`, visit node `2`, append `2` to `result`.
   - Set `current = 2.right` (node `3`).
5. **Second loop
   - **Inner loop**: Push `3` to `stack`, move `current` to `3.left` (None).
6. Pop from `stack`, visit node `3`, append `3` to `result`.
   - Set `current = 3.right` (None).
7. Pop from `stack`, visit node `4`, append `4` to `result`.
   - Set `current = 4.right` (node `6`).
8. **Third loop**:
   - **Inner loop**: Push `6` to `stack`, move `current` to `6.left` (node `5`).
   - **Inner loop**: Push `5` to `stack`, move `current` to `5.left` (None).
9. Pop from `stack`, visit node `5`, append `5` to `result`.
   - Set `current = 5.right` (None).
10. Pop from `stack`, visit node `6`, append `6` to `result`.
    - Set `current = 6.right` (node `7`).
11. **Fourth loop**:
    - **Inner loop**: Push `7` to `stack`, move `current` to `7.left` (None).
12. Pop from `stack`, visit node `7`, append `7` to `result`.
    - Set `current = 7.right` (None).

The final `result` list is `[1, 2, 3, 4, 5, 6, 7]`.

### 6. Evaluate

- **Time Complexity**: O(n) - Each node is visited once, and each node is pushed and popped from the stack once.
- **Space Complexity**: O(h) - Stack space for nodes, where `h` is the height of the tree. For a balanced binary tree, where the height `h` is about log(n), the space complexity will be O(log(n)). In the worst case, such as a completely unbalanced tree (like a linked list), the stack would need to store all `n` nodes on a single path, leading to a space complexity of O(n).

---

### Why This Problem is Important

### Industry Relevance
Understanding tree traversals is essential in software engineering, as they are frequently encountered in algorithms and data structure questions during technical interviews. The ability to write both recursive and iterative solutions demonstrates flexibility and knowledge of underlying structures.

### Prerequisites for Practicing This Problem
- Basic understanding of binary trees.
- Familiarity with recursive and iterative techniques for solving problems.
- Knowledge of stack data structures for the iterative approach.

### Follow-up Practice Problems
- **LeetCode 144**: Binary Tree Preorder Traversal
- **LeetCode 145**: Binary Tree Postorder Traversal
- **LeetCode 102**: Binary Tree Level Order Traversal
