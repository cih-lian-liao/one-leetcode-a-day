
# LeetCode 110: Balanced Binary Tree

### Problem Statement

Determine if a binary tree is height-balanced. A binary tree is height-balanced if the absolute difference between the heights of the left and right subtrees of every node is at most 1.

### Inputs
- `root`: The root node of the binary tree. 

### Outputs
- `True` if the binary tree is balanced.
- `False` otherwise.

### Constraints
- The number of nodes in the tree is in the range `[0, 5000]`.
- `-10^4 <= Node.val <= 10^4`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

The problem involves checking whether a binary tree is balanced in terms of height. To do this, we:
- Compute the height of the left and right subtrees for each node.
- Check if the absolute difference between the left and right subtree heights is greater than 1. If yes, the tree is not balanced.

#### Key Observations:
- The height of an empty tree is `0`.
- The height of a tree with only one node is `1`.
- Use recursion to calculate heights efficiently.

#### Clarifications:
- An empty tree is considered balanced.

---

### 2. Match

#### Problem Type:
- Binary Tree Traversal
- Depth Calculation

#### Relevant Algorithm/Pattern:
- Depth-First Search (DFS) with recursion.
- Postorder traversal: compute left and right subtree heights before using them.

---

### 3. Plan

**Steps**:
1. Define a recursive function `height_check` that calculates the height of a subtree.
2. In `height_check`:
   - Return `0` for an empty subtree (base case).
   - Recursively calculate the heights of the left and right subtrees.
   - If the absolute difference between the left and right subtree heights is greater than 1, mark the tree as unbalanced and stop further checks.
   - Return the height of the current subtree as `1 + max(left_height, right_height)`.
3. Use a class variable `self.balanced` to keep track of the balance state.
4. Return the value of `self.balanced` after performing `height_check` on the root.

**Edge Cases**:
- Empty tree: Return `True`.
- A single-node tree: Always balanced.

**Time Complexity**: O(n)  
**Space Complexity**: O(h), where `h` is the height of the tree (call stack usage for recursion).

---

### 4. Implement

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        # Initialize balanced state
        self.balanced = True

        # Helper function to compute height
        def height_check(root):
            if not root:  # Base case: empty tree
                return 0

            # Recursive calculation for left and right subtrees
            left_height = height_check(root.left)
            # Early return if imbalance detected
            if not self.balanced:
                return 0
            right_height = height_check(root.right)
            # Check balance condition
            if abs(left_height - right_height) > 1:
                self.balanced = False
            
            # Return height of current node
            return 1 + max(left_height, right_height)

        # Perform the height check
        height_check(root)
        return self.balanced
```

---

### 5. Review

**Test Cases**:
1. **Empty Tree**: `root = None`
   - Expected Output: `True`
2. **Single Node Tree**: `root = [1]`
   - Expected Output: `True`
3. **Balanced Tree**: `root = [3, 9, 20, None, None, 15, 7]`
   - Expected Output: `True`
4. **Unbalanced Tree**: `root = [1, 2, 2, 3, 3, None, None, 4, 4]`
   - Expected Output: `False`

**Dry Run for Complex Case**:
Tree: `[1, 2, 2, 3, 3, None, None, 4, 4]`
- Start from root `1`, compute left and right subtree heights recursively.
- At node `3`, the left subtree has height `2`, right subtree has height `0`. Imbalance detected. Set `self.balanced = False`.

---

### 6. Evaluate

#### Time Complexity:
- Each node is visited once.
- Total: **O(n)**

#### Space Complexity:
- Recursion stack space is proportional to the tree height.
- Worst case (skewed tree): **O(n)**.
- Average case (balanced tree): **O(logn)**.

---

### Additional Notes

#### Why This Problem is Important:
- Balancing binary trees is a fundamental concept in tree-based data structures like AVL trees.
- Tests recursive DFS and tree traversal understanding.

#### Industry Relevance:
- Essential for back-end systems (e.g., databases, file systems) that rely on balanced binary trees for efficiency.

#### Prerequisites:
- Understanding of recursion.
- Binary tree traversal techniques.

#### Follow-up Practice Problems:
1. [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
2. [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
3. [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

