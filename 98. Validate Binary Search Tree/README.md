

# LeetCode Problem 98: Validate Binary Search Tree

### Problem Description
Determine if a binary tree is a valid binary search tree (BST). A BST is defined as follows:
1. The left subtree of a node contains only nodes with values **less than** the node's value.
2. The right subtree of a node contains only nodes with values **greater than** the node's value.
3. Both the left and right subtrees must also be binary search trees.


#### [UMPIRE Method Solution]

#### 1. **Understand**

- **Problem Statement**: Check if a binary tree is a valid BST by ensuring that each node follows BST properties.
- **Input**: `TreeNode root` - the root node of a binary tree.
- **Output**: `Boolean` - `true` if the tree is a valid BST; `false` otherwise.
- **Constraints**: 
  - The number of nodes in the tree is in the range `[1, 10^4]`.
  - `-2^31 <= Node.val <= 2^31 - 1`.
- **Assumptions**:
  - All node values are unique, so no duplicate nodes.
  - A single-node tree is a valid BST by default.

---

#### 2. **Match**

- **Problem Pattern**: This problem aligns with tree traversal and validation techniques.
- **Potential Solutions**:
  - **Recursive Approach**: Traverse the tree recursively, maintaining valid range constraints (min and max values) to ensure all nodes meet BST properties.
  - **Inorder Traversal Approach**: (In additional notes) Perform an inorder traversal and check if values appear in ascending order.

---

#### 3. **Plan** (Recursive Solution)

The recursive solution involves setting **range constraints** for each node as we traverse down the tree:
1. Each node must lie within a specific range.
   - For the left child: `node.val` should be **greater than** `min` and **less than** `node.val`.
   - For the right child: `node.val` should be **greater than** `node.val` and **less than** `max`.
2. We will create a helper function that recursively checks if each node falls within the valid range:
   - Set initial `min` to negative infinity (`-inf`) and `max` to positive infinity (`inf`).
   - Recursively check each node’s value against these constraints.
3. If any node violates its constraint, return `false`. If all nodes are within range, return `true`.

- **Edge Cases**:
  - Single-node tree: This is a valid BST.
  - Subtrees with only left or right children: Ensure all nodes adhere to BST properties.
  - Nodes with very large or small integer values to confirm boundary handling.

- **Complexity**:
  - **Time Complexity**: O(n), since we visit each node once.
  - **Space Complexity**: O(n), due to the recursion stack in a worst-case scenario. (Or we can say O(h). "h" means height of tree.)

---

#### 4. **Implement**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # Helper function for recursive validation with range constraints
        def validate(node, min_val, max_val):
            # Base case: if the node is None, it's valid by default
            if not node:
                return True
            
            # Check if the current node value violates the BST properties
            if node.val <= min_val or node.val >= max_val:
                return False
            
            # Recursively validate the left subtree and right subtree
            return (validate(node.left, min_val, node.val) and
                    validate(node.right, node.val, max_val))
        
        # Initial call to validate with min as -inf and max as inf
        return validate(root, float('-inf'), float('inf'))
```

---

#### 5. **Review**

- **Debugging**:
  - Test with various inputs, including edge cases (e.g., single-node trees, left-skewed or right-skewed trees, and large balanced trees).
- **Example Tests**:
  - Example 1: `[2, 1, 3]` - Output: `true`
  - Example 2: `[5, 1, 4, null, null, 3, 6]` - Output: `false` (since `3` is in the wrong place in the BST structure)
- **Edge Cases**:
  - A tree with only one node should return `true`.
  - Ensure handling of minimum and maximum integer boundaries.

---

#### 6. **Evaluate**

- **Complexity Analysis**:
  - **Time Complexity**: O(n) - each node is visited once.
  - **Space Complexity**: O(n) - recursion call stack in the worst case (skewed tree).

- **Optimizations**:
  - The recursive approach is efficient, using min and max constraints to validate BST properties.

---

### Additional Notes: Inorder Traversal Solution

In an inorder traversal of a BST, node values should appear in strictly ascending order. This solution leverages this property:

1. Use an inorder traversal to visit nodes in ascending order.
2. Track the **previous node’s value** during traversal.
3. If the current node's value is **not greater than** the previous node’s value, return `false`.
4. If all nodes satisfy the order, return `true`.

#### Code Implementation (Inorder Traversal Solution)

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        self.prev = None

        def inorder(node: TreeNode) -> bool:
            if not node:
                return True
            
            if not inorder(node.left):
                return False
            
            if self.prev is not None and node.val <= self.prev:
                return False
            self.prev = node.val
            
            return inorder(node.right)
        
        return inorder(root)
```

