
# Problem 96. Unique Binary Search Trees

### Problem Summary

- **Goal**: Given a number `n`, calculate the number of different Binary Search Trees (BSTs) that can be built using `n` nodes labeled `1` through `n`.

---

### Approach: Dynamic Programming (DP)

**Why Dynamic Programming?**
- This problem involves finding combinations of smaller problems (subtree counts), making it perfect for dynamic programming.
- For each possible root of a tree, we can calculate the number of unique left and right subtrees independently, then multiply them to get the number of unique BSTs for that root. This approach saves time by reusing solutions to smaller subproblems.

---

### Step-by-Step Solution

1. **Set Up the DP Array**:
   - Define `dp[i]` as the number of unique BSTs that can be formed with `i` nodes.
   - We need to fill `dp` up to `dp[n]`, which will be our answer.

2. **Base Cases**:
   - `dp[0] = 1`: There's only one way to create a BST with zero nodes, which is an empty tree.
   - `dp[1] = 1`: With one node, there’s only one BST structure possible.

3. **Recursive Formula (How We Fill DP Array)**:
   - For each count of nodes from `2` to `n`, consider each node (from `1` to `nodes`) as the possible root of the BST.
   - When we choose a specific node as the root:
     - The left subtree will have `root - 1` nodes.
     - The right subtree will have `nodes - root` nodes.
   - The total number of unique BSTs for that root is the product of the unique BSTs in the left and right subtrees: `dp[left] * dp[right]`.
   - Sum up all these products over all possible root values to get the total for `dp[nodes]`.

4. **Code**:
   - Use loops to fill the `dp` array from `2` to `n`, using the recursive formula.

#### Code Implementation

```python
def numTrees(n: int) -> int:
    dp = [0] * (n + 1)  # Step 1: Initialize dp array
    dp[0], dp[1] = 1, 1  # Step 2: Set base cases

    # Step 3: Build dp array up to n
    for nodes in range(2, n + 1):  # Loop through each count of nodes
        for root in range(1, nodes + 1):  # Try each node as root
            left = dp[root - 1]  # Left subtree count
            right = dp[nodes - root]  # Right subtree count
            dp[nodes] += left * right  # Add the combinations
    
    return dp[n]  # Final answer is in dp[n]
```

---

### Example Walkthrough (`n = 3`)

1. **Starting Values**: `dp = [1, 1, 0, 0]` because `dp[0] = 1` and `dp[1] = 1`.
2. **Calculate `dp[2]`**:
   - For `root = 1`: `dp[0] * dp[1] = 1 * 1 = 1`.
   - For `root = 2`: `dp[1] * dp[0] = 1 * 1 = 1`.
   - `dp[2]` becomes `1 + 1 = 2`.

3. **Calculate `dp[3]`**:
   - For `root = 1`: `dp[0] * dp[2] = 1 * 2 = 2`.
   - For `root = 2`: `dp[1] * dp[1] = 1 * 1 = 1`.
   - For `root = 3`: `dp[2] * dp[0] = 2 * 1 = 2`.
   - `dp[3]` becomes `2 + 1 + 2 = 5`.

---

### How to Explain This in an Interview

1. **Restate the Problem**: Summarize what we’re solving—finding the number of unique BSTs for `n` nodes.

2. **Describe the Approach**:
   - Explain why dynamic programming is useful: we’re breaking the problem into smaller problems and reusing those solutions.
   - Describe the formula: For each possible root, we calculate the number of unique left and right subtrees and multiply them.

3. **Code Explanation**:
   - Go through the code, explaining the initialization of the `dp` array and each part of the loop.
   - Emphasize how, for each number of nodes, we try each node as the root and calculate the unique BSTs for each root.

4. **Complexity**:
   - **Time Complexity**: `O(n^2)` because we loop through `n` nodes and compute combinations for each root.
   - **Space Complexity**: `O(n)` since we’re storing up to `n` values in the `dp` array.

5. **Edge Cases**:
   - `n = 0` or `n = 1`: The base cases handle these directly, so our code works correctly even for small inputs.

---

### Summary

- **Key Idea**: For each count of nodes, we pick each node as the root and calculate left and right subtree possibilities separately.
- **Key Formula**: `dp[i] += dp[left] * dp[right]` for each possible root.
