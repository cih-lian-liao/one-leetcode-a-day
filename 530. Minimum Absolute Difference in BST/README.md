# LeetCode 530: Minimum Absolute Difference in BST

### Problem Statement

Given the root of a Binary Search Tree (BST), return the **minimum absolute difference** between the values of any two different nodes in the tree.

**Example:**

```
Input:
    4
   / \
  2   6
 / \
1   3

Output: 1
Explanation:
The minimum absolute difference is between 2 and 1 or 3 and 2, which is 1.
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

- **Problem Goal**:  
  Find the **minimum absolute difference** between the values of any two nodes in a BST.

- **Input**:  
  A binary search tree's root node.

- **Output**:  
  An integer representing the minimum absolute difference.

- **Constraints**:  
  - The number of nodes in the tree is between 2 and 10^4.
  - Each node's value is unique.

- **Clarification**:  
  - A **BST** follows the rule: `left child < root < right child`.
  - Two nodes must exist, so we don't need to handle single-node trees.

---

### 2. Match

- The problem can be solved with an **inorder traversal** because:
  - Inorder traversal of a BST yields values in **sorted order**.
  - The minimum absolute difference will be between **adjacent nodes** in this sorted order.

- **Known pattern**:  
  - Binary tree traversal (Inorder).
  - Maintain a variable for the previous node value to compare differences.

- **Data structure**:  
  - Recursion or Stack (for inorder traversal).

---

### 3. Plan

1. Perform an **inorder traversal** of the BST.  
   Inorder traversal will give the nodes in **sorted order**.

2. Use a variable `prev` to store the value of the previously visited node.

3. At each node:  
   - Calculate the difference between the current node's value and `prev`.  
   - Update the minimum difference if this value is smaller.

4. Continue the traversal until all nodes are visited.

5. Return the minimum absolute difference.

**Pseudocode**:

```
- Initialize `prev` as None, `min_diff` as infinity.
- Define a recursive function `inorder(node)`:
    - If node is None, return.
    - Recursively traverse left subtree.
    - If `prev` is not None:
        - Update `min_diff` with the difference between node.val and prev.
    - Update `prev` to node.val.
    - Recursively traverse right subtree.
- Call `inorder` with root.
- Return `min_diff`.
```

---

### 4. Implement

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        # Initialize variables
        self.prev = None  # To store previous node value
        self.min_diff = float('inf')  # To track the minimum absolute difference
        
        # Inorder traversal function
        def inorder(node):
            if not node:  # Base case: If node is None, return
                return
            
            # Traverse left subtree
            inorder(node.left)
            
            # Calculate minimum difference with previous node
            if self.prev is not None:
                diff = abs(node.val - self.prev)
                self.min_diff = min(self.min_diff, diff)
            
            # Update prev to current node's value
            self.prev = node.val
            
            # Traverse right subtree
            inorder(node.right)
        
        # Start the inorder traversal
        inorder(root)
        
        return self.min_diff
```

**Notes for Implementation**:
1. **Initialize Variables**:
   - `self.prev` is `None` initially since no node has been visited.
   - `self.min_diff` starts as `infinity` because we want to minimize this value.

2. **Inorder Traversal**:
   - This function processes nodes in sorted order.
   - The difference is calculated only if `self.prev` is not `None`.

3. **Comparison Logic**:
   - Update `self.min_diff` with `abs(node.val - self.prev)`.

---

### 5. Review

#### Dry Run

**Example Input:**

```
    4
   / \
  2   6
 / \
1   3
```

**Steps**:
1. Start `inorder` traversal at root (4). Move to the left subtree.
2. Visit node `1`:
   - `prev = None`, no difference calculation.
   - Update `prev = 1`.
3. Visit node `2`:
   - `diff = abs(2 - 1) = 1`.
   - Update `min_diff = 1`.
   - Update `prev = 2`.
4. Visit node `3`:
   - `diff = abs(3 - 2) = 1`.
   - `min_diff` remains `1`.
   - Update `prev = 3`.
5. Visit root `4`:
   - `diff = abs(4 - 3) = 1`.
   - `min_diff` remains `1`.
   - Update `prev = 4`.
6. Visit node `6`:
   - `diff = abs(6 - 4) = 2`.
   - `min_diff` remains `1`.

**Output**: `1`

---

### 6. Evaluate

**Time Complexity**:
- Inorder traversal visits each node **once**.  
  **O(n)** where `n` is the number of nodes.

**Space Complexity**:
- **O(h)** for recursion stack, where `h` is the height of the tree.  
  In a balanced BST, `h = log n`. In the worst case (skewed tree), `h = n`.

---

### Additional Notes

#### Why This Problem is Important
- Tests your understanding of **BST traversal** and how to efficiently find relationships between nodes.

#### Prerequisites for Practicing This Problem
- Binary Search Tree properties.
- Inorder traversal (recursive or iterative).

#### Industry Relevance
- Common interview question to test tree traversal techniques.
- Concepts applied to problems requiring sorted relationships.

#### Follow-up Practice Problems
1. LeetCode 783: Minimum Distance Between BST Nodes
2. LeetCode 94: Binary Tree Inorder Traversal
3. LeetCode 230: Kth Smallest Element in a BST
4. LeetCode 938: Range Sum of BST
