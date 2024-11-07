
# Problem 108. Convert Sorted Array to Binary Search Tree

### Problem Summary:
Given a sorted array, convert it into a **height-balanced** Binary Search Tree (BST). A height-balanced BST is a tree where the depth of the left and right subtrees of every node never differs by more than one.

### Key Concepts:
1. **Binary Search Tree (BST)**: A tree in which for each node:
   - Left children are smaller.
   - Right children are larger.
2. **Height-Balanced Tree**: A binary tree in which the depths of the two subtrees of every node never differ by more than 1.
3. **Divide and Conquer**: Given that the array is sorted, the middle element of any subarray can act as the root node, keeping the tree balanced.

### Approach to Solution:
The sorted array is processed using a **recursive divide-and-conquer** strategy:
1. **Middle Element as Root**: Select the middle element of the current subarray to serve as the root node of the tree/subtree.
2. **Divide the Array**: Recursively apply this process to the left and right halves of the array, forming left and right subtrees.
3. **Base Case**: If the left index surpasses the right, there’s no element to process, so return `None`.

### Code Implementation (Without Imports):
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def sortedArrayToBST(self, nums):
        # Helper function to convert a subarray to a BST
        def helper(left, right):
            # Base case: if left index is greater than right, there's no subtree
            if left > right:
                return None
            
            # Step 1: Find the middle element of the current subarray
            mid = (left + right) // 2
            
            # Step 2: Create a TreeNode with the middle element
            node = TreeNode(nums[mid])
            
            # Step 3: Recursively build the left and right subtrees
            node.left = helper(left, mid - 1)
            node.right = helper(mid + 1, right)
            
            # Return the root node of this subtree
            return node
        
        # Initial call to helper function with the full array range
        return helper(0, len(nums) - 1)
```

### Explanation (For Code Interview):

1. **Define TreeNode**:
   - We define a `TreeNode` class, which has a value (`val`), left child (`left`), and right child (`right`).

2. **Main Function (`sortedArrayToBST`)**:
   - The function takes the sorted list `nums` and returns the root node of a balanced BST.
   - It calls a helper function `helper(left, right)` to recursively construct the tree.

3. **Helper Function (`helper(left, right)`)**:
   - **Purpose**: Constructs a balanced subtree using elements within indices `left` and `right` in `nums`.
   - **Base Case**: 
     - When `left > right`, it indicates that there are no elements left in this subarray, so we return `None`. This happens when we’ve exhausted a section of the array during recursion, signaling the end of that subtree.
   - **Calculate Middle Index**:
     - We calculate the middle index, `mid = (left + right) // 2`, as the root element of the current subtree.
     - The middle element ensures that elements on the left and right are balanced around it, providing the height balance we need.
   - **Create Node**:
     - We create a `TreeNode` instance with `nums[mid]` as its value, making it the root of this subtree.
   - **Recursive Construction**:
     - For the left subtree: we call `helper(left, mid - 1)`, using elements before the middle element.
     - For the right subtree: we call `helper(mid + 1, right)`, using elements after the middle element.
     - Each recursive call progressively reduces the size of the array section, ultimately hitting the base case when the subarray is empty.
   - **Return the Node**:
     - After creating both left and right subtrees, we return the `node`. This node will be attached to its parent in the recursion stack, building up the tree structure.

4. **Final Call**:
   - We start with the entire array by calling `helper(0, len(nums) - 1)`, setting the initial bounds for `left` and `right` to cover the full range of the array.

### Example Walkthrough:
For `nums = [-10, -3, 0, 5, 9]`:
1. Middle element is `0` (root).
2. Left subarray `[-10, -3]`: Middle element `-3` becomes the left child of `0`.
3. Right subarray `[5, 9]`: Middle element `5` becomes the right child of `0`.
4. Continue this process recursively for each subtree, resulting in a balanced BST.

### Edge Cases:
1. **Empty Array**: Return `None` since there’s no element to form a tree.
2. **Single Element Array**: A single-element array results in a tree with only one node.
3. **Even Length Array**: If there are two middle elements, pick either as the root (either will work as they both keep the tree balanced).

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of elements in the array. We process each element once.
- **Space Complexity**: O(log n) for the recursive call stack, assuming a balanced tree.

### Interview Tips:
1. **Explain the Choice of Middle Element**: Emphasize that choosing the middle element each time ensures the tree remains balanced.
2. **Recursive Strategy**: Discuss how recursion divides the array into smaller subarrays, constructing balanced subtrees at each step.
3. **Complexity Discussion**: Show you understand the O(n) time complexity and the O(log n) space complexity due to recursion depth.
4. **Edge Cases**: Be ready to address the empty array, single-element array, and even-length array cases.
5. **Dry Run**: Walk through a small example like `nums = [1, 2, 3]` to show understanding.

### Practice Prompts for Interview:
1. **What happens if the array has only one element?**
2. **Why choose the middle element?**
3. **How does the solution ensure the tree remains height-balanced?**
4. **What would change if the array were not sorted?**
5. **Can you implement this without recursion?**

