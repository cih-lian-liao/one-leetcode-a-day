
# LeetCode 1046: Last Stone Weight

### Problem Statement

We are given a collection of stones, each with a positive integer weight. In each turn, we choose the two heaviest stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:

- If `x == y`, both stones are destroyed.
- If `x != y`, the stone with weight `y` is destroyed, and the stone with weight `y - x` remains.

The process continues until there is at most one stone left. Return the weight of the last remaining stone. If no stones remain, return 0.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs**:  
- An array of integers `stones` where each element is a positive integer representing the weight of a stone.  
- Length of the array: 1 <= `stones.length` <= 30.  
- Each weight: 1 <= `stones[i]` <= 1000.  

**Outputs**:  
- An integer representing the weight of the last remaining stone. Return `0` if no stones remain.

**Constraints**:  
- Only two heaviest stones are smashed in each turn.  
- The process continues until one or no stones are left.  

---

### 2. Match

**Problem Type**:  
This problem involves repeatedly extracting the two largest elements, performing an operation, and reinserting the result.  

**Relevant Patterns**:  
- **Heap (Priority Queue)**: A max heap efficiently handles extracting the largest elements.  
- **Simulation**: This is a simulation problem where you mimic the given process.  

---

### 3. Plan

**Steps**:  
1. Convert the `stones` array into a max heap. Python’s `heapq` library provides a min heap, so we negate the weights to simulate a max heap.  
2. Repeat the following steps while there are at least two stones:  
   - Extract the two largest stones.  
   - If their weights are equal, both are destroyed.  
   - If their weights are different, compute the difference and insert it back into the heap.  
3. If one stone remains, return its weight (convert it back to positive). Otherwise, return `0`.  

**Pseudocode**:
```
1. Create max_heap = [-stone for stone in stones]
2. Heapify max_heap
3. While len(max_heap) > 1:
       stone1 = -heappop(max_heap)
       stone2 = -heappop(max_heap)
       If stone1 != stone2:
           Push -(stone1 - stone2) into max_heap
4. Return -max_heap[0] if max_heap else 0
```

**Time Complexity**:  
O(n log n), where `n` is the length of the `stones` array. Each extraction and insertion takes O(log n), and we repeat this up to `n` times.  

**Space Complexity**:  
O(n) for the heap storage.

---

### 4. Implement

```python
import heapq

def lastStoneWeight(stones):
    """
    Simulate the process of smashing stones until one or no stones remain.
    :param stones: List[int]
    :return: int
    """
    # Convert stones array into a max heap by negating all weights
    max_heap = [-stone for stone in stones]
    heapq.heapify(max_heap)
    
    # Continue until one or no stones remain
    while len(max_heap) > 1:
        # Extract two heaviest stones
        stone1 = -heapq.heappop(max_heap)
        stone2 = -heapq.heappop(max_heap)
        
        # If weights are not equal, push the difference back into the heap
        if stone1 != stone2:
            heapq.heappush(max_heap, -(stone1 - stone2))
    
    # Return the last remaining stone's weight or 0 if no stones remain
    return -max_heap[0] if max_heap else 0
```

**Implementation Notes**:  
- Use Python’s `heapq` library for heap operations.  
- Negate values to simulate a max heap, as Python only provides a min heap.  
- Carefully handle the case where no stones remain.

---

### 5. Review

#### Example Dry Run

**Input**: `stones = [2,7,4,1,8,1]`  
**Execution**:
1. Initialize `max_heap = [-8, -7, -4, -1, -2, -1]` (heapified).
2. Pop `8` and `7` → Smash → Push `1` → Heap becomes `[-4, -2, -1, -1, -1]`.
3. Pop `4` and `2` → Smash → Push `2` → Heap becomes `[-2, -1, -1, -1]`.
4. Pop `2` and `1` → Smash → Push `1` → Heap becomes `[-1, -1, -1]`.
5. Pop `1` and `1` → Both destroyed → Heap becomes `[-1]`.
6. Return `1`.

**Output**: `1`

**Edge Case**:
- **Input**: `stones = [1]`  
  **Output**: `1` (only one stone, directly returned).  
- **Input**: `stones = [10, 10]`  
  **Output**: `0` (both destroyed).

---

### 6. Evaluate

**Time Complexity**:  
O(n log n), where `n` is the length of the `stones` array.  

**Space Complexity**:  
O(n) for the heap storage.  

---

### Additional Notes

**Why This Problem is Important**:  
- Builds familiarity with heap operations, a fundamental concept in algorithms.  
- Common in simulation-type problems in real-world applications.  

**Prerequisites**:  
- Understanding of heaps and priority queues.  
- Familiarity with Python’s `heapq` library.  

**Industry Relevance**:  
- Priority queues are widely used in scheduling, resource allocation, and simulations.  

**Follow-up Practice Problems**:  
1. [LeetCode 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)  
2. [LeetCode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)  
3. [LeetCode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)  
