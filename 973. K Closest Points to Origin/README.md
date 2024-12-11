# LeetCode 973: K Closest Points to Origin

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate
### 1. Understand

#### Problem Statement
You are given an array of points where `points[i] = [xi, yi]` represents a point on the 2D X-Y plane. You are also given an integer `k`. The goal is to return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the X-Y plane is defined as the Euclidean distance:
```
d = sqrt((x2 - x1)^2 + (y2 - y1)^2)
```

You may return the answer in **any order**, and the answer is guaranteed to be unique.

#### Inputs:
- `points`: List of points `[[xi, yi], ...]` (2D coordinates of points).
- `k`: Integer representing the number of closest points to return.

#### Outputs:
- A list of `k` points that are closest to the origin.

#### Constraints:
- `1 <= k <= points.length <= 10^4`
- `-10^4 <= xi, yi <= 10^4`

#### Assumptions:
- You can return the points in any order.
- The points are guaranteed to be unique.

---

### 2. Match

#### Problem Type
This problem relates to:
- Sorting problems: Find the closest points by sorting the list by distance.
- Heap problems: Use a heap to efficiently maintain the closest `k` points.

#### Relevant Data Structures and Algorithms
- **Heap**: Use a max heap of size `k` to maintain the closest points efficiently.
- **Euclidean Distance**: Compute distances without the square root to optimize comparison.

---

### 3. Plan

#### Approach
1. **Calculate Distance**:
   - Compute the squared distance from each point to the origin to avoid unnecessary square root calculations.
   
2. **Use a Max Heap**:
   - Maintain a max heap of size `k` to store the `k` closest points.
   - Insert the first `k` points directly.
   - For each subsequent point:
     - If the point is closer than the current farthest point in the heap, replace it.
   
3. **Return Results**:
   - Extract all points from the heap and return them.

#### Edge Cases
- `k == points.length`: Return all points.
- Points with identical distances.

#### Time Complexity
- Building the heap: O(k)
- For the remaining `n - k` points, heap operations: O((n - k) * log k)
- Total: O(n * log k)

#### Space Complexity
- Heap size: O(k)
- Result storage: O(k)
- Total: O(k)

---

### 4. Implement

```python
import heapq
from typing import List

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        """
        Finds the k closest points to the origin using a max heap.
        """
        max_heap = []  # Initialize an empty max heap

        # Step 1: Iterate through the points
        for x, y in points:
            dist = -(x**2 + y**2)  # Use negative distance for max heap
            if len(max_heap) < k:
                # Add the point if the heap is not full
                heapq.heappush(max_heap, (dist, [x, y]))
            else:
                # Replace the farthest point if the current point is closer
                heapq.heappushpop(max_heap, (dist, [x, y]))

        # Step 2: Extract the points from the heap
        return [point for _, point in max_heap]
```

---

### 5. Review

#### Dry Run
#### Input:
```plaintext
points = [[1, 3], [-2, 2], [5, 8], [0, 1]], k = 2
```

#### Execution:
1. Initialize `max_heap = []`.
2. Process each point:
   - Point `[1, 3]`: Distance = -10, `max_heap = [(-10, [1, 3])]`.
   - Point `[-2, 2]`: Distance = -8, `max_heap = [(-10, [1, 3]), (-8, [-2, 2])]`.
   - Point `[5, 8]`: Distance = -89, not added (farthest point remains `[1, 3]`).
   - Point `[0, 1]`: Distance = -1, replaces `[-10, [1, 3]]`, `max_heap = [(-8, [-2, 2]), (-1, [0, 1])]`.

#### Result:
```plaintext
[[-2, 2], [0, 1]]
```

#### Test Cases
1. Basic input:
   - Input: `points = [[3, 3], [5, -1], [-2, 4]], k = 2`
   - Output: `[[3, 3], [-2, 4]]`
2. Single point:
   - Input: `points = [[1, 2]], k = 1`
   - Output: `[[1, 2]]`
3. All points:
   - Input: `points = [[1, 3], [2, 4]], k = 2`
   - Output: `[[1, 3], [2, 4]]`

---

### 6. Evaluate

#### Time Complexity
- O(n * log k), where `n` is the number of points and `k` is the size of the heap.

#### Space Complexity
- O(k), where `k` is the number of closest points stored in the heap.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates how to efficiently manage large data sets using heaps.
- Tests understanding of priority queues and computational geometry.

#### Prerequisites for Practicing
- Familiarity with heaps (`heapq` in Python).
- Understanding Euclidean distance calculations.
- Basic understanding of list comprehensions and iteration in Python.

#### Industry Relevance
- Used in real-world applications like clustering (k-means), geographical queries (finding nearest locations), and more.

#### Follow-up Practice Problems
1. [LeetCode 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
2. [LeetCode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
3. [LeetCode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
4. [LeetCode 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
