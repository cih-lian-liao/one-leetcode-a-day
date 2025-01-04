# LeetCode 133: Clone Graph

### Problem Statement
You are given a reference of a node in a **connected undirected graph**. The graph is represented in the following way:

- Each node has a unique value.
- You are provided a reference to a node.
- You need to return a **deep copy** (clone) of the graph.

The graph is connected and undirected.

### Example

#### Input:

A graph structured as:
```
1 - 2
|   |
4 - 3
```

Adjacency List:
```
Node 1: [2, 4]
Node 2: [1, 3]
Node 3: [2, 4]
Node 4: [1, 3]
```

#### Output:
The deep copy of the same graph structure.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**:
  - A reference node in a connected undirected graph.

- **Outputs**:
  - A deep copy of the input graph.

- **Constraints**:
  - The graph is connected and undirected.
  - Node values are unique.
  - Maximum number of nodes: `100`.

- **Ambiguities Clarified**:
  - Deep copy means each node in the new graph must have new instances and should not share memory with the original graph.
  - The graph can be empty (input `None`).

---

### 2. Match

This problem can be matched with **Graph Traversal** techniques:

- **Graph Traversal**: Use either Depth First Search (DFS) or Breadth First Search (BFS).
- Use a dictionary to map original nodes to their clones.

---

### 3. Plan

#### Steps:

1. **Edge Case**:
   - If the input node is `None`, return `None`.

2. **Initialize**:
   - Use a dictionary `visited` to map original nodes to their clones.

3. **Traversal**:
   - Implement DFS or BFS to traverse the graph.
   - For each node:
     - If the node is already cloned, return the cloned node.
     - Otherwise, create a clone and store it in the `visited` dictionary.
     - Recursively (or iteratively for BFS) clone all its neighbors.

4. **Return the Cloned Graph**:
   - Return the cloned reference node.

#### Time Complexity:
- O(N + E): Traverses all nodes and edges once.

#### Space Complexity:
- O(N): Stores all nodes in the `visited` dictionary.

---

### 4. Implement

#### Python Code (DFS Solution):
```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node: 'Node') -> 'Node':
    if not node:
        return None

    visited = {}  # To store cloned nodes

    def dfs(node):
        if node in visited:
            return visited[node]

        # Clone the node
        clone = Node(node.val)
        visited[node] = clone

        # Clone the neighbors
        for neighbor in node.neighbors:
            clone.neighbors.append(dfs(neighbor))

        return clone

    return dfs(node)
```

#### BFS Solution:
```python
from collections import deque

def cloneGraph(node: 'Node') -> 'Node':
    if not node:
        return None

    visited = {}
    visited[node] = Node(node.val)
    queue = deque([node])

    while queue:
        current = queue.popleft()

        for neighbor in current.neighbors:
            if neighbor not in visited:
                visited[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            visited[current].neighbors.append(visited[neighbor])

    return visited[node]
```

---

### 5. Review

#### Edge Cases:
- Empty graph (input `None`).
- Single node without neighbors.
- Fully connected graph.

#### Dry Run:

Input:
```
Node 1: [2, 4]
Node 2: [1, 3]
Node 3: [2, 4]
Node 4: [1, 3]
```

Steps:
1. Start at Node 1. Create a clone for Node 1.
2. Visit neighbors 2 and 4. Create clones for them and connect to Node 1's clone.
3. Move to Node 2. Visit its neighbors 1 and 3. Node 1 is already cloned, so connect Node 2's clone to Node 1's clone. Create a clone for Node 3.
4. Repeat this process until all nodes are visited and cloned.
5. Return the clone of Node 1.

---

### 6. Evaluate

#### Time Complexity:
- O(N + E): Each node and edge is processed once.

#### Space Complexity:
- O(N): Storing cloned nodes in the `visited` dictionary.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Understanding graph traversal and cloning is crucial for tackling problems in networking, social graphs, and tree cloning.

#### Prerequisites for Practicing This Problem
- Basic understanding of graphs.
- Familiarity with DFS and BFS algorithms.
- Knowledge of hash maps/dictionaries in Python.

#### Industry Relevance
- Used in applications such as:
  - Social network data duplication.
  - Creating backups of network configurations.
  - Simulating graph-like data structures.

#### Follow-up Practice Problems
- [LeetCode 207: Course Schedule](https://leetcode.com/problems/course-schedule/)
- [LeetCode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
- [LeetCode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)
