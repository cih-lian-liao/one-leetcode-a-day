
# LeetCode 450: Delete Node in a BST

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Problem Statement**  
Given the root of a binary search tree (BST) and a value `key`, delete the node with the value `key` in the BST. Return the root of the modified BST.

**Rules**:  
1. The tree must remain a valid BST after deletion.
2. If the node with the given value does not exist, return the original tree.

**Constraints**:  
- The number of nodes in the tree is `n`.
- `-10^4 <= Node.val, key <= 10^4`

**Key Observations**:
- A BST is defined by these properties:
  1. The left subtree contains values smaller than the root.
  2. The right subtree contains values greater than the root.
- The deletion process varies depending on the number of child nodes for the node to be deleted:
  1. **No children**: Simply remove the node.
  2. **One child**: Replace the node with its child.
  3. **Two children**: Replace the node's value with the smallest value in its right subtree, then delete that smallest node.

---

### 2. Match

**Patterns and Algorithms**:
- BST traversal using recursion.
- Handling subtrees during structural modifications.

**Chosen Approach**:
- Use recursion to traverse the tree to find the node.
- Handle the three deletion cases:
  - No child.
  - One child.
  - Two children (find the smallest value in the right subtree).

---

### 3. Plan

1. Traverse the BST recursively.
   - If `key > root.val`, move to the right subtree.
   - If `key < root.val`, move to the left subtree.
   - If `key == root.val`, process deletion.
2. Handle deletion based on the number of children:
   - **No children**: Return `None`.
   - **One child**: Return the non-`None` child.
   - **Two children**:
     1. Find the minimum value in the right subtree.
     2. Replace the current node's value with the minimum value.
     3. Delete the minimum value node in the right subtree.
3. Return the modified root.

**Time Complexity**:
- Average case: O(logn) (balanced tree).
- Worst case: O(n) (unbalanced tree).

**Space Complexity**:
- O(h), where `h` is the height of the tree (for recursion stack).

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
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            # Base case: Node not found
            return root

        if key > root.val:
            # Traverse to the right subtree
            root.right = self.deleteNode(root.right, key)
        elif key < root.val:
            # Traverse to the left subtree
            root.left = self.deleteNode(root.left, key)
        else:
            # Node to delete is found
            if not root.left:
                # Case 1: No left child
                return root.right
            elif not root.right:
                # Case 2: No right child
                return root.left
            
            # Case 3: Two children
            # Find the minimum value in the right subtree
            cur = root.right
            while cur.left:
                cur = cur.left
            # Replace root's value with that minimum value
            root.val = cur.val
            # Delete the minimum value node from the right subtree
            root.right = self.deleteNode(root.right, root.val)
        
        return root
```

---

### 5. Review

**Dry Run with Test Case**

Input:
```
root = [5, 3, 6, 2, 4, null, 7], key = 3
```
Tree before deletion:
```
       5
      / \
     3   6
    / \    \
   2   4    7
```

Step-by-Step Execution:
1. Start at root (5). `key < 5`, traverse to the left subtree (3).
2. At node (3). `key == 3`, process deletion:
   - Node (3) has two children.
   - Find the minimum in the right subtree (value 4).
   - Replace value of (3) with 4.
   - Delete node with value 4 from the right subtree.
3. Tree after deletion:
```
       5
      / \
     4   6
    /      \
   2        7
```

Output:
```
[5, 4, 6, 2, null, null, 7]
```

**Edge Cases**:
- `key` not in the tree.
- Deleting the root node.
- Tree with only one node.

---

### 6. Evaluate

**Time Complexity**:
- Average case: O(logn) for a balanced BST.
- Worst case: O(n) for a skewed BST.

**Space Complexity**:
- O(h), where `h` is the height of the tree.

---

### Additional Notes

### Why This Problem is Important
- Understanding BST deletion is a fundamental skill in data structures and algorithms.
- This problem highlights recursion, traversal, and structural tree modification.

### Industry Relevance
- Common in database indexing systems.
- Used in designing efficient data retrieval systems.

### Prerequisites for Practicing This Problem
1. Understanding binary search tree properties.
2. Familiarity with recursion.

### Follow-up Practice Problems
1. [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
2. [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
3. [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
