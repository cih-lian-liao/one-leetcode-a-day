

# LeetCode 337: House Robber III

# Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses along this street are arranged in a binary tree. It is impossible to rob two directly connected houses (parent and child). Your task is to calculate the maximum amount of money you can rob without alerting the police.

### Example

#### Example 1:
```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation:
  3
 / \
2   3
 \    \ 
  3    1
Maximum amount robbed = 3 + 3 + 1 = 7.
```

#### Example 2:
```
Input: root = [3,4,5,1,3,null,1]
Output: 9
Explanation:
     3
    / \
   4   5
  / \    \
 1   3    1
Maximum amount robbed = 4 + 5 = 9.
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Input**: A binary tree where each node contains a non-negative integer representing the money at that house.
- **Output**: An integer representing the maximum amount that can be robbed.
- **Constraints**:
  - Nodes contain non-negative values.
  - No two directly connected houses can be robbed.

**Clarifications**:
- If the tree is empty (`root == None`), return `0`.
- Each node's value is independent of other nodes.

---

### 2. Match

- **Pattern**: Dynamic Programming on Trees.
- **Similar Problems**: "House Robber" (Linear), "House Robber II" (Circular Array).
- **Key Insight**: For each node, compute two values:
  - `withRoot`: Maximum amount if we rob the current node.
  - `withoutRoot`: Maximum amount if we skip the current node.

---

### 3. Plan

1. Define a recursive function `dfs(node)`:
   - If the node is `None`, return `[0, 0]` (no money can be robbed).
   - Recursively compute `leftPair` and `rightPair` for left and right child nodes.
2. Calculate:
   - `withRoot`: Robbing the current node. Formula:
     ```
     withRoot = node.val + leftPair[1] + rightPair[1]
     ```
   - `withoutRoot`: Skipping the current node. Formula:
     ```
     withoutRoot = max(leftPair) + max(rightPair)
     ```
3. Return `[withRoot, withoutRoot]` for each node.
4. The final answer is `max(dfs(root))`.

**Time Complexity**: O(n)  
**Space Complexity**: O(h), where `h` is the height of the tree (stack space for recursion).

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
    def rob(self, root: Optional[TreeNode]) -> int:
        
        def dfs(root):
            # Base Case: If the node is None
            if not root:
                return [0, 0]
            
            # Recursively calculate left and right child
            leftPair = dfs(root.left)
            rightPair = dfs(root.right)

            # Calculate robbing or skipping the current node
            withRoot = root.val + leftPair[1] + rightPair[1]
            withoutRoot = max(leftPair) + max(rightPair)

            # Return both possibilities
            return [withRoot, withoutRoot]
        
        # Calculate the final result
        return max(dfs(root))
```

**Notes for Implementation**:
- Clearly define the recursive base case: `return [0, 0]` for `None`.
- Use helper variables (`leftPair`, `rightPair`) to simplify readability.
- Ensure both cases (`withRoot` and `withoutRoot`) are computed independently.

---

### 5. Review

**Dry Run with Example Input:**
Tree:
```

    3
   / \
  2   3
   \    \
    3    1

```

Dry run example continues:

1. **Node = None**  
   - Return `[0, 0]`.

2. **Node = 3 (left-most leaf)**  
   - `leftPair = [0, 0]`, `rightPair = [0, 0]`  
   - `withRoot = 3 + 0 + 0 = 3`  
   - `withoutRoot = max(0) + max(0) = 0`  
   - Return `[3, 0]`.

3. **Node = 2**  
   - `leftPair = [0, 0]`, `rightPair = [3, 0]`  
   - `withRoot = 2 + 0 + 0 = 2`  
   - `withoutRoot = max(0) + max(3) = 3`  
   - Return `[2, 3]`.

4. **Node = 1 (right-most leaf)**  
   - `leftPair = [0, 0]`, `rightPair = [0, 0]`  
   - `withRoot = 1 + 0 + 0 = 1`  
   - `withoutRoot = max(0) + max(0) = 0`  
   - Return `[1, 0]`.

5. **Node = 3 (right child of root)**  
   - `leftPair = [0, 0]`, `rightPair = [1, 0]`  
   - `withRoot = 3 + 0 + 0 = 3`  
   - `withoutRoot = max(0) + max(1) = 1`  
   - Return `[3, 1]`.

6. **Root = 3**  
   - `leftPair = [2, 3]`, `rightPair = [3, 1]`  
   - `withRoot = 3 + 3 + 1 = 7`  
   - `withoutRoot = max(2) + max(3) = 6`  
   - Return `[7, 6]`.

**Output**: `max(7, 6) = 7`.

---

### 6. Evaluate

**Time Complexity**:  
O(n), where `n` is the number of nodes in the tree. Each node is visited once.

**Space Complexity**:  
O(h), where `h` is the height of the tree, due to the recursion stack.

---

### Additional Notes

#### Importance of This Problem:
- It extends the linear dynamic programming concepts in "House Robber" to a binary tree structure.
- Enhances problem-solving skills for tree-based dynamic programming.

#### Prerequisites for Practicing This Problem:
1. Familiarity with recursion.
2. Understanding of dynamic programming basics.
3. Knowledge of binary tree traversal.

#### Industry Relevance:
- Efficient tree traversal algorithms are critical in various domains like networking, file systems, and AI.

#### Follow-up Practice Problems:
1. LeetCode 198: House Robber
2. LeetCode 213: House Robber II
3. LeetCode 124: Binary Tree Maximum Path Sum
4. LeetCode 437: Path Sum III
