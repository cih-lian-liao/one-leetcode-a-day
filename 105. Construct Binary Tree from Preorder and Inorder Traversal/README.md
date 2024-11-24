# 105. Construct Binary Tree from Preorder and Inorder Traversal

### Problem Statement

Given two integer arrays `preorder` and `inorder` where:

- `preorder` is the preorder traversal of a binary tree.
- `inorder` is the inorder traversal of the same tree.

Construct and return the binary tree.

### Example
**Input:**
```
preorder = [3, 9, 20, 15, 7]
inorder = [9, 3, 15, 20, 7]
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
- The first element of `preorder` is always the root node.
- Using the root's position in `inorder`, we can separate the left and right subtrees.

**Inputs:**
- `preorder` (List[int]): Preorder traversal of the tree.
- `inorder` (List[int]): Inorder traversal of the tree.

**Outputs:**
- Root of the reconstructed binary tree.

**Constraints:**
- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- Values in `preorder` and `inorder` are unique.

---

### 2. **Match**

- **Pattern Recognized:** Binary Tree Construction.
- **Relevant Techniques:** Divide and Conquer, Recursion.
- **Data Structures Used:** 
  - HashMap for efficient lookup of `inorder` indices.
  - Recursive helper function to build the tree.

---

### 3. **Plan**

**Steps to Solve:**
1. Use a HashMap to store the indices of `inorder` elements for O(1) lookup.
2. Create a recursive function `helper(pre_start, pre_end, in_start, in_end)`:
   - Base Case: If `pre_start > pre_end`, return `None`.
   - Root Node: The root value is `preorder[pre_start]`.
   - Find the root's position in `inorder` using the HashMap.
   - Calculate the size of the left subtree.
   - Recursively build the left subtree using the next elements in `preorder` and `inorder`.
   - Recursively build the right subtree using the remaining elements.
   - Return the root node.
3. Call the recursive function for the entire range of `preorder` and `inorder`.

**Pseudocode:**
1. Create a HashMap `inorder_index_map` for efficient lookup of `inorder` indices.
2. Define the recursive function:
   - Compute the root value and create a new TreeNode.
   - Find the index of the root in `inorder`.
   - Divide `inorder` into left and right subtrees.
   - Recursively construct the left and right subtrees.
3. Return the root node of the tree.

**Complexity Analysis:**
- **Time Complexity:** O(n) (traversing `preorder` and `inorder`, and using HashMap for O(1) lookups).
- **Space Complexity:** O(n) (due to recursion stack and HashMap).

---

### 4. **Implement**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    # Create a hashmap to store value->index mapping for inorder
    inorder_index_map = {val: idx for idx, val in enumerate(inorder)}

    # Helper function to construct the tree
    def helper(preorder_start, preorder_end, inorder_start, inorder_end):
        if preorder_start > preorder_end:
            return None
        
        # Root value is the first element in preorder
        root_val = preorder[preorder_start]
        root = TreeNode(root_val)
        
        # Find root index in inorder
        root_index_inorder = inorder_index_map[root_val]
        
        # Calculate sizes of left and right subtrees
        left_tree_size = root_index_inorder - inorder_start
        
        # Recursively build left and right subtrees
        root.left = helper(preorder_start + 1, preorder_start + left_tree_size,
                           inorder_start, root_index_inorder - 1)
        root.right = helper(preorder_start + left_tree_size + 1, preorder_end,
                            root_index_inorder + 1, inorder_end)
        return root
    
    # Call helper for the entire range
    return helper(0, len(preorder) - 1, 0, len(inorder) - 1)
```

---

### 5. **Review**

#### Dry Run Example:
**Input:**  
```
preorder = [3, 9, 20, 15, 7]
inorder = [9, 3, 15, 20, 7]
```

**Step-by-Step Process:**
1. Root: `3` (preorder[0]).  
   - Index in `inorder`: `1`.  
   - Left subtree: `inorder[0:1] = [9]`.  
   - Right subtree: `inorder[2:] = [15, 20, 7]`.

2. Left Subtree of `3`:  
   - Root: `9`.  
   - Index in `inorder`: `0`.  
   - No left or right children.

3. Right Subtree of `3`:  
   - Root: `20`.  
   - Index in `inorder`: `3`.  
   - Left subtree: `inorder[2:3] = [15]`.  
   - Right subtree: `inorder[4:] = [7]`.

4. Continue recursively to construct left and right subtrees of `20`.

---

### 6. **Evaluate**

**Time Complexity:** O(n).  
- Constructing the tree involves traversing `preorder` and `inorder` once.  
- Lookup in the HashMap is O(1).

**Space Complexity:** O(n).  
- HashMap storage for `inorder` indices: O(n).  
- Recursion stack in the worst case (skewed tree): O(n).

---

### **Why This Problem is Important**

- **Industry Relevance:** This problem demonstrates binary tree construction, a crucial concept in file systems, database indexing, and compiler design.
- **Prerequisites:** Binary trees, recursion, and traversal algorithms.

- **Follow-up Practice Problems:**  
  -> 106. Construct Binary Tree from Inorder and Postorder Traversal
  
  -> 94. Binary Tree Inorder Traversal
  
  -> 144. Binary Tree Preorder Traversal
  
