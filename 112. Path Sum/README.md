# LeetCode 112: Path Sum

### Problem Description

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf path** such that adding up all the values along the path equals `targetSum`. A **leaf** is a node with no children.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs:**

- `root`: The root node of a binary tree (may be `None`).
- `targetSum`: An integer, representing the target sum of the path.

**Outputs:**

- `True` if there exists a root-to-leaf path where the sum of the node values equals `targetSum`.
- `False` otherwise.

**Constraints:**

- The number of nodes in the tree is in the range [0, 5000].
- `-1000 <= Node.val <= 1000`.
- `-10^9 <= targetSum <= 10^9`.

**Assumptions and Clarifications:**

- A "path" must go from the root to a leaf node.
- If `root` is `None`, the result is `False`.

---

### 2. Match

This problem aligns with **tree traversal** patterns, specifically:

- **Depth-First Search (DFS):** Recursive or iterative.
- Use a helper function to pass cumulative sums during the traversal.

**Relevant Data Structure:**

- Binary Tree.

---

### 3. Plan

#### Approach: Recursive DFS with Helper Function

1. **Recursive Helper Function:**
   - Accept the current node (`root`) and the cumulative sum so far (`cur_sum`) as arguments.

2. **Base Case:**
   - If the current node is `None`, return `False`.
   - If the current node is a leaf, check if `cur_sum + root.val == targetSum`.

3. **Recursive Case:**
   - Call the helper function recursively for the left and right children of the current node, passing the updated cumulative sum.

4. **Combine Results:**
   - Return `True` if either the left or right subtree returns `True`.

5. **Edge Cases:**
   - Empty tree (`root == None`).
   - Single-node tree.

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
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        
        def has_sum(root, cur_sum):
            # Base Case: If the current node is None, there is no path
            if not root:
                return False

            # Update the cumulative sum
            cur_sum += root.val

            # Check if the current node is a leaf and the cumulative sum matches the target
            if not root.left and not root.right:
                return cur_sum == targetSum
            
            # Recursive calls for left and right subtrees
            return has_sum(root.left, cur_sum) or has_sum(root.right, cur_sum)

        # Start the recursion with the root node and an initial cumulative sum of 0
        return has_sum(root, 0)
```

---

### 5. Review

#### Example Walkthrough:

**Example Input:**
```text
root = [5, 4, 8, 11, None, 13, 4, 7, 2, None, None, None, 1]
targetSum = 22
```

**Tree Structure:**
```
          5
       /    \
     4        8
    /        /  \
   11      13   4
  /  \          \
 7    2          1
```

1. Start at `root = 5`, `cur_sum = 0`.
   - `cur_sum = 0 + 5 = 5`.
2. Move to left subtree (`root = 4`), `cur_sum = 5`.
   - `cur_sum = 5 + 4 = 9`.
3. Move to left subtree (`root = 11`), `cur_sum = 9`.
   - `cur_sum = 9 + 11 = 20`.
4. Move to left subtree (`root = 7`), `cur_sum = 20`.
   - `cur_sum = 20 + 7 = 27`. This path fails.
5. Backtrack and move to right subtree (`root = 2`), `cur_sum = 20`.
   - `cur_sum = 20 + 2 = 22`. This path succeeds.

Result: **True**

---

### 6. Evaluate

#### Time Complexity:
- Each node is visited once, so the time complexity is **O(n)**, where `n` is the number of nodes in the tree.

#### Space Complexity:
- In the worst case (skewed tree), the recursion stack uses **O(n)** space.
- In the best case (balanced tree), the recursion stack uses **O(log n)** space.

#### Optimizations:
- If the tree is balanced, this solution is optimal.

---

### Additional Notes

### Backtracking Solution and Explanation

#### Code:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def hasPathSum(root: TreeNode, targetSum: int) -> bool:
    if not root:
        return False
    
    # Base case: if the current node is a leaf
    if not root.left and not root.right:
        return targetSum == root.val

    # Update the target sum by subtracting the current node's value
    targetSum -= root.val

    # Recursive calls to left and right children
    return self.hasPathSum(root.left, targetSum) or self.hasPathSum(root.right, targetSum)
```

#### Explanation:

1. **Base Case:**
   - If the current node is `None`, there’s no path, so return `False`.
   - If the current node is a leaf (no left or right child), check whether its value equals the `targetSum`.

2. **Recursive Case:**
   - Subtract the value of the current node from `targetSum` to update the remaining sum.
   - Recursively call the function for the left and right children.

3. **Combining Results:**
   - Return `True` if either the left or right subtree has a valid path.

4. **Edge Cases:**
   - An empty tree returns `False`.
   - A single-node tree only succeeds if the node’s value equals the target sum.

#### Complexity:
- **Time Complexity:** **O(n)** (each node is visited once).
- **Space Complexity:** **O(n)** for recursion stack in the worst case (skewed tree).

---

### Why This Problem is Important

- **Industry Relevance:** Traversing trees is fundamental in many systems, such as file systems, AI (decision trees), and databases.
- **Common Pattern:** Recursive DFS with cumulative sums is a widely used technique for path-related problems.

### Prerequisites for Practicing This Problem

1. Basics of binary trees.
2. Recursive programming concepts.

### Follow-up Practice Problems

1. [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/) (Find all root-to-leaf paths).
2. [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/) (Count paths that sum to a given value).
3. [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/).
