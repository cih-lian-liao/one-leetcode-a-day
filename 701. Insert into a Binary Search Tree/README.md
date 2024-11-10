# Problem 701. Insert into a Binary Search Tree

### Problem Statement

Given the `root` of a binary search tree (BST) and a `val` to insert into the tree, insert the value into the BST. Return the root of the BST after insertion. 

It is **guaranteed** that the new value does not exist in the original BST.

### Input
- `root`: The root of the BST (TreeNode or `None` if the tree is empty).
- `val`: The integer value to insert into the BST.

### Output
- The root node of the BST after the insertion.

### Constraints
- The number of nodes in the tree will be in the range `[0, 10^4]`.
- `-10^8 <= Node.val <= 10^8`
- `-10^8 <= val <= 10^8`
  
---

#### UMPIRE Method Solution

### 1. Understand

- **Objective**: Insert a given integer into a Binary Search Tree (BST) while maintaining the BST properties.
- **BST Properties**:
  - For each node, the values in its left subtree are less than the node’s value.
  - The values in its right subtree are greater than the node’s value.
- **Ambiguity**: None, as we only need to insert the value without rebalancing the tree.
- **Edge Cases**:
  - If `root` is `None`, return a new node with `val` as its value.
  - If `val` is less than or greater than existing nodes, it should be inserted in the appropriate position.
  - Since `val` is guaranteed to be unique, we don't need to handle cases where `val` equals `root.val`.
---
### 2. Match

- **Pattern**: Binary Search Tree insertion using recursion.
- **Data Structure**: TreeNode class for each tree node.
---
### 3. Plan

1. **Base Case**:
   - If the `root` is `None`, create a new `TreeNode` with `val` and return it. This will handle both empty trees and the insertion point for `val`.
2. **Recursive Case**:
   - Compare `val` with the `root.val`.
     - If `val` is greater than `root.val`, recurse on the `right` subtree.
     - If `val` is less than `root.val`, recurse on the `left` subtree.
3. **Return the `root`**:
   - Once the insertion is complete, return the `root` node of the modified tree.

### Pseudocode
```plaintext
function insertIntoBST(root, val):
    if root is None:
        return new TreeNode(val)
    
    if val > root.val:
        root.right = insertIntoBST(root.right, val)
    else:
        root.left = insertIntoBST(root.left, val)
    
    return root
```

### Complexity Analysis

- **Time Complexity**: O(log n) on average for balanced trees; O(n) for skewed trees where all nodes lie on one side.
- **Space Complexity**: O(log n) on average due to recursion stack; O(n) in the worst case for a skewed tree.
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
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # Step 1: Base case
        if not root:
            return TreeNode(val)  # create and return a new node if root is None
        
        # Step 2: Recursive case
        if val > root.val:  # check if val should go in the right subtree
            root.right = self.insertIntoBST(root.right, val)  # insert into right subtree
        else:  # otherwise, insert into the left subtree
            root.left = self.insertIntoBST(root.left, val)
        
        # Step 3: Return the root of the modified BST
        return root
```

### Step-by-Step Explanation

1. **Check if `root` is `None`**:
   - If the tree is empty or we've reached a leaf node’s child, create a new `TreeNode` with `val`.
2. **Determine the Correct Subtree**:
   - If `val` is greater than `root.val`, we proceed to the right subtree.
   - If `val` is less than `root.val`, we proceed to the left subtree.
3. **Recursive Insertion**:
   - Recursively call `insertIntoBST` on the selected subtree and link the result to `root.right` or `root.left`.
4. **Return the Updated Root**:
   - Return `root` to allow previous recursion calls to build up the full BST structure.
---
### 5. Review

- **Edge Cases**:
  - Empty Tree: If `root` is initially `None`, the solution correctly creates and returns a new root.
  - Greater than all existing nodes: Properly inserts on the far right.
  - Less than all existing nodes: Properly inserts on the far left.
- **Test Cases**:
  - `insertIntoBST(None, 10)` should return a single node with value `10`.
  - `insertIntoBST(root, 5)` should insert `5` in the correct position based on `root.val`.
---
### 6. Evaluate

- **Time Complexity**: 
  - Average Case: O(log n)
  - Worst Case (skewed tree): O(n)
- **Space Complexity**: 
  - Average Case: O(log n) (recursive call stack)
  - Worst Case: O(n) (skewed tree call stack)

---

### Additional Information

### Iterative Solution

An iterative solution could involve a `while` loop to traverse the BST iteratively without using recursion. This may save space in cases of a deep tree structure.

### Iterative Solution Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # If the tree is empty, create a new TreeNode and return it as the root
        if not root:
            return TreeNode(val)

        # Start with the current node as the root
        current = root

        # Loop until we find the correct position to insert the new value
        while True:
            # If val is greater than current node's value, go to the right subtree
            if val > current.val:
                # If there is no right child, insert the new value here
                if not current.right:
                    current.right = TreeNode(val)
                    break
                else:
                    # Otherwise, move to the right child
                    current = current.right
            # If val is less than the current node's value, go to the left subtree
            else:
                # If there is no left child, insert the new value here
                if not current.left:
                    current.left = TreeNode(val)
                    break
                else:
                    # Otherwise, move to the left child
                    current = current.left

        # Return the original root of the tree after insertion
        return root
```

### Explanation

1. **Initialize**:
   - If `root` is `None`, we simply create and return a new `TreeNode` with `val`.

2. **Iterate to Find Insertion Point**:
   - Start at the `root` and traverse down the tree.
   - For each node, compare `val` with `current.val`.
     - If `val > current.val`, move to the `right` subtree.
     - If `val <= current.val`, move to the `left` subtree.
   - When an appropriate empty spot (`None`) is found in the left or right subtree, insert the new node there and exit the loop.

3. **Return Root**:
   - Return the original `root` of the modified tree.

3. **Summary**:  
   - This iterative approach avoids recursion, potentially saving stack space in cases of deep trees. It has the same time complexity as the recursive solution:
     - **Time Complexity**: O(log n) on average, O(n) in the worst case.
     -  **Space Complexity**: O(1) since we’re using a loop instead of recursion.

### Why This Problem is Important

BST insertion is fundamental for understanding tree-based data structures. Mastering insertion operations in BSTs is crucial for understanding more advanced data structures and algorithms, such as AVL and Red-Black Trees.

### Industry Relevance

BSTs are widely used in databases, search engines, and in-memory data structures due to their efficient insertion and search operations.

### Prerequisites for Practicing This Problem

- Understanding of tree structures, especially binary search trees.
- Familiarity with recursion.
- Knowledge of algorithm complexity.

### Follow-up Practice Problems

1. **LeetCode 450** - Delete Node in a BST
2. **LeetCode 700** - Search in a Binary Search Tree
3. **LeetCode 98** - Validate Binary Search Tree
4. **LeetCode 1008** - Construct Binary Search Tree from Preorder Traversal
