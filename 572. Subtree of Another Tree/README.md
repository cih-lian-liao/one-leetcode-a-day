# LeetCode 572: Subtree of Another Tree

### Problem Statement

Given two binary trees `root` and `subRoot`, determine if `subRoot` is a subtree of `root`. A subtree of a binary tree `root` is a tree consisting of a node in `root` and all of its descendants. The subtree must have the same structure and node values as `subRoot`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

- **Input**:
  - `root`: The root of the main binary tree.
  - `subRoot`: The root of the binary tree to check as a subtree.

- **Output**:
  - A boolean value (`True` or `False`) indicating if `subRoot` is a subtree of `root`.

- **Constraints**:
  - Both `root` and `subRoot` are binary trees.
  - Values are integers and may include negatives.
  - Node count: `1 <= root.size, subRoot.size <= 2000`.

- **Clarifications**:
  - A tree is a subtree of itself.
  - Handle edge cases like empty `root` or `subRoot`.

---

### 2. Match

- This problem involves **tree traversal** and **comparison**.
- Matches the pattern of **DFS** (Depth-First Search) or **BFS** (Breadth-First Search).
- Key subproblem: Determine if two trees are identical starting from their root nodes.

---

### 3. Plan

**Approach: DFS**

1. Define a helper function `isSameTree` to check if two trees are identical:
   - Both trees are null → return `True`.
   - One tree is null → return `False`.
   - Root values are different → return `False`.
   - Recursively check left and right subtrees.

2. Traverse the `root` tree using DFS:
   - For each node in `root`, check if `isSameTree` with `subRoot` returns `True`.
   - If yes, `subRoot` is a subtree of `root`.

3. Return `False` if no match is found after traversing the entire `root`.

**Edge Cases**:
- `root` is `None`: Always return `False`.
- `subRoot` is `None`: Always return `True` (an empty tree is a subtree of any tree).
- Large unbalanced trees.

**Time Complexity**:
- `O(n * m)`, where `n` is the size of `root` and `m` is the size of `subRoot`.

**Space Complexity**:
- `O(h)`, where `h` is the height of the tree (`root`).

---

### 4. Implement

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isSubtree(self, root, subRoot):
        # Helper function to check if two trees are identical
        def isSameTree(root1, root2):
            if not root1 and not root2:
                return True
            if not root1 or not root2 or root1.val != root2.val:
                return False
            return isSameTree(root1.left, root2.left) and isSameTree(root1.right, root2.right)
        
        # Traverse the main tree to find a matching subtree
        if not root:
            return False
        if isSameTree(root, subRoot):
            return True
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
```

#### **Implementation Notes**

1. **isSameTree**:
   - Checks if two trees are identical by comparing their root values and recursively their left and right subtrees.
   - If a mismatch occurs at any level, return `False`.

2. **isSubtree**:
   - Uses DFS to traverse the main tree (`root`).
   - Calls `isSameTree` at each node to check for a subtree match.

3. **Base Case**:
   - If `root` becomes `None`, it means we've traversed the entire tree, so return `False`.

---

### 5. Review

#### Dry Run with Example

**Example Input**:
```
root:
        3
       / \
      4   5
     / \
    1   2


subRoot:
      4
     / \
    1   2
```

**Step-by-Step Execution**:
1. Start at `root = 3`:
   - `isSameTree(3, 4)` → `False` (values differ).
2. Traverse left subtree (`root = 4`):
   - `isSameTree(4, 4)` → `True`.
   - Check left subtree: `isSameTree(1, 1)` → `True`.
   - Check right subtree: `isSameTree(2, 2)` → `True`.
   - Result: `True`.
3. Return `True` without further traversal.

**Edge Case Input**:
- `root = None`, `subRoot = TreeNode(1)` → Output: `False`.
- `root = TreeNode(1)`, `subRoot = None` → Output: `True`.

---

### 6. Evaluate

#### Time Complexity:
- `O(n * m)`:
  - For every node in `root` (`n`), `isSameTree` is called (`m` nodes in `subRoot`).

#### Space Complexity:
- `O(h)`:
  - Maximum depth of recursion is the height of the tree.

#### Optimizations:
- Use a **hashing technique** to store tree structures and compare hashes for faster subtree matching.
- Switch to BFS if tree height is too large to prevent stack overflow.

---

### Additional Notes

#### BFS Solution (For Reference)
```python
from collections import deque

class Solution:
    def isSubtree(self, root, subRoot):
        def isSameTree(root1, root2):
            if not root1 and not root2:
                return True
            if not root1 or not root2 or root1.val != root2.val:
                return False
            return isSameTree(root1.left, root2.left) and isSameTree(root1.right, root2.right)
        
        if not root:
            return False
        
        queue = deque([root])
        while queue:
            node = queue.popleft()
            if node.val == subRoot.val and isSameTree(node, subRoot):
                return True
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        return False
```

#### Explanation of BFS Solution

1. **Queue Initialization**:
   - A queue is initialized with the `root` node to enable level-order traversal (BFS).

2. **Iterative Tree Traversal**:
   - Nodes are processed level by level. For each node, its value is compared with the `subRoot` root value.

3. **Subtree Match Check**:
   - If the current node's value matches `subRoot`'s root value, the `isSameTree` function is invoked to verify if their structures are identical.

4. **Add Children to Queue**:
   - If no match is found, the current node's left and right children are added to the queue for further processing.

5. **Efficiency**:
   - BFS avoids recursive stack overflow in cases of large trees with significant depth.
   - It ensures every node is visited and checked systematically.

#### Time Complexity:
- **O(n * m)**:
  - For each node in `root` (`n`), a comparison of `subRoot` (`m` nodes) is performed if values match.

#### Space Complexity:
- **O(w)**:
  - `w` is the maximum width of the binary tree (number of nodes at the widest level). The queue stores all nodes at one level during traversal.

---

#### Why This Problem is Important:
- **Industry Relevance**:
  - Binary tree operations are frequently used in system design and coding interviews.
  - Concepts like tree traversal, recursion, and optimization are foundational in software engineering.

#### Prerequisites:
- Binary tree traversal (DFS and BFS).
- Recursion fundamentals.

#### Follow-up Problems:
1. LeetCode 100: Same Tree
2. LeetCode 101: Symmetric Tree
3. LeetCode 226: Invert Binary Tree
4. LeetCode 236: Lowest Common Ancestor of a Binary Tree
