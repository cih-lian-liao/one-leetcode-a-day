# LeetCode 100: Same Tree

### Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

### Constraints

1. The number of nodes in both trees is in the range [0, 100].
2. `-10^4 <= Node.val <= 10^4`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

- **Input**: Two binary tree roots `p` and `q`.
- **Output**: A boolean (`True` if trees are the same, `False` otherwise).
- **Constraints**:
  - Both trees can have up to 100 nodes.
  - Values of nodes range from `-10^4` to `10^4`.
- **Assumptions**:
  - If both trees are empty, they are the same.
  - If one tree is empty and the other is not, they are different.

#### Key Questions:
1. What does "structurally identical" mean?
   - Trees have the same shape, and corresponding nodes have the same value.
2. Can we use recursive or iterative solutions?
   - Yes, both approaches are feasible.

---

### 2. Match

- **Problem Type**: Tree traversal and comparison.
- **Patterns**: 
  - Recursive traversal (depth-first search).
  - Iterative traversal using a queue (breadth-first search).

---

### 3. Plan (Recursive Solution)

1. Check if both nodes (`p` and `q`) are `None`.
   - If true, return `True`.
2. Check if one node is `None` or their values differ.
   - If true, return `False`.
3. Recursively compare:
   - Left child of `p` with left child of `q`.
   - Right child of `p` with right child of `q`.

#### Time Complexity:
- Recursive: O(n), where `n` is the number of nodes in the trees.

#### Space Complexity:
- Recursive: O(h), where `h` is the height of the tree (due to recursion stack).

---

### 4. Implement (Recursive Solution)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # Base Case: Both nodes are None
        if not p and not q:
            return True
        
        # One node is None or values are not the same
        if not p or not q or p.val != q.val:
            return False
        
        # Recursive calls for left and right subtrees
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

---

### 5. Review

#### Debugging
- Test cases:
  - Both trees are `None`: `p = None, q = None` → Output: `True`.
  - One tree is `None`: `p = TreeNode(1), q = None` → Output: `False`.
  - Structurally identical trees: `p = [1,2,3], q = [1,2,3]` → Output: `True`.
  - Different structures: `p = [1,2], q = [1,null,2]` → Output: `False`.

#### Dry Run Example 1 (Simple Case, Output = True):

Input:  
`p = [1, 2, 3]`  
`q = [1, 2, 3]`

Both trees can be represented as follows:

```
Tree p:          Tree q:
    1                1
   / \              / \
  2   3            2   3
```

Final output: **`True`** (trees are identical).

---

#### Dry Run Example 2 (Complex Case, Output = True):

Input:  
`p = [1, 2, 3, 4, null, null, 5]`  
`q = [1, 2, 3, 4, null, null, 5]`

Both trees can be represented as follows:

```
Tree p:                 Tree q:
      1                     1
     / \                   / \
    2   3                 2   3
   /     \               /     \
  4       5             4       5
```

Final output: **`True`** (trees are identical).

---

#### Dry Run Example 3 (Complex Case, Output = False):

Input:  
`p = [1, 2, 3, 4]`  
`q = [1, 2, 3, null, 5]`

Both trees can be represented as follows:

```
Tree p:                 Tree q:
      1                     1
     / \                   / \
    2   3                 2   3
   /                     / \
  4                     null  5
```

1. **Step 1**: Compare roots:
   - `p.val = 1`, `q.val = 1` → Match → Recurse.

2. **Step 2**: Compare left children:
   - `p.left.val = 2`, `q.left.val = 2` → Match → Recurse.

3. **Step 3**: Compare left child of `2`:
   - `p.left.left.val = 4`, `q.left.left` is `None` → Mismatch → Return `False`.

Final output: **`False`** (trees are not identical).

---

### 6. Evaluate

#### Complexity:
- Time Complexity: O(n), where `n` is the total number of nodes in the trees.
- Space Complexity: O(h), where `h` is the height of the tree.

#### Optimizations:
- Recursive approach is simple and concise.
- Use iterative approach to avoid recursion depth issues for very deep trees (see additional notes).

---

### Additional Notes

### Iterative Solution:

1. Use a queue to store pairs of nodes `(p, q)` for comparison.
2. While the queue is not empty:
   - Pop a pair from the queue.
   - Check if both nodes are `None`. Continue if true.
   - If one is `None` or values differ, return `False`.
   - Add left children and right children of both nodes to the queue.
3. If the queue is exhausted, return `True`.

### Implementation:

```python
from collections import deque

def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    queue = deque([(p, q)])
    while queue:
        node1, node2 = queue.popleft()

        # Both nodes are None
        if not node1 and not node2:
            continue

        # One is None or values are not the same
        if not node1 or not node2 or node1.val != node2.val:
            return False

        # Add children to the queue for comparison
        queue.append((node1.left, node2.left))
        queue.append((node1.right, node2.right))
    
    return True
```

### Complexity of Iterative Solution:
- **Time Complexity**: O(n), where `n` is the total number of nodes.
- **Space Complexity**: O(n), for the queue storing nodes.

---

### Why This Problem Is Important:

- Reinforces recursive thinking for tree traversal.
- Demonstrates patterns useful in other tree comparison problems.

### Industry Relevance:
- Frequently asked in coding interviews as a fundamental tree problem.

### Prerequisites:
- Understanding of tree traversal (DFS/BFS).
- Familiarity with recursion and iterative tree traversal.

### Follow-Up Practice Problems:
1. **LeetCode 572: Subtree of Another Tree**
2. **LeetCode 110: Balanced Binary Tree**
3. **LeetCode 101: Symmetric Tree**
4. **LeetCode 222: Count Complete Tree Nodes**

