# LeetCode Problem 144: Binary Tree Preorder Traversal


#### Problem Description
You are given the `root` of a binary tree. Return the **preorder traversal** of its nodes' values. Preorder traversal follows the order:
1. Visit the root node.
2. Traverse the left subtree.
3. Traverse the right subtree.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**
**Inputs:**
- `root`: A binary tree node or `None` if the tree is empty.

**Outputs:**
- An array of integers representing the preorder traversal of the tree.

**Constraints:**
- The number of nodes in the tree is in the range `[0, 10^4]`.
- `-100 <= Node.val <= 100`.

---

### **2. Match**
- The problem relates to **tree traversal**.
- The specific traversal pattern here is **preorder**: **Root → Left → Right**.
- This can be solved using either:
  - Recursive DFS, or
  - Iterative DFS using a stack.

We will use **iterative DFS** for this solution to ensure efficiency and avoid stack overflow for large inputs.

---

### **3. Plan**
**Iterative Plan:**
1. **Initialization:**
   - Use a stack to simulate the recursive call stack.
   - Use a `res` list to store the traversal results.
   - Use a variable `cur` to track the current node, starting at the root.

2. **Traversal Loop:**
   - While `cur` is not `None` or the stack is not empty:
     - If `cur` is not `None`:
       - Add `cur.val` to `res` (visit the root).
       - Push `cur.right` to the stack (to process later).
       - Move `cur` to `cur.left`.
     - If `cur` is `None`:
       - Backtrack to the saved right child by popping the stack and assigning it to `cur`.

3. **Return the Result:**
   - Once traversal is complete, return `res`.

---

### **4. Implement**
```python
def preorderTraversal(root):
    # Initialize variables
    cur, stack = root, []
    res = []
    
    # Iterative traversal
    while cur or stack:
        if cur:
            # Visit root and add to result
            res.append(cur.val)
            # Save the right child for later
            stack.append(cur.right)
            # Move to the left child
            cur = cur.left
        else:
            # Backtrack to the saved right child
            cur = stack.pop()
    
    return res
```

**Detailed Notes for Implementation (Step-by-Step):**
1. **Initialize Variables:**
   - `cur` starts at `root`.
   - `stack` will help us save the right children for later processing.
   - `res` stores the traversal result.

2. **While Loop**:
   - As long as there is a current node (`cur`) or the stack has elements, keep iterating.

3. **Visit Current Node:**
   - If `cur` is not `null`, add `cur.val` to `res`.
   - Save the right child of the current node (`cur.right`) by pushing it to the stack.
   - Move `cur` to its left child (`cur = cur.left`).

4. **Handle Null Node:**
   - If `cur` is `null`, pop the stack to backtrack to the right child and assign it to `cur`.

5. **Return Result:**
   - Once traversal is complete, return `res`.

---

### **5. Review**
**Testing:**
- **Example 1:**
  ```python
  Input: root = [1, null, 2, 3]
  Output: [1, 2, 3]
  ```
- **Example 2:**
  ```python
  Input: root = []
  Output: []
  ```
- **Example 3:**
  ```python
  Input: root = [1]
  Output: [1]
  ```

**Edge Cases:**
- An empty tree (`root = None`) → Output should be `[]`.
- A tree with only one node → Output should be `[root.val]`.
- Skewed trees (all nodes on one side) → Ensure correct traversal.

---

### **6. Evaluate**
- **Time Complexity:** O(n)
  - We visit each node exactly once.
- **Space Complexity:** O(h)
  - The stack can grow up to the height of the tree `h`.
---
#### **Dry Run Example**

#### Example 1: Complex Tree
```text
Input:
        10
       /  \
      5    15
     / \     \
    3   7     18

Expected Output: [10, 5, 3, 7, 15, 18]
```

**Step-by-Step Execution:**
1. **Initialization:**
   - `cur = root` (`10`), `stack = []`, `res = []`.

2. **First Iteration (cur = 10):**
   - Add `10` to `res` → `res = [10]`.
   - Push `cur.right` (`15`) to `stack` → `stack = [15]`.
   - Move `cur` to `cur.left` (`5`).

3. **Second Iteration (cur = 5):**
   - Add `5` to `res` → `res = [10, 5]`.
   - Push `cur.right` (`7`) to `stack` → `stack = [15, 7]`.
   - Move `cur` to `cur.left` (`3`).

4. **Third Iteration (cur = 3):**
   - Add `3` to `res` → `res = [10, 5, 3]`.
   - Push `cur.right` (`None`) to `stack` → `stack = [15, 7, None]`.
   - Move `cur` to `cur.left` (`None`).

5. **Backtrack (cur = None):**
   - Pop the top of the stack → `stack = [15, 7]`.
   - Assign the popped value (`None`) to `cur`.

6. **Next Iteration (cur = None):**
   - Pop the top of the stack → `stack = [15]`.
   - Assign `cur = 7`.

7. **Fourth Iteration (cur = 7):**
   - Add `7` to `res` → `res = [10, 5, 3, 7]`.
   - Push `cur.right` (`None`) to `stack` → `stack = [15, None]`.
   - Move `cur` to `cur.left` (`None`).

8. **Backtrack (cur = None):**
   - Pop the top of the stack → `stack = [15]`.
   - Assign the popped value (`None`) to `cur`.

9. **Next Iteration (cur = None):**
   - Pop the top of the stack → `stack = []`.
   - Assign `cur = 15`.

10. **Fifth Iteration (cur = 15):**
    - Add `15` to `res` → `res = [10, 5, 3, 7, 15]`.
    - Push `cur.right` (`18`) to `stack` → `stack = [18]`.
    - Move `cur` to `cur.left` (`None`).

11. **Backtrack (cur = None):**
    - Pop the top of the stack → `stack = []`.
    - Assign `cur = 18`.

12. **Sixth Iteration (cur = 18):**
    - Add `18` to `res` → `res = [10, 5, 3, 7, 15, 18]`.
    - Push `cur.right` (`None`) to `stack` → `stack = [None]`.
    - Move `cur` to `cur.left` (`None`).

13. **End:**
    - Both `cur` is `None` and `stack` is empty.

**Final Result:**
`res = [10, 5, 3, 7, 15, 18]`

---

### **Recursive Solution**

```python
def preorderTraversal(root):
    result = []
    
    def preorder(node):
        if not node:
            return
        # Visit root
        result.append(node.val)
        # Recursively traverse left subtree
        preorder(node.left)
        # Recursively traverse right subtree
        preorder(node.right)
    
    preorder(root)
    return result
```

**Explanation:**
1. **Base Case:** If the current node is `None`, return immediately (no further traversal required).
2. **Visit Root:** Add the value of the current node (`node.val`) to `result`.
3. **Recursive Calls:**
   - Call the `preorder` function on `node.left` to traverse the left subtree.
   - Call the `preorder` function on `node.right` to traverse the right subtree.

#### **Time and Space Complexity for Recursive Solution**
- **Time Complexity:** O(n)
  - Each node is visited exactly once.
- **Space Complexity:** O(h)
  - The recursion stack requires space proportional to the height of the tree `h`.

---

#### **Why This Problem is Important**
- **Industry Relevance:** Tests fundamental knowledge of tree traversal techniques, essential for many real-world applications.
- **Prerequisites:** Understanding of binary trees and recursion.

#### **Follow-Up Problems**
1. [LeetCode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
2. [LeetCode 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)
3. [LeetCode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
