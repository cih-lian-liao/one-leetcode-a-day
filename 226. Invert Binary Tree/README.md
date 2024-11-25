
# LeetCode 226: Invert Binary Tree

### Problem Description

Invert a binary tree such that for every node, its left and right children are swapped.

### Input
- A root of a binary tree.

### Output
- The root of the inverted binary tree.

### Constraints
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

1. Fully grasp the problem:
   - You are given the root of a binary tree. For every node, you need to swap its left and right subtrees.

2. Inputs:
   - A binary tree node (the root of the tree).

3. Outputs:
   - The root of the inverted tree where all left and right children are swapped.

4. Constraints:
   - The tree can be empty (null root).
   - The node values are integers.

5. Assumptions:
   - Null trees (no root) return null.
   - Leaf nodes do not have children, so nothing changes for them.

---

### 2. Match

1. Problem Type:
   - Binary tree traversal.
   - Tree modification.

2. Pattern:
   - Depth-first search (DFS) or breadth-first search (BFS).

3. Data Structures:
   - Use recursion for DFS.
   - Use a queue for BFS if using iterative methods.

---

### 3. Plan

We will implement a recursive DFS approach to invert the binary tree:

1. Base Case:
   - If the root is `null`, return `null`.

2. Recursive Case:
   - Swap the left and right children of the current node.
   - Recursively call the function for the left child.
   - Recursively call the function for the right child.

3. Return the root after processing.

---

### 4. Implement (DFS Solution)
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invertTree(root):
    # Base case: If the tree is empty, return None
    if root is None:
        return None
    
    # Swap the left and right children
    root.left, root.right = root.right, root.left
    
    # Recursively invert the left and right subtrees
    invertTree(root.left)
    invertTree(root.right)
    
    # Return the root of the inverted tree
    return root
```

#### Implementation Notes

1. **Base Case**:
   - If the tree is empty (`root is None`), return `None`.

2. **Swap Operation**:
   - Use `root.left, root.right = root.right, root.left` to swap the left and right children.

3. **Recursive Call**:
   - Call `invertTree(root.left)` and `invertTree(root.right)` to process the children.

4. **Return Statement**:
   - After processing the entire tree, return the root.

---

### 5. Review

#### Dry Run
Input Tree:
```
    4
   / \
  2   7
 / \ / \
1  3 6  9
```

Step-by-step Execution:
1. Start at the root (4).
2. Swap the left (2) and right (7) children.
3. Recursively process the left child (7):
   - Swap its children (6 and 9).
4. Recursively process the right child (2):
   - Swap its children (1 and 3).
5. Return the root after all swaps.

Inverted Tree:
```
    4
   / \
  7   2
 / \ / \
9  6 3  1
```

#### Edge Cases
1. Empty tree (`root is None`):
   - Output: `None`.

2. Single node tree:
   - Input: `TreeNode(1)`
   - Output: `TreeNode(1)`.

---

### 6. Evaluate

1. **Time Complexity**:
   - O(n), where `n` is the number of nodes in the tree. Each node is visited once.

2. **Space Complexity**:
   - O(h), where `h` is the height of the tree (stack space for recursion).

---

### Additional Notes

### BFS Solution (Iterative)

```python
from collections import deque

def invertTree(root):
    if root is None:
        return None
    
    # Initialize a queue for level-order traversal
    queue = deque([root])
    
    while queue:
        current = queue.popleft()
        
        # Swap the left and right children
        current.left, current.right = current.right, current.left
        
        # Add children to the queue if they exist
        if current.left:
            queue.append(current.left)
        if current.right:
            queue.append(current.right)
    
    return root
```

#### Explanation
1. Use a queue to process nodes level by level.
2. Swap the left and right children of each node.
3. Add the children to the queue for further processing.
4. Return the root after all nodes are processed.

#### Time Complexity
- O(n), where `n` is the number of nodes in the tree. Each node is processed once.

#### Space Complexity
- O(w), where `w` is the maximum width of the tree (maximum number of nodes in any level).

---

### Why This Problem is Important
- Understanding tree traversal (DFS and BFS) is critical in software development and system design.
- Swapping and modifying tree structures are common operations in problems like serialization, deserialization, and tree balancing.

### Industry Relevance
- Used in graphics rendering (scene graphs) and database index manipulation.
- Tree manipulations are fundamental in search engines and compilers.

### Prerequisites for Practicing This Problem
1. Basics of binary trees.
2. Understanding of recursion.
3. Familiarity with queues for BFS.

### Follow-up Practice Problems
1. LeetCode 101: Symmetric Tree
2. LeetCode 100: Same Tree
3. LeetCode 94: Binary Tree Inorder Traversal
4. LeetCode 102: Binary Tree Level Order Traversal
5. LeetCode 617: Merge Two Binary Trees
