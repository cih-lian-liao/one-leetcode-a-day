# LeetCode 286: Walls and Gates

### Problem Statement
You are given an `m x n` grid `rooms` initialized with the following values:

- `-1` represents a wall or an obstacle.
- `0` represents a gate.
- `2147483647` represents an empty room.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, leave it as `2147483647`.

The distance is calculated as the number of steps you must take to reach the gate.

### Example:
**Input:**
```
[
  [2147483647, -1, 0, 2147483647],
  [2147483647, 2147483647, 2147483647, -1],
  [2147483647, -1, 2147483647, -1],
  [0, -1, 2147483647, 2147483647]
]
```

**Output:**
```
[
  [3, -1, 0, 1],
  [2, 2, 1, -1],
  [1, -1, 2, -1],
  [0, -1, 3, 4]
]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Problem Details
- The grid represents a map of rooms, gates, and walls.
- Goal: For each empty room, calculate the shortest distance to a gate.
- Obstacles (`-1`) block movement.
- The result should modify the `rooms` grid in-place.

#### Input
- A 2D list `rooms` of dimensions `m x n`.

#### Output
- The modified `rooms` grid where each empty room is updated with the shortest distance to a gate.

#### Constraints
- `m, n >= 1`
- Grid values: `-1`, `0`, or `2147483647`
- A gate is always reachable if it exists.

---

### 2. Match

#### Related Problem Patterns
- Multi-source shortest path problem.
- Breadth-First Search (BFS) is suitable since it calculates the shortest distance layer by layer.

#### Algorithm and Data Structures
- **BFS** starting from all gates.
- Use a queue to store coordinates of gates.
- Explore neighbors using a directional array.

---

### 3. Plan

#### Steps
1. Parse the grid and find all gate positions.
2. Initialize a queue with these gate positions.
3. Perform BFS:
   - For each position, explore all four possible directions.
   - If a neighboring cell is an empty room (`2147483647`), update its value with the current distance + 1 and add it to the queue.
4. Continue until all reachable rooms are processed.

#### Time Complexity
- O(m * n): Each cell is visited at most once.

#### Space Complexity
- O(m * n): Queue can grow up to the size of the grid.

---

### 4. Implement

```python
from collections import deque

def walls_and_gates(rooms):
    if not rooms or not rooms[0]:
        return

    m, n = len(rooms), len(rooms[0])
    queue = deque()

    # Step 1: Initialize the queue with all gate positions
    for i in range(m):
        for j in range(n):
            if rooms[i][j] == 0:
                queue.append((i, j))

    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    # Step 2: BFS to calculate distances
    while queue:
        x, y = queue.popleft()
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            # Step 3: Check boundaries and valid cells
            if 0 <= nx < m and 0 <= ny < n and rooms[nx][ny] == 2147483647:
                rooms[nx][ny] = rooms[x][y] + 1
                queue.append((nx, ny))
```

#### Implementation Notes
1. **Queue Initialization:**
   - Add all gate positions to the queue first to process all gates simultaneously.
2. **Boundary Check:**
   - Ensure the new positions are within grid boundaries.
3. **Update Distance:**
   - Update the empty room with the shortest distance from a gate.

---

### 5. Review

#### Debugging and Testing

**Edge Cases:**
1. Empty grid:
   ```python
   rooms = []
   walls_and_gates(rooms)
   print(rooms)  # Output: []
   ```

2. No gates:
   ```python
   rooms = [[2147483647, -1, 2147483647]]
   walls_and_gates(rooms)
   print(rooms)  # Output: [[2147483647, -1, 2147483647]]
   ```

3. Fully blocked:
   ```python
   rooms = [[-1, -1, -1]]
   walls_and_gates(rooms)
   print(rooms)  # Output: [[-1, -1, -1]]
   ```

**Complex Case:**
Input:
```
[
  [0, 2147483647, -1, 2147483647],
  [2147483647, -1, 2147483647, -1],
  [-1, 0, 2147483647, 2147483647]
]
```

Output:
```
[
  [0, 1, -1, 2],
  [1, -1, 1, -1],
  [-1, 0, 1, 2]
]
```

### Step-by-Step Dry Run
1. Initialize queue: `[(0, 0), (2, 1)]`
2. Process `(0, 0)`:
   - Update `(0, 1)` to `1`.
   - Update `(1, 0)` to `1`.
3. Process `(2, 1)`:
   - Update `(1, 2)` to `1`.
   - Update `(2, 2)` to `1`.
   - Continue BFS until queue is empty.

---

### 6. Evaluate

#### Time Complexity
- O(m * n): Each cell is processed once during BFS.

#### Space Complexity
- O(m * n): Queue size is proportional to the grid size in the worst case.

---

### Additional Notes

#### The Reason Why This Problem is Important
- It reinforces BFS concepts and multi-source shortest path algorithms.
- Common in scenarios like maze solving or pathfinding in grid-based games.

#### Prerequisites for Practicing This Problem
- Understanding BFS and grid traversal.
- Familiarity with handling edge cases in 2D grids.

#### Industry Relevance
- Pathfinding algorithms are widely used in game development and navigation systems.
- Grid-based problems are common in technical interviews.

#### Follow-up Practice Problems
1. LeetCode 542: 01 Matrix
2. LeetCode 994: Rotting Oranges
3. LeetCode 785: Is Graph Bipartite?

