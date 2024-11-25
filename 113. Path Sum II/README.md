# LeetCode 113: Path Sum II

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

### Problem Description:
Given a binary tree and a target sum, find all root-to-leaf paths where each path's sum equals the target sum.

### Inputs:
- `root`: The root node of the binary tree.
- `targetSum`: An integer, the desired sum of the path.

### Outputs:
- A list of lists, where each inner list represents a root-to-leaf path that satisfies the target sum.

### Constraints:
1. The number of nodes in the tree is in the range `[0, 5000]`.
2. `-1000 <= Node.val <= 1000`.
3. `-1000 <= targetSum <= 1000`.

### Assumptions:
- A path must end at a leaf node.
- If no such path exists, return an empty list.

---

### 2. Match

### Problem Type:
- **Tree Traversal**: This involves traversing a binary tree to find specific paths.
- **Backtracking**: We need to explore all paths and backtrack when the sum doesn't match or when the node is a leaf.

### Suitable Algorithm:
- Use **Depth-First Search (DFS)** with **backtracking** to explore each potential path.

---

### 3. Plan

### Steps:
1. **Base Case**:
   - If the node is `None`, return immediately (no action needed).

2. **Add Current Node**:
   - Add the current node's value to the path being tracked.

3. **Check Leaf Node**:
   - If the current node is a leaf and the `remaining_sum` equals the node's value, save the current path in the result list.

4. **Recursive Exploration**:
   - Recursively explore the left and right subtrees with the updated `remaining_sum` (subtracting the current node's value).

5. **Backtrack**:
   - Remove the current node from the path after exploring its children to ensure the path remains valid for other branches.

### Pseudocode:
```
Function pathSum(root, targetSum):
    result = []
    dfs(root, targetSum, [], result)
    return result

Function dfs(node, remaining_sum, current_path, result):
    If node is None:
        Return
    Add node.val to current_path
    If node is a leaf and remaining_sum equals node.val:
        Append a copy of current_path to result
    Else:
        Recur on node.left with remaining_sum - node.val
        Recur on node.right with remaining_sum - node.val
    Remove node.val from current_path (backtrack)
```

### Time Complexity:
O(n^2) in the worst case when the tree is balanced, and we have to copy paths at each leaf node.

### Space Complexity:
O(h) for recursion depth, where `h` is the height of the tree.

---

### 4. Implement

### Code:
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from typing import Optional, List

class Solution:
    def dfs(self, node: Optional[TreeNode], remaining_sum: int, current_path: List[int], result: List[List[int]]) -> None:
        """
        Perform a DFS to find all root-to-leaf paths with the given sum.
        """
        if not node:
            return

        # Add current node to the path
        current_path.append(node.val)

        # Check if it's a leaf node and sum equals remaining_sum
        if not node.left and not node.right and remaining_sum == node.val:
            result.append(list(current_path))  # Add a copy of the path

        # Recur on left and right subtrees
        self.dfs(node.left, remaining_sum - node.val, current_path, result)
        self.dfs(node.right, remaining_sum - node.val, current_path, result)

        # Backtrack: remove the current node from the path
        current_path.pop()

    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        """
        Return all root-to-leaf paths where the sum equals targetSum.
        """
        result = []
        self.dfs(root, targetSum, [], result)
        return result
```

### Implementation Notes:
- **`dfs` Function**: Core recursive function to traverse the tree and build paths.
- **`current_path`**: Tracks the path from root to the current node.
- **Backtracking**: Ensures `current_path` is restored after exploring each branch.
- **Edge Case**: If `root` is `None`, `result` remains empty.

---

### 5. Review

### Dry Run Example:
#### Input:
Tree:
```
    5
   / \
  4   8
 /   / \
11  13  4
/ \      / \
7   2    5   1
```
`targetSum = 22`

#### Execution:
1. Start at root (5), add to path: `[5]`. Remaining sum: `17`.
2. Move to left child (4), add to path: `[5, 4]`. Remaining sum: `13`.
3. Move to left child (11), add to path: `[5, 4, 11]`. Remaining sum: `2`.
4. Move to left child (7), add to path: `[5, 4, 11, 7]`. Remaining sum: `-5`.
   - Backtrack (remove 7).
5. Move to right child (2), add to path: `[5, 4, 11, 2]`. Remaining sum: `0`.
   - Save path.
   - Backtrack (remove 2, 11, 4).
6. Explore right subtree of root (8).
   - Continue similarly for other paths.

#### Output:
`[[5, 4, 11, 2], [5, 8, 4, 5]]`

---

### 6. Evaluate

### Time Complexity:
- O(n^2) in the worst case (balanced tree with many leaf nodes).
- O(n) in the best case (linear tree with minimal branches).

### Space Complexity:
- O(h) for recursion depth.
- O(l * h) for storing `l` paths of maximum height `h`.

### Optimization:
- For read-only paths, a `yield` approach could reduce memory usage but is not compatible with this problem's requirements.

---

### Additional Notes

### Why This Problem Is Important:
- **Industry Relevance**: It involves recursion, tree traversal, and backtracking, which are common in real-world applications like decision trees.
- **Common Algorithms**: Depth-first search (DFS) is fundamental for solving problems involving trees and graphs.

### Prerequisites:
- Understanding binary trees, recursion, and backtracking.

### Follow-Up Problems:
1. LeetCode 112: Path Sum
2. LeetCode 437: Path Sum III
3. LeetCode 257: Binary Tree Paths
4. LeetCode 129: Sum Root to Leaf Numbers
