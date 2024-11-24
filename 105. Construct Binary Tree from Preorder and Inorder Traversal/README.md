
# 105. Construct Binary Tree from Preorder and Inorder Traversal

### Problem Statement

Given two integer arrays `preorder` and `inorder` where:

- `preorder` is the preorder traversal of a binary tree.
- `inorder` is the inorder traversal of the same tree.

Construct and return the binary tree.

### Example

**Input:**
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

**Output:**  
A binary tree:  
```
    3
   / \
  9  20
     / \
    15  7
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. **Understand**

**Objective:** Reconstruct a binary tree given its preorder and inorder traversal arrays.

**Key Insights:**
- **Preorder traversal:** Root -> Left Subtree -> Right Subtree.
- **Inorder traversal:** Left Subtree -> Root -> Right Subtree.
- The first element of `preorder` is the root node.
- Using the root's position in `inorder`, we can divide the left and right subtrees.

**Inputs:**
- `preorder` (List[int]): Preorder traversal of the tree.
- `inorder` (List[int]): Inorder traversal of the tree.

**Outputs:**
- Root of the reconstructed binary tree.

**Constraints:**
- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- Values in `preorder` and `inorder` are unique.

### 2. **Match**

- **Pattern Recognized:** Tree Construction.
- **Technique:** Use recursion to build the tree. Divide the problem into smaller subproblems using the preorder and inorder traversal relationships.

---

### 3. **Plan**

**Steps:**
1. Identify the root node from the first element of `preorder`.
2. Find the root's index in `inorder` to divide the left and right subtrees.
3. Recursively repeat steps 1 and 2 for the left and right subtrees.
4. Base case: If `preorder` or `inorder` is empty, return `None`.

---

### 4. **Implement**

Here is the Python implementation:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # Base case: If no nodes remain in preorder or inorder, return None
        if not preorder or not inorder:
            return None

        # First element in preorder is the root
        root = TreeNode(preorder[0])

        # Find the root's index in inorder
        mid_index = inorder.index(preorder[0])

        # Recursively build the left subtree
        root.left = self.buildTree(preorder[1:mid_index + 1], inorder[:mid_index])

        # Recursively build the right subtree
        root.right = self.buildTree(preorder[mid_index + 1:], inorder[mid_index + 1:])

        # Return the constructed tree
        return root
```

**Explanation of Key Steps:**
1. **Root Creation:** The first element of `preorder` is the root of the tree.
2. **Split Subtrees:** Find the root's index in `inorder` to split into left and right subtrees.
3. **Recursive Construction:** Call the function recursively for the left and right subtrees.
4. **Base Case:** Stop recursion when either `preorder` or `inorder` is empty.

---

### 5. **Review**

**Testing with Edge Cases:**
1. **Basic Case:**
   Input:
   ```
   preorder = [1]
   inorder = [1]
   ```
   Output: Single node tree:
   ```
   1
   ```

2. **Unbalanced Tree:**
   Input:
   ```
   preorder = [1,2,3]
   inorder = [3,2,1]
   ```

   Output:
    ```
          1
         /
        2
       /
     3
    ```

**Dry Run with Input:**  
`preorder = [3,9,20,15,7]`, `inorder = [9,3,15,20,7]`.

- Root: 3 (from `preorder[0]`).
- Split using index of 3 in `inorder`:
  - Left subtree: `preorder[1:2], inorder[:1]` -> Root: 9.
  - Right subtree: `preorder[2:], inorder[2:]` -> Root: 20.
    - Left of 20: Root: 15.
    - Right of 20: Root: 7.

---

### 6. **Evaluate**

**Time Complexity:** O(n²).  
- Finding the root in `inorder` takes O(n) for each recursion step.  
- For `n` nodes, the total cost is O(n²).

**Space Complexity:** O(n).  
- Stack space for recursion.

---

### **Why This Problem is Important**

- **Industry Relevance:** Demonstrates tree construction, an essential skill in various systems like databases, parsers, and compiler design.
- **Prerequisites:** Understanding of binary trees, recursion, and traversal algorithms.

## **Follow-up Practice Problems**
- **106. Construct Binary Tree from Inorder and Postorder Traversal**
- **94. Binary Tree Inorder Traversal**
- **652. Find Duplicate Subtrees**
