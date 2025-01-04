
# LeetCode 207: Course Schedule

### Problem Statement

You are given `numCourses` courses labeled from `0` to `numCourses - 1` and a list of prerequisites `prerequisites` where `prerequisites[i] = [a, b]` indicates that you must take course `b` before course `a`.

Return `true` if you can finish all courses, otherwise return `false`.

### Example 1:
```plaintext
Input: numCourses = 2, prerequisites = [[1, 0]]
Output: true
```

### Example 2:
```plaintext
Input: numCourses = 2, prerequisites = [[1, 0], [0, 1]]
Output: false
```

### Constraints:
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= a, b < numCourses`
- All pairs `[a, b]` are unique.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### **1. Understand**
- **Inputs**:
  - `numCourses`: Total number of courses.
  - `prerequisites`: List of prerequisite pairs `[a, b]` where course `b` must be taken before course `a`.

- **Outputs**:
  - Return `true` if all courses can be completed.
  - Return `false` if there’s a cycle in prerequisites.

- **Constraints**:
  - Handle large graphs efficiently, as `numCourses` can be up to `2000`.

- **Clarifications**:
  - The prerequisites form a **directed graph**. Solving this is equivalent to checking if the graph is a Directed Acyclic Graph (DAG).

---

### **2. Match**
- This is a graph traversal problem.
- Known patterns:
  - **Cycle detection** in a directed graph.
  - Can be solved using:
    - **Topological Sort**: Kahn’s Algorithm.
    - **DFS-based cycle detection**.

---

### **3. Plan**

#### **Solution 1: Topological Sort (Kahn's Algorithm)**

1. Represent the courses as a **graph** using an adjacency list.
2. Count the **in-degree** of each node (number of edges pointing to it).
3. Use a queue to process nodes with `in-degree = 0`.
4. Process each node in the queue:
   - Remove its outgoing edges (decrement the in-degree of its neighbors).
   - If a neighbor’s in-degree becomes `0`, add it to the queue.
5. If all nodes are processed, return `true`. Otherwise, return `false`.

#### **Solution 2: DFS Cycle Detection**

1. Represent the courses as a **graph** using an adjacency list.
2. Use a `visited` array:
   - `0`: Not visited.
   - `1`: Visiting (part of the current path).
   - `-1`: Fully processed.
3. Perform DFS on each node:
   - If a node is marked `1` during DFS, a cycle is detected.
   - Mark nodes as `-1` once fully processed.
4. If all nodes can be processed without detecting a cycle, return `true`.

---

### **4. Implement**

#### **Solution 1: Topological Sort**

```python
from collections import deque, defaultdict

def canFinish(numCourses, prerequisites):
    graph = defaultdict(list)
    indegree = [0] * numCourses

    # Build graph and in-degree array
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        indegree[course] += 1

    # Start with courses having no prerequisites
    queue = deque([i for i in range(numCourses) if indegree[i] == 0])
    processed = 0

    while queue:
        current = queue.popleft()
        processed += 1
        for neighbor in graph[current]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)

    return processed == numCourses
```

#### **Solution 2: DFS Cycle Detection**

```python
def canFinish(numCourses, prerequisites):
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[prereq].append(course)

    visited = [0] * numCourses

    def dfs(course):
        if visited[course] == 1:  # Cycle detected
            return False
        if visited[course] == -1:  # Already processed
            return True

        visited[course] = 1  # Mark as visiting
        for neighbor in graph[course]:
            if not dfs(neighbor):
                return False
        visited[course] = -1  # Mark as processed
        return True

    for i in range(numCourses):
        if not dfs(i):
            return False
    return True
```

---

### **5. Review**

#### **Example Test Case**

For `numCourses = 4, prerequisites = [[1, 0], [2, 1], [3, 2]]`:

- **Topological Sort**:
  - Queue starts with `[0]`.
  - Process `0`, add `1` to queue.
  - Process `1`, add `2` to queue.
  - Process `2`, add `3` to queue.
  - Process `3`. All nodes processed.
  - Output: `true`.

- **DFS**:
  - Start DFS from `0`, explore `1 -> 2 -> 3`.
  - Mark `3, 2, 1, 0` as processed.
  - No cycle detected.
  - Output: `true`.

#### **Edge Cases**
1. `numCourses = 1, prerequisites = []`: Single course, no dependencies.
2. `numCourses = 2, prerequisites = [[1, 0], [0, 1]]`: Cycle exists, output `false`.
3. Large input with no cycles: Should handle efficiently.

---

### **6. Evaluate**

#### **Time Complexity**
- **Topological Sort**:
  - O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **DFS**:
  - O(V + E), as every node and edge is visited once.

#### **Space Complexity**
- **Topological Sort**:
  - O(V + E) for graph and in-degree array.
- **DFS**:
  - O(V + E) for graph and recursion stack.

---

### Additional Notes

#### **The Reason Why This Problem is Important**
- Teaches graph traversal techniques applicable in scheduling and dependency resolution.

#### **Prerequisites for Practicing This Problem**
- Understanding of graphs.
- Basics of BFS and DFS.

#### **Industry Relevance**
- Dependency management in software.
- Course scheduling systems.

#### **Follow-up Practice Problems**
- [LeetCode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
- [LeetCode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
- [LeetCode 269: Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
