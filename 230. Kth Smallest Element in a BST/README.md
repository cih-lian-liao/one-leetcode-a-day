# LeetCode 230: Kth Smallest Element in a BST

### Problem Statement

Given the `root` of a binary search tree (BST) and an integer `k`, return the `k`th smallest element in the BST.

### Inputs:
- `root`: The root node of a BST.
- `k`: An integer representing the desired position in the sorted order (1-indexed).

### Outputs:
- The value of the `k`th smallest element in the BST.

### Constraints:
- The number of nodes in the BST is `n`, where `1 ≤ k ≤ n ≤ 10^4`.
- `-10^4 ≤ Node.val ≤ 10^4`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
To solve this problem:
1. **Inputs:** A BST and a positive integer `k`.
2. **Outputs:** The value of the `k`th smallest element in the BST.
3. **Clarification:** The BST property ensures that an inorder traversal will produce elements in sorted order. Utilize this property to efficiently locate the `k`th smallest element.
---
### 2. Match
- **Pattern Identified:** This problem matches with the "inorder traversal of a binary tree" pattern.
- **Data Structures Needed:** Use a stack for iterative inorder traversal to maintain O(1) space for recursion.
---
### 3. Plan
1. Initialize an empty stack and a counter `n = 0`.
2. Perform an iterative inorder traversal using the stack:
   - Traverse to the leftmost node while adding nodes to the stack.
   - Pop the top of the stack, process it, and increment the counter `n`.
   - If `n == k`, return the node's value.
   - Move to the right child and repeat.
3. Time Complexity: O(H + k), where `H` is the height of the BST.
4. Space Complexity: O(H), where `H` is the height of the stack.

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
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        # Step 1: Initialize variables
        n = 0  # Counter for tracking the number of processed nodes
        stack = []  # Stack for traversal
        cur = root  # Current node for traversal

        # Step 2: Iterative inorder traversal
        while cur or stack:
            # Traverse to the leftmost node
            while cur:
                stack.append(cur)  # Add current node to stack
                cur = cur.left  # Move to the left child
            
            # Process the node at the top of the stack
            cur = stack.pop()  # Pop the top node
            n += 1  # Increment counter
            if n == k:  # If kth element is reached, return its value
                return cur.val
            
            # Move to the right child
            cur = cur.right
```

### Notes for Implementation
1. **Initialization:** Use a stack and counter `n` to keep track of processed nodes.
2. **Inorder Traversal:** Mimic recursion with a while loop.
3. **Decrement `k`:** When the counter `n` equals `k`, return the current node's value.
4. **Edge Cases:** Ensure `k` is valid (within the range of total nodes).

---

### 5. Review

#### Dry Run Example 1
Given the tree:

```
        3
       / \
      1   4
       \
        2
```

- Input: `root = [3,1,4,null,2], k = 2`
- Process:
  - Start at root (3), traverse left to 1.
  - Process 1 (`n = 1`).
  - Move to 2, process 2 (`n = 2`).
- Output: `2`.

#### Dry Run Example 2 (Complex Example)
Given the tree:

```
          5
         / \
        3   6
       / \
      2   4
     /
    1
```

- Input: `root = [5,3,6,2,4,null,null,1], k = 4`

**Step-by-step Traversal:**
1. Start at the root (5), traverse left to 3, then to 2, and finally to 1. Add nodes to the stack in the order: `[5, 3, 2, 1]`.
2. Pop the stack:
   - Process 1 (`n = 1`).
   - Move to `null` (1 has no right child).
3. Pop the stack:
   - Process 2 (`n = 2`).
   - Move to `null` (2 has no right child).
4. Pop the stack:
   - Process 3 (`n = 3`).
   - Move to 4 (right child of 3).
5. Pop the stack:
   - Process 4 (`n = 4`).
   - The 4th smallest element is `4`.

- Output: `4`.

---

### 6. Evaluate

#### Time Complexity
- O(H + k), where `H` is the height of the tree.
- Traversal stops as soon as the `k`th element is found.

#### Space Complexity
- O(H), where `H` is the height of the tree, for the stack.

##### The **time complexity** is **O(H + k)**, where:

- `H` is the height of the BST, representing the maximum depth of the tree. This is the cost of traversing to the leftmost node during initialization or returning to process nodes from the stack.
- `k` represents the cost of visiting and processing `k` nodes in inorder traversal until we reach the `k`th smallest element.

##### Why Not O(n)?
While O(n) is the worst-case time complexity for traversing the entire BST, in this specific problem, we stop processing as soon as we find the `k`th smallest element. This means we do not visit all `n` nodes in the BST in most cases, making **O(H + k)** a more precise representation of the actual cost.

##### How to Explain This Orally in an Interview:
1. Start by describing the two parts of the complexity:
   - "The **O(H)** part accounts for the stack operations and the initial traversal to the leftmost node."
   - "The **O(k)** part comes from processing `k` nodes during the inorder traversal."
2. Explain the efficiency:
   - "Since we stop once we find the `k`th smallest element, we avoid traversing unnecessary parts of the BST. This makes our approach more efficient than a full traversal, which would take O(n)."
3. Use edge cases to clarify:
   - "For example, if `k = 1` and the tree is left-skewed, the traversal will mostly involve going down the left side of the tree, resulting in a complexity closer to O(H)."
   - "If `k` is close to `n`, we may approach the worst-case O(n)."

##### The **space complexity** is **O(H)** because:
- The stack can hold at most `H` nodes at any given time, where `H` is the height of the tree.
- The height `H` is `log(n)` for a balanced BST and `n` for a skewed BST.

##### Why O(H) is Precise:
1. In recursive inorder traversal, the call stack would also use O(H), but here we replace it with an explicit stack, keeping the same complexity.
2. We don’t need to store additional data besides the stack, ensuring space efficiency.

##### How to Explain This Orally in an Interview:
1. Start by linking space complexity to the tree structure:
   - "The height of the tree determines the maximum size of our stack during traversal."
2. Provide a comparison:
   - "In a balanced BST, the height is about `log(n)`, so space complexity is `O(log(n))`. In a worst-case skewed tree, the height is `n`, leading to `O(n)` space complexity."
3. Emphasize why this is optimal:
   - "This is the best we can achieve for iterative traversal because the stack is essential to mimic recursion for inorder traversal."

##### Why O(n) Isn't Always the Best Description:
- **O(n)** assumes that all nodes will be processed, which is not true for this problem. Stopping the traversal early makes O(H + k) more accurate.
- **O(H + k)** provides a nuanced view, capturing the efficiency of stopping once the `k`th smallest element is found.

---

### Additional Notes

### Why This Problem is Important
- **Industry Relevance:** Understanding iterative traversal is crucial in software engineering interviews for roles involving data structures.
- **Skill-building:** Efficient traversal and problem-solving techniques in tree-based problems.

### Prerequisites
- Familiarity with binary trees.
- Understanding stack-based traversal.

### Follow-up Practice Problems
1. LeetCode 94: Binary Tree Inorder Traversal.
2. LeetCode 701: Insert into a Binary Search Tree.
3. LeetCode 98: Validate Binary Search Tree.
