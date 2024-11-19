# LeetCode Problem 104: Maximum Depth of Binary Tree

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

**Problem Statement:**  
Given a binary tree, find its maximum depth. The maximum depth is the number of nodes along the longest path from the root node to the farthest leaf node.

**Inputs:**  
- A binary tree root node (`TreeNode` object or `null` if the tree is empty).

**Outputs:**  
- An integer representing the maximum depth of the tree.

**Constraints:**
- The number of nodes in the tree is between 0 and 10⁴.
- Tree node values can be any integer.

**Clarifications:**  
- If the tree is empty, the depth is `0`.
- The depth of a single-node tree is `1`.

---

### **2. Match**

**Problem Type:**  
- This is a tree traversal problem that involves finding the longest path.

**Relevant Algorithms and Data Structures:**  
- Breadth-First Search (BFS): Useful for level-order traversal to count levels (preferred in this solution).
- Depth-First Search (DFS): Alternative recursive approach to explore depths of subtrees.

---

### **3. Plan**

#### **Plan Using BFS:**
1. Use a queue to perform level-order traversal of the binary tree.
2. Initialize the `depth` variable to `0`.
3. While the queue is not empty:
   - Process all nodes at the current level.
   - Add their child nodes to the queue for the next level.
   - Increment the depth counter.
4. When the queue is empty, return the `depth`.

**Edge Cases:**  
- Empty tree (`root = null`): Return `0`.
- Single-node tree: Return `1`.

**Time Complexity:** O(n)  
- Each node is visited once.

**Space Complexity:** O(w)  
- `w` is the maximum width of the tree, which is the number of nodes at the largest level.

---

### **4. Implement**

```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root: TreeNode) -> int:
    if not root:
        return 0  # Empty tree case
    
    queue = deque([root])  # Initialize queue with root node
    depth = 0  # Initialize depth
    
    while queue:
        level_size = len(queue)  # Number of nodes at current level
        for _ in range(level_size):  # Process all nodes at this level
            node = queue.popleft()  # Remove the current node
            if node.left:  # Add left child to queue
                queue.append(node.left)
            if node.right:  # Add right child to queue
                queue.append(node.right)
        depth += 1  # Increment depth after processing a level
    
    return depth
```

**Implementation Details:**
1. Check if the tree is empty (`root == null`). If yes, return `0`.
2. Use a queue (`deque`) to hold nodes for level-order traversal.
3. Traverse each level:
   - For every node at the current level, pop it from the queue and enqueue its children (if any).
   - After processing the level, increment the `depth`.
4. When the queue is empty, all levels are processed, and the depth is returned.

---

### **5. Review**

**Edge Cases to Test:**
1. Empty tree (`root = null`): Expected output = `0`.
2. Single-node tree: Expected output = `1`.
3. Balanced tree with multiple levels: Ensure the depth matches the actual height.
4. Left-skewed or right-skewed tree: Ensure the algorithm correctly handles unbalanced cases.

**Complex Case Dry Run:**

**Input:**
```text
    1
   / \
  2   3
 / \
4   5
```

**Execution Steps:**
1. Initialize `queue = [1]` and `depth = 0`.
2. Process level 1: `queue = []`, add children `2` and `3`, `depth = 1`.
3. Process level 2: `queue = [4, 5]`, `depth = 2`.
4. Process level 3: `queue = []`, `depth = 3`.

**Output:** `3`

---

### **6. Evaluate**

**Time Complexity:** O(n)  
- Each node is visited exactly once.

**Space Complexity:** O(w)  
- `w` is the maximum width of the tree, which occurs at the largest level.

---

### **DFS Solution**

**Plan Using DFS (Recursive):**
1. If the current node is `null`, return `0`.
2. Recursively find the depth of the left and right subtrees.
3. Return the maximum of the two depths plus `1`.

**DFS Implementation:**
```python
def maxDepthDFS(root: TreeNode) -> int:
    if not root:
        return 0
    left_depth = maxDepthDFS(root.left)
    right_depth = maxDepthDFS(root.right)
    return max(left_depth, right_depth) + 1
```

**DFS Time Complexity:** O(n)  
**DFS Space Complexity:** O(h)  
- `h` is the height of the tree (recursion stack depth).

---

### **BFS vs DFS Comparison**

| **Aspect**       | **BFS**                                      | **DFS**                                      |
|-------------------|----------------------------------------------|----------------------------------------------|
| **Traversal**     | Level by level                              | Depth first (down each path to leaves)       |
| **Memory Usage**  | O(w), depends on the tree width             | O(h), depends on the tree height             |
| **Speed**         | Uniform traversal across levels             | May be faster for finding depths            |
| **Implementation**| Iterative using a queue                     | Recursive or stack-based iterative           |
| **Best Use Case** | Good for level-by-level problems            | Good for depth-specific problems             |
| **For This Problem** | BFS is better for clarity and level-order logic | DFS is also valid but may risk stack overflow |

---

### **Why This Problem is Important**

- **Industry Relevance:** Binary trees and traversal algorithms are foundational in many coding interviews and are widely used in system design (e.g., parsers, file systems).
- **Prerequisites:** Understanding tree structures, recursion, BFS/DFS traversal techniques.


### **Follow-up Practice Problems**
1. **LeetCode 110:** Balanced Binary Tree
2. **LeetCode 111:** Minimum Depth of Binary Tree
3. **LeetCode 543:** Diameter of Binary Tree
4. **LeetCode 226:** Invert Binary Tree
