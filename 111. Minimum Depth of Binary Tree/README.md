# LeetCode Problem 111: Minimum Depth of Binary Tree

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

**Problem Statement**:  
Given a binary tree, find its minimum depth. The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Key Details**:
- A **leaf** is a node with no children.
- If the tree is empty (`root == null`), the depth is 0.

**Inputs**:
- A binary tree root node (`TreeNode`).

**Outputs**:
- An integer representing the minimum depth of the tree.

**Constraints**:
1. The number of nodes in the tree is in the range `[0, 10^5]`.
2. Node values are not specified but irrelevant for depth calculation.

**Assumptions**:
- The input is a valid binary tree.

---

### **2. Match**

**Problem Type**:
- Tree traversal problem.
- Closely related to breadth-first search (BFS) and depth-first search (DFS).

**Applicable Algorithms**:
- **BFS**: Optimal for finding the minimum depth as it explores layer by layer and stops at the first leaf.
- **DFS**: Can also calculate minimum depth, but requires traversing the entire tree.

**Data Structures**:
- Use a **queue** for BFS.
- Use **recursion** for DFS.

---

### **3. Plan**

**Approach**:
We will use the BFS approach to find the minimum depth efficiently.

**Steps**:
1. Handle the edge case where the root is `null` and return 0.
2. Initialize a queue to store nodes and their depths.
3. Perform a level-order traversal:
   - Dequeue a node and check if it is a leaf (both children are `null`). If yes, return its depth.
   - Otherwise, enqueue its non-null child nodes with their updated depths.
4. The first leaf encountered during the traversal gives the minimum depth.

**Edge Cases**:
- Empty tree (`root == null`).
- Tree with only one node.
- Tree with only left or right children.

**Time Complexity**: O(n)  
**Space Complexity**: O(w), where `w` is the maximum width of the tree.

---

### **4. Implement**

#### BFS Solution
```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def minDepth(root):
    """
    Function to calculate the minimum depth of a binary tree using BFS.
    """
    # Edge case: if the tree is empty
    if not root:
        return 0

    # Initialize queue for BFS with the root node and its depth
    queue = deque([(root, 1)])  # (node, depth)

    # BFS traversal
    while queue:
        node, depth = queue.popleft()

        # Check if the current node is a leaf node
        if not node.left and not node.right:
            return depth

        # Enqueue left child if it exists
        if node.left:
            queue.append((node.left, depth + 1))

        # Enqueue right child if it exists
        if node.right:
            queue.append((node.right, depth + 1))
```

**Implementation Notes**:
1. **Initialization**:
   - A queue is initialized with the root node and depth `1`.
   - Depth is updated incrementally during traversal.

2. **Leaf Node Check**:
   - The traversal stops at the first leaf node encountered, ensuring minimal computation.

3. **Enqueue Children**:
   - Only non-null children are enqueued to ensure efficient memory use.

---

### **5. Review**

**Dry Run**:

#### Example Input:
```python
# Tree structure:
#       3
#      / \
#     9   20
#        /  \
#       15   7
root = TreeNode(3)
root.left = TreeNode(9)
root.right = TreeNode(20)
root.right.left = TreeNode(15)
root.right.right = TreeNode(7)

print(minDepth(root))  # Expected Output: 2
```

#### Dry Run Steps:
1. Initialize queue: `[(3, 1)]`.
2. Dequeue `(3, 1)`. Node `3` is not a leaf. Enqueue `(9, 2)` and `(20, 2)`.
3. Dequeue `(9, 2)`. Node `9` is a leaf. **Return depth: 2**.

---

### **6. Evaluate**

**Time Complexity**:
- BFS ensures that each node is visited only once, so the complexity is **O(n)**, where `n` is the number of nodes in the tree.

**Space Complexity**:
- The queue stores nodes for each level, so the space complexity is **O(w)**, where `w` is the maximum width of the tree.

**Optimizations**:
- BFS is inherently optimal for this problem as it stops traversal as soon as the first leaf is found.

---

### **Additional Notes**

#### **DFS Solution**

**Approach**:
The depth-first search (DFS) approach calculates the minimum depth by recursively traversing the tree. The key is to ensure proper handling of cases where a node has only one child.

**Steps**:
1. Handle the base case where the root is `null` (return depth 0).
2. Recursively calculate the minimum depth of the left and right subtrees.
3. If one of the subtrees is missing (`null`), return the depth of the non-null subtree plus 1.
4. If both subtrees exist, return the smaller depth between the two subtrees plus 1.

**Implementation**:
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def minDepthDFS(root):
    """
    Function to calculate the minimum depth of a binary tree using DFS.
    """
    # Base case: if the tree is empty
    if not root:
        return 0

    # If one of the subtrees is missing
    if not root.left:
        return 1 + minDepthDFS(root.right)
    if not root.right:
        return 1 + minDepthDFS(root.left)

    # If both subtrees are present, take the minimum depth
    return 1 + min(minDepthDFS(root.left), minDepthDFS(root.right))
```

**Dry Run**:

#### Example Input:
```python
# Tree structure:
#       3
#      / \
#     9   20
#        /  \
#       15   7
root = TreeNode(3)
root.left = TreeNode(9)
root.right = TreeNode(20)
root.right.left = TreeNode(15)
root.right.right = TreeNode(7)

print(minDepthDFS(root))  # Expected Output: 2
```

#### Dry Run Steps:
1. Call `minDepthDFS(3)`.
   - Call `minDepthDFS(9)` → `9` is a leaf, return 1.
   - Call `minDepthDFS(20)`:
     - Call `minDepthDFS(15)` → `15` is a leaf, return 1.
     - Call `minDepthDFS(7)` → `7` is a leaf, return 1.
     - Return `1 + min(1, 1) = 2`.
2. Return `1 + min(1, 2) = 2`.

**Time Complexity**:
- Each node is visited once, so the complexity is **O(n)**.

**Space Complexity**:
- The recursive stack depth depends on the height of the tree. In the worst case (a skewed tree), the space complexity is **O(h)**, where `h` is the height of the tree.

**When to Use DFS**:
- Use DFS if you are required to traverse the entire tree, or if BFS is not feasible due to memory constraints (e.g., when the tree's width is very large).

---

**Why This Problem Is Important**:
- **Industry Relevance**: Tree traversal problems are common in technical interviews. This problem tests understanding of BFS/DFS and handling edge cases in tree structures.

**Prerequisites for Practicing**:
- Basic understanding of binary trees.
- Knowledge of BFS and DFS traversal techniques.
- Familiarity with queue data structure for BFS.

**Follow-up Practice Problems**:
1. [112. Path Sum](https://leetcode.com/problems/path-sum/)
2. [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
3. [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
