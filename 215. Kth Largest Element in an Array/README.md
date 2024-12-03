# LeetCode 215: Kth Largest Element in an Array

### Problem Statement

Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.

Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.

### Constraints
- 1 <= k <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
- **Inputs**:
  - `nums`: A list of integers, potentially unsorted.
  - `k`: An integer, representing the position of the largest element to find.
  
- **Outputs**:
  - Return the `k`th largest element in the array.

- **Constraints**:
  - The array size can go up to 10^4, so the solution needs to handle large inputs efficiently.
  - Negative and positive integers are allowed in the array.

- **Clarifications**:
  - What if there are duplicates? (The solution treats duplicates as individual elements.)
  - Can we use sorting directly? (Sorting is valid but less optimal compared to other methods for large arrays.)

---

### 2. Match
- **Pattern**:
  - This is a "Top-K" problem, where we need to extract partial order information (not the full sorted array).
  - Suitable for using:
    - Sorting (brute force approach, less optimal).
    - Heap (efficient for maintaining the largest `k` elements).

- **Relevant Data Structures and Algorithms**:
  - **Min-Heap**: To maintain the `k` largest elements efficiently.
  - **Quickselect**: An alternative algorithm for finding the `k`th largest/smallest element.

---

### 3. Plan

#### Approach: Min-Heap
1. Initialize a min-heap with the first `k` elements of `nums`.
2. Heapify the array segment to form a valid min-heap.
3. Iterate over the remaining elements of `nums`:
   - If the current element is greater than the heap's root, replace the root (pop and push).
4. After processing all elements, the root of the heap is the `k`th largest element.

#### Pseudocode:
```python
min_heap = nums[:k]
heapify(min_heap)

for num in nums[k:]:
    if num > min_heap[0]:
        heappop(min_heap)
        heappush(min_heap, num)

return min_heap[0]
```

#### Time Complexity:
- Initialization: O(k) for heapify.
- Processing the remaining elements: O((n-k) log k), where n is the size of the array.
- Total: O(n log k).

#### Space Complexity:
- O(k) for the heap storage.

---

### 4. Implement

```python
import heapq

class Solution:
    def findKthLargest(self, nums: list[int], k: int) -> int:
        """
        Find the k-th largest element in the array using a min-heap.
        """
        # Step 1: Initialize the min-heap with the first k elements.
        min_heap = nums[:k]
        heapq.heapify(min_heap)  # Converts the list into a valid min-heap
        
        # Step 2: Process the rest of the array.
        for num in nums[k:]:
            if num > min_heap[0]:  # Only modify the heap if the current number is larger than the heap's root
                heapq.heappop(min_heap)  # Remove the smallest element in the heap
                heapq.heappush(min_heap, num)  # Add the new number to the heap
        
        # Step 3: The root of the heap is the k-th largest element.
        return min_heap[0]
```

#### Implementation Notes:
1. **Heap Initialization**:
   - `nums[:k]` extracts the first `k` elements from the array.
   - `heapq.heapify(min_heap)` ensures the list becomes a min-heap, where the smallest element is at the root.

2. **Iterating Through the Array**:
   - For each element in the remaining array (`nums[k:]`), compare it with the heap root.
   - If it's greater, replace the root to maintain the top `k` largest elements.

3. **Return Statement**:
   - At the end of the loop, the heap root (`min_heap[0]`) is the `k`th largest element.

---

### 5. Review

#### Test Cases:
**Example 1**:
```python
nums = [3, 2, 1, 5, 6, 4]
k = 2
solution = Solution()
assert solution.findKthLargest(nums, k) == 5
```

**Example 2**:
```python
nums = [3, 2, 3, 1, 2, 4, 5, 5, 6]
k = 4
assert solution.findKthLargest(nums, k) == 4
```

**Edge Case**:
```python
nums = [1]
k = 1
assert solution.findKthLargest(nums, k) == 1
```

#### Dry Run:
Input:
```python
nums = [3, 2, 1, 5, 6, 4]
k = 2
```
1. Initial heap: `[3, 2]` (heapify: `[2, 3]`).
2. Process remaining elements:
   - `1` (skip, smaller than `2`).
   - `5` (replace `2` → `[3, 5]`).
   - `6` (replace `3` → `[5, 6]`).
   - `4` (skip, smaller than `5`).
3. Heap root: `5`.

Output: `5`.

---

### 6. Evaluate

#### Time Complexity:
- O(n log k):
  - Heapify: O(k).
  - Processing: O((n-k) log k).

#### Space Complexity:
- O(k) for maintaining the heap.

---

### Additional Notes

#### Why This Problem is Important
- Commonly asked in coding interviews.
- Demonstrates efficient data structure usage (heap) for partial sorting.

#### Prerequisites for Practicing
- Basic understanding of heaps (`heapq` in Python).
- Familiarity with array manipulation and sorting.

#### Industry Relevance
- Real-world applications like real-time leaderboards or "Top K" search results.

#### Follow-up Practice Problems
1. LeetCode 703: Kth Largest Element in a Stream
2. LeetCode 347: Top K Frequent Elements
3. LeetCode 973: K Closest Points to Origin
