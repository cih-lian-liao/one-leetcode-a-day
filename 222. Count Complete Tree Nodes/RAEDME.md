
# LeetCode 222: Count Complete Tree Nodes

### Problem Statement

Given the `root` of a **complete binary tree**, return the number of nodes in the tree.

In a **complete binary tree**:
- Every level, except possibly the last, is completely filled.
- All nodes in the last level are as far left as possible.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs**:  
- `root` - The root node of a complete binary tree.  

**Outputs**:  
- An integer representing the total number of nodes in the tree.

**Constraints**:  
- The number of nodes is in the range `[0, 5 * 10^4]`.  
- The tree is a **complete binary tree**.

**Assumptions**:  
- The input tree satisfies the "complete binary tree" property.

---

### 2. Match

This problem relates to **tree traversal** and **binary tree properties**:  
- A naive approach would involve traversing all nodes using DFS or BFS to count them (`O(n)` time complexity).  
- However, for a **complete binary tree**, we can optimize the solution using the following properties:  
   - If the left and right subtree heights are equal, the left subtree is a full binary tree.  
   - If not, the right subtree is a full binary tree.

**Patterns**:  
- Binary Tree traversal.  
- Optimization using the height of the binary tree.

**Data Structures**:  
- Recursion (DFS).  

---

### 3. Plan

We use recursion and leverage the **complete binary tree** property to avoid traversing every node.

**Steps**:  
1. **Define a helper function `getHeight(node)`**:  
   - Traverse the leftmost path of the tree to calculate the height of the tree.  

2. **Check the height of left and right subtrees**:  
   - If `left_height == right_height`:  
     - The left subtree is a **full binary tree**. Use the formula `2^h - 1` to count the left subtree nodes.  
     - Recursively calculate the number of nodes in the right subtree.  
   - If `left_height != right_height`:  
     - The right subtree is a **full binary tree**. Use the formula `2^h - 1` to count the right subtree nodes.  
     - Recursively calculate the number of nodes in the left subtree.  

3. Combine the counts:  
   - Total nodes = nodes in left/right subtree + root node.  

**Edge Cases**:  
- An empty tree (`root == None`).  
- A tree with only one node.

**Time Complexity**:  
- O((log n)^2): Each recursive step calculates the height in O(log n), and there are O(log n) recursive calls.  

**Space Complexity**:  
- O(log n): Recursive depth corresponds to the height of the tree.

---

### 4. Implement

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        # Helper function to calculate the height of the tree (leftmost path)
        def getHeight(node):
            height = 0
            while node:
                height += 1
                node = node.left  # Move left to measure the height
            return height
        
        if not root:  # Base case: If the tree is empty
            return 0
        
        # Calculate the height of left and right subtrees
        left_height = getHeight(root.left)
        right_height = getHeight(root.right)
        
        # Case 1: If left and right subtree heights are equal
        if left_height == right_height:
            # Left subtree is full: 2^left_height - 1 + 1 (root node) + right subtree count
            return (2 ** left_height) + self.countNodes(root.right)
        else:
            # Case 2: Right subtree is full
            # Right subtree nodes = 2^right_height - 1 + 1 (root node) + left subtree count
            return (2 ** right_height) + self.countNodes(root.left)
```

**Notes on Code**:  
- The `getHeight` function calculates the height of a subtree in O(log n).  
- The recursive function intelligently avoids unnecessary traversal by leveraging the tree's properties.

---

### 5. Review

**Testing the Code**:  

Test Case 1:  
```
Input:
        1
       / \
      2   3
     / \ / \
    4  5 6  7
Output: 7
```

Test Case 2:  
```
Input:
        1
       / \
      2   3
     / \
    4   5
Output: 5
```

Test Case 3:  
```
Input:
    1
Output: 1
```

Test Case 4:  
```
Input: []
Output: 0
```

**Dry Run Example**:  
For the input:
```
        1
       / \
      2   3
     / \
    4   5
```
1. Start at root `1`:  
   - `left_height = 2`, `right_height = 1` → Right subtree is full.  
   - Right subtree has `2^1 = 2` nodes.  
   - Recurse to left subtree (root = `2`).

2. At node `2`:  
   - `left_height = 1`, `right_height = 1` → Left subtree is full.  
   - Left subtree has `2^1 = 2` nodes.  
   - Recurse to right subtree (root = `5`).

3. At node `5`:  
   - Left and right are `None`, return 1.

---

### 6. Evaluate

**Time Complexity**:  
- Each `getHeight` call takes O(log n).  
- There are O(log n) recursive calls.  
- Overall time complexity = O((log n) * (log n)).

**Space Complexity**:  
- Recursive stack depth = O(log n).

---

### **Additional Notes**

**Why This Problem is Important**:  
- It teaches how to optimize recursion by leveraging binary tree properties.  
- It demonstrates the application of mathematical formulas for full binary trees.

**Prerequisites**:  
- Understanding binary tree traversal.  
- Recursion fundamentals.  
- Binary tree properties: full and complete binary trees.

**Industry Relevance**:  
- Used in building and optimizing binary tree-based data structures like heaps and segment trees.  

**Follow-up Practice Problems**:  
1. LeetCode 110: Balanced Binary Tree  
2. LeetCode 98: Validate Binary Search Tree  
3. LeetCode 111: Minimum Depth of Binary Tree  
4. LeetCode 104: Maximum Depth of Binary Tree  
