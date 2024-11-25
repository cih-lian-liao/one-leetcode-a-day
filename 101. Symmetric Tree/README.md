# 101. Symmetric Tree

### Problem Description

Given the root of a binary tree, check whether it is a **mirror of itself** (i.e., symmetric around its center).

#### Example 1

**Input:**
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

**Output:** `true`


#### Example 2

**Input:**
```
    1
   / \
  2   2
   \    \
   3     3
```

**Output:** `false`

### Constraints
- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand
- **Inputs:** 
  - `root`: The root node of the binary tree.
- **Outputs:**
  - Return `true` if the tree is symmetric, `false` otherwise.
- **Constraints:**
  - Must check for symmetry in structure and values.
  - A tree with only one node is symmetric.
- **Assumptions:**
  - Null nodes are considered symmetric.

---

### 2. Match
- This problem involves **tree traversal** and **mirroring comparison**:
  - Use recursion (DFS) or iteration (BFS).
  - Check if two subtrees are mirrors of each other.

---

### 3. Plan

#### DFS Solution
1. Base cases:
   - If both nodes are `None`, they are symmetric.
   - If only one node is `None`, they are not symmetric.
   - If values of the two nodes are not equal, they are not symmetric.
2. Recursively compare:
   - The left child of one subtree with the right child of the other.
   - The right child of one subtree with the left child of the other.

#### BFS Solution
1. Use a queue to store pairs of nodes to compare.
2. While the queue is not empty:
   - Dequeue a pair of nodes.
   - If both are `None`, continue.
   - If only one is `None`, return `false`.
   - If values of the two nodes are not equal, return `false`.
   - Enqueue the left and right children of the nodes in the correct order for comparison.

#### Edge Cases
- Single node tree.
- Tree with only left or right child.
- Empty tree (`None`).

---

### 4. Implement

### DFS Solution
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def isMirror(t1: TreeNode, t2: TreeNode) -> bool:
            if not t1 and not t2:
                return True
            if not t1 or not t2:
                return False
            return (t1.val == t2.val and 
                    isMirror(t1.left, t2.right) and 
                    isMirror(t1.right, t2.left))
        
        return isMirror(root, root)
```

**Step-by-Step Explanation:**
1. `isMirror` is a helper function that recursively checks for mirroring.
2. Base cases:
   - If both `t1` and `t2` are `None`, return `True`.
   - If one is `None`, return `False`.
   - If `t1.val` != `t2.val`, return `False`.
3. Recursively compare the left and right subtrees in mirrored positions.

---

### BFS Solution
```python
from collections import deque

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        
        queue = deque([(root.left, root.right)])
        while queue:
            t1, t2 = queue.popleft()
            if not t1 and not t2:
                continue
            if not t1 or not t2:
                return False
            if t1.val != t2.val:
                return False
            queue.append((t1.left, t2.right))
            queue.append((t1.right, t2.left))
        
        return True
```

**Step-by-Step Explanation:**
1. Use a queue to store node pairs.
2. Compare each pair:
   - If both nodes are `None`, they are symmetric, continue.
   - If one is `None`, return `False`.
   - If values are unequal, return `False`.
3. Add children in mirrored order:
   - Left child of `t1` and right child of `t2`.
   - Right child of `t1` and left child of `t2`.

---

### 5. Review

#### Dry Run
Input:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

**DFS Steps:**
1. Compare `root.left (2)` with `root.right (2)`: Equal.
2. Compare `root.left.left (3)` with `root.right.right (3)`: Equal.
3. Compare `root.left.right (4)` with `root.right.left (4)`: Equal.
4. All checks pass, return `True`.

**BFS Steps:**
1. Queue: `[(2, 2)]`.
2. Dequeue `(2, 2)`, enqueue `[(3, 3), (4, 4)]`.
3. Dequeue `(3, 3)`, enqueue `[]` (no children).
4. Dequeue `(4, 4)`, enqueue `[]`.
5. Queue empty, return `True`.

---

### 6. Evaluate

#### Time Complexity
- **DFS:** O(n) — Every node is visited once.
- **BFS:** O(n) — Every node is enqueued and dequeued once.

#### Space Complexity
- **DFS:** O(h) — Recursion stack height (`h` is the tree height).
- **BFS:** O(n) — Queue size in the worst case.

---

### Why This Problem Is Important
- **Industry Relevance:**
  - Symmetry checks are common in UI rendering, graphics, and tree-based algorithms.
- **Prerequisites:**
  - Understanding binary trees, recursion, and iterative traversal.
- **Follow-up Practice:**
  - [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
  - [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
  - [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