- **Explanation**:
  - This method naturally verifies BST properties by confirming that values appear in ascending order during traversal.
  
---
### Comparison Between Recursive Solution and Inorder Traversal Solution

1. **Approach and Intuition**:
   - **Recursive Solution**: 
     - This approach explicitly enforces BST properties by **setting min and max constraints** at each node.
     - Each recursive call passes a specific range that a node’s value must satisfy. For example, the left child must be less than the current node, and the right child must be greater.
     - This method is direct and logical, as it mirrors the definition of a BST—ensuring every node is correctly positioned relative to its parent nodes.
   
   - **Inorder Traversal Solution**:
     - In this approach, we rely on the **ascending order property** of BSTs during an inorder traversal.
     - By visiting nodes in an inorder fashion (left-root-right), BST values should appear in strictly increasing order.
     - Instead of setting constraints, this method checks if each node value is greater than the last visited node value (`prev`), thus indirectly validating BST properties.

2. **Code Complexity and Readability**:
   - **Recursive Solution**:
     - The code is often viewed as straightforward because it directly applies min and max constraints, making the conditions for validity explicit.
     - However, since it involves passing constraints (min and max values) down each recursive call, it might be slightly more challenging for beginners to track the purpose of each constraint.
   - **Inorder Traversal Solution**:
     - This solution is relatively clean and concise because it doesn’t require additional parameters (like min and max constraints).
     - By maintaining a `prev` variable, the inorder traversal simplifies tracking and verifying the order, making it easier to read.
     - For those familiar with inorder traversal properties of BSTs, this approach can feel more intuitive, as it simply checks the natural order of elements.

3. **Efficiency and Performance**:
   - **Time Complexity**:
     - Both approaches have a **time complexity of O(n)**, as they require visiting each node once.
   - **Space Complexity**:
     - **Recursive Solution**: O(n) in the worst case (skewed trees), as the recursion stack might need to hold up to `n` nodes in memory.
     - **Inorder Traversal Solution**: O(n) as well, due to the recursion stack depth for a skewed tree.
   - Both solutions are efficient for BST validation but use different mechanisms to ensure correctness.

4. **Suitability for Different Tree Structures**:
   - **Recursive Solution**:
     - This approach is more versatile because it checks each node based on absolute constraints.
     - For highly unbalanced trees or when explicit constraint checking is required, the recursive method with min/max constraints is often more robust.
   - **Inorder Traversal Solution**:
     - This approach shines with balanced trees, as it maintains an ordered sequence throughout traversal.
     - In contexts where maintaining a simple order is sufficient, the inorder traversal provides a clear and effective way to validate BST properties.

5. **When to Use Which Solution**:
   - **Recursive Solution**:
     - Ideal for interviews where the problem description explicitly asks for range constraints at each node.
     - Suitable when you need to keep track of boundary values, such as in problems where constraints on node values might change dynamically.
   - **Inorder Traversal Solution**:
     - Preferred for problems that emphasize order checking rather than constraint enforcement.
     - Generally considered simpler in interview settings, as it directly uses the BST’s inorder properties, reducing the mental overhead of tracking constraints.
   - Both solutions offer valuable ways to validate a BST, each with distinct strengths. Choosing one often depends on the specific constraints of the problem and personal preference. 
  
---

### Importance of Problem 98

- **Industry Relevance**:
  - Validating tree structures is crucial in backend and database management systems.
  - BST validation is a foundational skill in software engineering, relevant in scenarios involving hierarchical data and search optimizations.

### Prerequisites for Practicing

- Understanding of binary trees and tree traversal techniques.
- Familiarity with recursion and managing constraints.

### Follow-Up Practice Problems

1. **Problem 230. Kth Smallest Element in a BST** - Builds on inorder traversal.
2. **Problem 450. Delete Node in a BST** - Expands BST manipulation skills.
3. **Problem 235. Lowest Common Ancestor of a Binary Search Tree** - Applies BST properties for efficient solution.
