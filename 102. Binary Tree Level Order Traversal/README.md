

# LeetCode 102: Binary Tree Level Order Traversal

### Problem Statement

Given the `root` of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

### Example

#### Input:
```plaintext
root = [3,9,20,null,null,15,7]
```

#### Visual Representation:
```plaintext
          3
         / \
        9   20
           /  \
          15   7
```

#### Output:
```plaintext
[[3],[9,20],[15,7]]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

#### Inputs:
- A `TreeNode` representing the root of the binary tree.

#### Outputs:
- A 2D list of integers, where each sublist contains the node values at a particular level of the tree.

#### Constraints:
- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`.

#### Assumptions:
- If the tree is empty (`root` is `None`), the output should be an empty list.

---

### 2. Match

- **Pattern**: Breadth-First Search (BFS).
- **Data Structure**: Use a queue (FIFO) to traverse the tree level by level.

---

### 3. Plan

1. **Edge Case**: If the root is `None`, return an empty list.
2. Initialize a queue and enqueue the root node.
3. Use a loop to traverse the tree level by level:
   - Determine the number of nodes at the current level (`n`).
   - Iterate `n` times to process all nodes at the current level.
   - For each node, dequeue it, add its value to the current level's list, and enqueue its children (if they exist).
4. Append the current level's list to the result.
5. Repeat until all nodes have been processed.
6. Return the result.

---

### 4. Implement

```python
from collections import deque
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # Step 1: Handle edge case
        if root is None:
            return []
        
        # Step 2: Initialize queue and result
        node_queue = deque([root])
        result = []

        # Step 3: Traverse level by level
        while node_queue:
            level = []
            n = len(node_queue)  # Number of nodes at the current level
            
            # Step 4: Process all nodes at the current level
            for _ in range(n):
                node = node_queue.popleft()  # Dequeue node
                level.append(node.val)  # Add node value to current level
                
                # Enqueue children if they exist
                if node.left:
                    node_queue.append(node.left)
                if node.right:
                    node_queue.append(node.right)
            
            # Step 5: Add current level to result
            result.append(level)
        
        # Step 6: Return the result
        return result
```

---

### 5. Review

#### Dry Run with Example Input

##### Example Tree:
```plaintext
          3
         / \
        9   20
           /  \
          15   7
```

##### Input:
```plaintext
root = [3,9,20,null,null,15,7]
```

##### Step-by-Step Execution:
1. Initialize `node_queue = deque([3])`, `result = []`.
2. Process level 1:
   - `n = 1`.
   - Dequeue `3`, enqueue `9` and `20`, add `3` to the current level.
   - `level = [3]`, `node_queue = deque([9, 20])`.
3. Process level 2:
   - `n = 2`.
   - Dequeue `9`, enqueue no children, add `9` to the current level.
   - Dequeue `20`, enqueue `15` and `7`, add `20` to the current level.
   - `level = [9, 20]`, `node_queue = deque([15, 7])`.
4. Process level 3:
   - `n = 2`.
   - Dequeue `15`, enqueue no children, add `15` to the current level.
   - Dequeue `7`, enqueue no children, add `7` to the current level.
   - `level = [15, 7]`, `node_queue = deque([])`.
5. Final result: `[[3], [9, 20], [15, 7]]`.

#### Dry Run with Complex Example Input

##### Example Tree:
```plaintext
          1
         / \
        2   3
       / \    \
      4   5    6
     /          \
    7            8
```

##### Input:
```plaintext
root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
```

##### Step-by-Step Execution:
1. Initialize `node_queue = deque([1])`, `result = []`.
2. Process level 1:
   - `n = 1`.
   - Dequeue `1`, enqueue `2` and `3`, add `1` to the current level.
   - `level = [1]`, `node_queue = deque([2, 3])`.
3. Process level 2:
   - `n = 2`.
   - Dequeue `2`, enqueue `4` and `5`, add `2` to the current level.
   - Dequeue `3`, enqueue no left child, enqueue `6`, add `3` to the current level.
   - `level = [2, 3]`, `node_queue = deque([4, 5, 6])`.
4. Process level 3:
   - `n = 3`.
   - Dequeue `4`, enqueue `7`, add `4` to the current level.
   - Dequeue `5`, enqueue no children, add `5` to the current level.
   - Dequeue `6`, enqueue `8`, add `6` to the current level.
   - `level = [4, 5, 6]`, `node_queue = deque([7, 8])`.
5. Process level 4:
   - `n = 2`.
   - Dequeue `7`, enqueue no children, add `7` to the current level.
   - Dequeue `8`, enqueue no children, add `8` to the current level.
   - `level = [7, 8]`, `node_queue = deque([])`.
6. Final result: `[[1], [2, 3], [4, 5, 6], [7, 8]]`.

---

### 6. Evaluate

#### Time Complexity:
- **O(n)**: Each node is visited exactly once.

#### Space Complexity:
- **O(m)**: Where `m` is the maximum number of nodes at any level (queue size).

---

### Additional Notes

### Why This Problem is Important
- **Industry Relevance**: BFS is a foundational algorithm in software engineering, frequently used in real-world problems like shortest path calculations and network traversal.
  
### Prerequisites for Practicing This Problem
- Understanding of binary trees.
- Familiarity with BFS and queue data structures.

### Follow-Up Practice Problems
1. [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
2. [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
3. [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
