# LeetCode 703: Kth Largest Element in a Stream

### Problem Statement

Design a class to find the `k`th largest element in a stream. The `k`th largest element is defined as the `k`th largest element in a sorted order, not the `k`th distinct element.

Implement the `KthLargest` class:
- `KthLargest(int k, int[] nums)` initializes the object with an integer `k` and a stream of integers `nums`.
- `int add(int val)` appends the integer `val` to the stream and returns the element representing the `k`th largest element in the stream.

### Constraints
- 1 <= k <= 10^4
- 0 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- -10^4 <= val <= 10^4
- At most `10^4` calls will be made to `add`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
- **Inputs**:
  - `k`: An integer representing the rank of the largest element to find.
  - `nums`: A list of integers representing the initial stream of numbers.
  - `val`: An integer representing the new value to be added to the stream.
  
- **Outputs**:
  - Return the `k`th largest element after adding `val`.

- **Constraints**:
  - Handle large inputs efficiently.
  - Maintain a `k`-size heap for efficient `k`th largest computation.
  - Use a min-heap to keep the smallest `k` elements, where the root is the `k`th largest.

- **Clarifications**:
  - If `nums` has fewer than `k` elements, the heap grows until it reaches size `k` by adding new values.

---

### 2. Match
- **Pattern**:
  - This is a streaming problem.
  - It is best solved using a **min-heap** of size `k` to efficiently keep track of the `k` largest elements.
  
- **Relevant Data Structures**:
  - Min-heap (priority queue) using Python's `heapq`.

---

### 3. Plan
- Initialize the heap with all elements from `nums` and convert it into a min-heap using `heapq.heapify`.
- Keep the heap size to at most `k` by removing the smallest element whenever its size exceeds `k`.
- For the `add` method:
  1. Push the new value (`val`) into the heap.
  2. If the heap size exceeds `k`, remove the smallest element.
  3. Return the root of the heap as it represents the `k`th largest element.

- **Time Complexity**:
  - Initialization: O(n log k), where `n` is the size of `nums`.
  - `add` method: O(log k) per call.
  
- **Space Complexity**:
  - O(k) for the min-heap storage.

---

### 4. Implement

```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: list[int]):
        """
        Initialize the KthLargest object with k and nums.
        1. Set up the min-heap with all numbers in nums.
        2. Keep only the largest k elements in the heap.
        """
        self.min_heap = nums  # Initialize the heap with nums
        self.k = k            # Store the value of k
        heapq.heapify(self.min_heap)  # Turn nums into a valid heap
        while len(self.min_heap) > k:
            heapq.heappop(self.min_heap)  # Remove smallest elements until size = k

    def add(self, val: int) -> int:
        """
        Add a new value to the stream.
        1. Push the value into the heap.
        2. Ensure the heap size remains at most k.
        3. Return the kth largest element (heap root).
        """
        heapq.heappush(self.min_heap, val)  # Add the new value
        if len(self.min_heap) > self.k:
            heapq.heappop(self.min_heap)  # Remove the smallest element
        return self.min_heap[0]  # Root of the heap is the kth largest
```

---

### 5. Review

#### Test Case
Input:
```python
k = 3
nums = [4, 5, 8, 2]
kthLargest = KthLargest(k, nums)

# Sequence of adds
print(kthLargest.add(3))  # Returns 4
print(kthLargest.add(5))  # Returns 5
print(kthLargest.add(10)) # Returns 5
print(kthLargest.add(9))  # Returns 8
print(kthLargest.add(4))  # Returns 8
```

#### Dry Run
1. Initialization:
   - Min-heap after `heapify`: [2, 4, 8, 5].
   - After removing excess elements: [4, 5, 8].

2. Adding `3`:
   - Min-heap after push: [3, 4, 8, 5].
   - Min-heap after pop: [4, 5, 8].
   - Output: 4.

3. Adding `5`:
   - Min-heap after push: [4, 5, 8, 5].
   - Min-heap after pop: [5, 5, 8].
   - Output: 5.

---

### 6. Evaluate

#### Time Complexity
- **Initialization**: O(n log k).
- **add() method**: O(log k) per call.

#### Space Complexity
- O(k) for the heap.

---

### Additional Notes

#### Why This Problem is Important
- Demonstrates the use of heaps for streaming data.
- Teaches efficient handling of "top K" type problems.

#### Prerequisites for Practicing
- Understanding of heaps (`heapq` in Python).
- Basic knowledge of priority queues and sorting.

#### Industry Relevance
- Common in real-time systems, e.g., real-time leaderboards, recommendation systems.

#### Follow-up Practice Problems
1. LeetCode 215: Kth Largest Element in an Array
2. LeetCode 347: Top K Frequent Elements
3. LeetCode 973: K Closest Points to Origin
