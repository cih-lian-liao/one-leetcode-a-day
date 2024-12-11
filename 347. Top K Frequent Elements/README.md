

# LeetCode 347 - Top K Frequent Elements

### Problem Statement
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in **any order**.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### Problem Details
- **Input**:
  - `nums`: An array of integers. Example: `[1, 1, 1, 2, 2, 3]`
  - `k`: An integer representing the number of top frequent elements to return. Example: `2`
  
- **Output**:
  - An array of integers containing the `k` most frequent elements. Example: `[1, 2]`
  
- **Constraints**:
  - `1 <= nums.length <= 10^5`
  - `k` is always valid (`1 <= k <= number of unique elements in nums`).
  - Elements in `nums` can have negative or positive values.

#### Key Questions
1. Can the elements in the result be returned in any order? 
   - Yes, the order does not matter.
2. Are there duplicate values in `nums`?
   - Yes, and we need to count their occurrences.

---

### **2. Match**

#### Known Problem Type
- **Frequency Analysis**: This problem involves counting occurrences of elements and selecting the most frequent ones.

#### Relevant Data Structures and Algorithms
1. **Hash Map (Dictionary)**:
   - To count the frequency of elements efficiently.
2. **Heap**:
   - To keep track of the top `k` frequent elements in \(O(N \log K)\).
3. **Bucket Sort**:
   - To group elements by frequency for \(O(N)\) complexity.

---

### **3. Plan**

#### Approach: Bucket Sort

1. **Count Frequencies**:
   - Use a hash map (`Counter`) to store the frequency of each element.
   - Example: `{1: 3, 2: 2, 3: 1}` for `nums = [1, 1, 1, 2, 2, 3]`.

2. **Bucketize Frequencies**:
   - Create an array of buckets where each bucket's index represents the frequency.
   - Store elements in their respective frequency bucket.
   - Example: `[[], [3], [2], [1]]` for `nums = [1, 1, 1, 2, 2, 3]`.

3. **Collect Top `k` Frequent Elements**:
   - Traverse the buckets from the highest frequency to the lowest.
   - Gather elements until we have `k` elements in the result.

#### Pseudocode
```python
1. Create a frequency map using Counter(nums).
2. Create an empty list of buckets with length len(nums) + 1.
3. Populate the buckets based on frequency.
4. Traverse the buckets in reverse order.
5. Append elements to the result until k elements are collected.
6. Return the result.
```

#### Time and Space Complexity
- **Time Complexity**: O(n), where `n` is the length of `nums`.
- **Space Complexity**: O(n), for the frequency map and buckets.

---

### **4. Implement**

#### Bucket Sort Solution
```python
from collections import Counter
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Count the frequencies of elements
        freq_map = Counter(nums)  # {1: 3, 2: 2, 3: 1}
        
        # Step 2: Create buckets for frequencies
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in freq_map.items():
            buckets[freq].append(num)
        
        # Step 3: Collect top-k elements from the buckets
        result = []
        for freq in range(len(buckets) - 1, 0, -1):  # Traverse from high frequency to low
            for num in buckets[freq]:
                result.append(num)
                if len(result) == k:
                    return result
```

---

### **5. Review**

#### Edge Cases
1. **Single Element Array**:
   - Input: `[1], k=1`
   - Output: `[1]`
2. **All Unique Elements**:
   - Input: `[1, 2, 3, 4], k=2`
   - Output: `[1, 2]` (or any two elements)
3. **All Elements with Same Frequency**:
   - Input: `[1, 2, 3, 1, 2, 3], k=2`
   - Output: `[1, 2]` (or any two elements)

#### Dry Run
Input:
```python
nums = [1, 1, 1, 2, 2, 3]
k = 2
```

Execution:
1. **Step 1**: Frequency Map:
   ```python
   freq_map = {1: 3, 2: 2, 3: 1}
   ```
2. **Step 2**: Buckets:
   ```python
   buckets = [[], [3], [2], [1], [], [], []]
   ```
3. **Step 3**: Reverse Traversal:
   - Start from `freq = 3`, add `[1]` to `result`.
   - Move to `freq = 2`, add `[2]` to `result`.
   - Stop as `len(result) == k`.

Output:
```python
[1, 2]
```

---

### **6. Evaluate**

#### Complexity Analysis
- **Time Complexity**: O(n)
  - Counting frequencies: O(n)
  - Filling buckets: O(n)
  - Traversing buckets: O(n)
- **Space Complexity**: O(n)
  - Frequency map: O(n)
  - Buckets: O(n)

---

### Additional Notes

#### Solution Using Min-Heap

1. **Approach**:
   - Count frequencies using `Counter`.
   - Maintain a Min-Heap of size `k` to store top `k` elements.
   - Push `(frequency, element)` into the heap and remove the smallest frequency element if the heap size exceeds `k`.

2. **Implementation**:
```python
from collections import Counter
import heapq
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Count the frequencies of elements
        freq_map = Counter(nums)  # Example: {1: 3, 2: 2, 3: 1}
        
        # Step 2: Use a Min-Heap to keep the top k frequent elements
        min_heap = []
        for num, freq in freq_map.items():
            heapq.heappush(min_heap, (freq, num))  # Push (frequency, number) pair
            if len(min_heap) > k:
                heapq.heappop(min_heap)  # Remove the smallest frequency element
        
        # Step 3: Extract the results from the heap
        result = [num for freq, num in min_heap]
        return result
```

3. **Complexity**:
   - **Time Complexity**: O(n log k)
   - **Space Complexity**: O(n + k)

#### Why This Problem is Important
- Understanding frequency analysis is foundational for real-world problems like recommendation systems, text analysis, and search engines.

#### Prerequisites for Practicing This Problem
- Hash map operations (insert, update, and lookup).
- Familiarity with bucket sort and heap operations.

#### Industry Relevance
- Common in designing search engines, data analytics tools, and machine learning preprocessing pipelines.

#### Follow-up Practice Problems
1. LeetCode 692: Top K Frequent Words
2. LeetCode 451: Sort Characters By Frequency
3. LeetCode 973: K Closest Points to Origin
