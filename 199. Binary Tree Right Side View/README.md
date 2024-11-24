
# LeetCode Problem 199: Binary Tree Right Side View

### Problem Statement

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it. Return the values of the nodes you can see ordered from top to bottom.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

#### Inputs
- `root`: The root node of a binary tree.

#### Outputs
- A list of integers representing the values of the nodes visible from the right side.

#### Constraints
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`.

#### Assumptions
- If the tree is empty, return an empty list.
- Only the rightmost node at each depth is visible.

---

### 2. Match

#### Problem Pattern
This problem can be solved using **level-order traversal (BFS)** or **depth-first search (DFS)**:
- BFS allows us to traverse each level of the tree and pick the last node for each level.
- DFS can also work if we prioritize visiting the right subtree before the left subtree and record the first node encountered at each depth.

#### Algorithm Choice
We will use BFS for the main solution, as it is intuitive and ensures level-order traversal.

---

### 3. Plan

#### BFS Solution
1. Initialize an empty list `result` to store the visible nodes.
2. Use a queue (`collections.deque`) to perform a level-order traversal.
3. For each level:
   - Iterate through all nodes in the queue.
   - Keep track of the last node’s value at this level.
   - Add its children (left and right) to the queue.
4. Append the last node value at each level to `result`.
5. Return `result`.

#### Edge Cases
- Empty tree (`root == None`): Return `[]`.
- Single node tree: Return the root’s value `[root.val]`.

---

### 4. Implement

#### BFS Implementation

```python
from collections import deque

def rightSideView(root):
    if not root:
        return []  # Edge case: empty tree
    
    result = []
    queue = deque([root])  # Initialize the queue with the root node
    
    while queue:
        level_size = len(queue)  # Number of nodes at the current level
        for i in range(level_size):
            node = queue.popleft()  # Get the front node of the queue
            if i == level_size - 1:  # If this is the last node in the level
                result.append(node.val)  # Add it to the result
            # Add child nodes to the queue
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```

---

### 5. Review

#### Dry Run

Consider the following binary tree:

```
        1
      /   \
     2     3
      \      \
       5      4
```

**Input**: `root = [1, 2, 3, null, 5, null, 4]`  
**Expected Output**: `[1, 3, 4]`

**Execution Steps**:
1. Start with `queue = [1]`, `result = []`.
2. **Level 1**: Dequeue `1`. Append `1` to `result`. Enqueue `2` and `3`.
   - `queue = [2, 3]`, `result = [1]`.
3. **Level 2**: Dequeue `2`, then `3`. Append `3` (last node in this level) to `result`. Enqueue `5` and `4`.
   - `queue = [5, 4]`, `result = [1, 3]`.
4. **Level 3**: Dequeue `5`, then `4`. Append `4` (last node in this level) to `result`.
   - `queue = []`, `result = [1, 3, 4]`.

**Output**: `[1, 3, 4]`

#### Edge Cases
1. Empty tree: `root = None` → Output: `[]`
2. Single node tree: `root = TreeNode(1)` → Output: `[1]`

---

### 6. Evaluate

#### Time Complexity
- **BFS**: Each node is visited once.  
  **Time Complexity**: O(n)

#### Space Complexity
- Queue stores up to `w` nodes, where `w` is the maximum width of the tree.  
  **Space Complexity**: O(w)

---

### Additional Notes

### DFS Solution

#### Explanation
Using **DFS**, we can explore the tree starting from the root, visiting the right subtree before the left. At each depth, we record the first node encountered, which corresponds to the visible node from the right side.

#### Implementation

```python
def rightSideViewDFS(root):
    def dfs(node, depth, result):
        if not node:
            return
        # Add the first node encountered at each depth
        if depth == len(result):
            result.append(node.val)
        dfs(node.right, depth + 1, result)  # Visit right subtree first
        dfs(node.left, depth + 1, result)  # Then visit left subtree

    result = []
    dfs(root, 0, result)
    return result
```

#### Time Complexity
- **DFS**: Each node is visited once.  
  **Time Complexity**: O(n)

#### Space Complexity
- Call stack uses up to `h` space, where `h` is the height of the tree.  
  **Space Complexity**: O(h)

#### Why BFS is Better in this case:
- **Direct alignment with the problem requirements**: It naturally traverses level by level.
- **Simpler implementation**: Fewer conditions and no explicit depth tracking.
- **More intuitive**: Easier to understand, visualize, and debug.
- **Performance**: Comparable time complexity, and space complexity is better in most cases.

However, DFS is still valuable in certain scenarios, such as when recursion provides a more elegant solution or when you need to explore the tree deeply before making decisions. It’s good to understand both approaches to decide which fits best for a given problem or tree structure.

---

### Why This Problem is Important

### Industry Relevance
- Understanding tree traversal (BFS and DFS) is crucial in handling hierarchical data structures, which are common in real-world applications like file systems and databases.

### Prerequisites for Practicing This Problem
- Binary tree basics
- BFS and DFS traversal techniques
- Familiarity with Python data structures (`deque`, recursion)

### Follow-up Practice Problems
1. [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
2. [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
3. [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)
