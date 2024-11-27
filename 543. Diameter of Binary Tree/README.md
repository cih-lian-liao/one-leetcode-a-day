# LeetCode 543: Diameter of Binary Tree

### Problem Statement

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

### Inputs
- `root`: The root node of the binary tree.

### Outputs
- An integer representing the diameter (number of edges in the longest path).

### Constraints
- The number of nodes in the tree is in the range `[1, 10^4]`.
- `-100 <= Node.val <= 100`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

To calculate the diameter of the binary tree:
1. At each node, compute the height of its left and right subtrees.
2. Update the diameter as the sum of the left and right subtree heights (representing the longest path through the current node).
3. Return the maximum diameter found across all nodes.

#### Key Observations:
- The diameter is determined by the heights of subtrees, not the total depth of the tree.
- Use a recursive function to compute both the height and update the diameter in one traversal.

#### Clarifications:
- A single-node tree has a diameter of `0`.

---

### 2. Match

#### Problem Type:
- Binary Tree Traversal
- Depth Calculation

#### Relevant Algorithm/Pattern:
- Depth-First Search (DFS) with recursion.
- Postorder traversal: compute left and right subtree heights first.

---

### 3. Plan

**Steps**:
1. Initialize a class variable `self.diameter` to `0` to store the maximum diameter.
2. Define a recursive function `height_and_max_diameter`:
   - Return `0` if the subtree is empty (base case).
   - Recursively calculate the height of the left and right subtrees.
   - Update `self.diameter` with the maximum of its current value and the sum of left and right subtree heights.
   - Return the height of the current node as `1 + max(left_height, right_height)`.
3. Call the recursive function on the root node.
4. Return `self.diameter`.

**Edge Cases**:
- Single node: The diameter is `0`.
- Skewed tree: Test both left-skewed and right-skewed trees.

**Time Complexity**: O(n)  
**Space Complexity**: O(h), where `h` is the height of the tree (recursion stack usage).

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
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        # Initialize diameter
        self.diameter = 0

        # Helper function to compute height and update diameter
        def height_and_max_diameter(root):
            if not root:  # Base case: empty subtree
                return 0

            # Recursive height calculation
            left_height = height_and_max_diameter(root.left)
            right_height = height_and_max_diameter(root.right)

            # Update diameter with the path passing through current node
            self.diameter = max(self.diameter, left_height + right_height)

            # Return height of current node
            return 1 + max(left_height, right_height)

        # Start the recursive function
        height_and_max_diameter(root)
        
        return self.diameter
```

---

### 5. Review

**Test Cases**:
1. **Single Node Tree**: `root = [1]`
   - Expected Output: `0`
2. **Complete Binary Tree**: `root = [1, 2, 3, 4, 5]`
   - Expected Output: `3`
3. **Skewed Tree (Left)**: `root = [1, 2, None, 3, None, 4]`
   - Expected Output: `3`
4. **Balanced Tree**: `root = [1, 2, 3, 4, 5, None, None]`
   - Expected Output: `4`

**Dry Run for Complex Case**:
Tree: `[1, 2, 3, 4, 5]`

```
      1
    /   \
   2     3
  / \
 4   5
```

1. Compute left height of `1` (subtree `[2, 4, 5]`):
   - Compute left height of `2` (subtree `[4]`): Height = `1`.
   - Compute right height of `2` (subtree `[5]`): Height = `1`.
   - Update diameter: `1 + 1 = 2`.

2. Compute right height of `1` (subtree `[3]`): Height = `1`.
3. Update diameter at `1`: `2 + 1 = 3`.

Final diameter: `3`.

---

### 6. Evaluate

#### Time Complexity:
- Each node is visited once.
- Total: **O(n)**.

#### Space Complexity:
- Recursion stack space is proportional to the tree height.
- Worst case (skewed tree): **O(n)**.
- Average case (balanced tree): **O(logn)**.

---

### Additional Notes

#### Why This Problem is Important:
- Helps understand tree traversal and postorder traversal patterns.
- Builds foundational knowledge for other tree problems involving height and depth.

#### Industry Relevance:
- Relevant for applications involving hierarchical data structures, such as file systems and decision trees.

#### Prerequisites:
- Recursive DFS
- Binary tree height calculation

#### Follow-up Practice Problems:
1. [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
2. [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
3. [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
