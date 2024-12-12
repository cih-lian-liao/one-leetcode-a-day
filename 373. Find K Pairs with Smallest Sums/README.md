# LeetCode 373: Find K Pairs with Smallest Sums

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### Problem Description
Given two sorted arrays `nums1` and `nums2` and an integer `k`, return the `k` pairs `(u1, v1), (u2, v2), ..., (uk, vk)` with the smallest sums. A pair `(u, v)` consists of one element from `nums1` and one element from `nums2`.

#### Inputs:
- Two sorted integer arrays, `nums1` and `nums2`.
- An integer `k`.

#### Outputs:
- A list of `k` pairs `(u, v)` such that their sums are among the smallest possible.

#### Constraints:
- `1 <= nums1.length, nums2.length <= 10^4`
- `-10^9 <= nums1[i], nums2[j] <= 10^9`
- `nums1` and `nums2` are sorted in ascending order.
- `1 <= k <= min(10^4, nums1.length * nums2.length)`

#### Assumptions:
- Both arrays are sorted.
- The output should include exactly `k` pairs or fewer if `k > nums1.length * nums2.length`.

---

### **2. Match**

#### Problem Type:
- **Pattern**: Priority Queue (Heap)
- **Data Structure**: Min-Heap

#### Why Min-Heap?
The heap allows us to efficiently retrieve the smallest pair sum while dynamically exploring other possible pairs.

---

### **3. Plan**

#### Approach:
1. Use a **min-heap** to store candidate pairs `(sum, i, j)`, where `sum = nums1[i] + nums2[j]`.
2. Push the initial pairs `(nums1[i], nums2[0])` for all `i` up to `min(k, len(nums1))` into the heap.
3. Repeatedly extract the smallest pair `(nums1[i], nums2[j])` from the heap:
   - Add this pair to the result list.
   - If possible, push the next pair `(nums1[i], nums2[j+1])` into the heap.
4. Stop when `k` pairs are collected or the heap is empty.

#### Pseudocode:
1. Initialize `min_heap`.
2. Push pairs `(nums1[i] + nums2[0], i, 0)` for `i in range(min(k, len(nums1)))`.
3. While `k > 0` and the heap is not empty:
   - Pop `(sum, i, j)` from the heap.
   - Append `(nums1[i], nums2[j])` to the result list.
   - If `j+1 < len(nums2)`, push `(nums1[i] + nums2[j+1], i, j+1)` to the heap.
4. Return the result.

#### Time Complexity:
- Heap insertion/extraction: O(log(min(k, n))).
- Total iterations: O(k).
- Overall: O(k * log(min(k, n))).

#### Space Complexity:
- Heap size: O(min(k, n)).

---

### **4. Implement**

```python
import heapq

def kSmallestPairs(nums1, nums2, k):
    """
    Find the k pairs with the smallest sums from two sorted arrays.
    """
    # Step 1: Edge case checks
    if not nums1 or not nums2 or k == 0:
        return []

    # Step 2: Initialize the min-heap
    min_heap = []
    for i in range(min(k, len(nums1))):  # Only push the first k elements of nums1
        heapq.heappush(min_heap, (nums1[i] + nums2[0], i, 0))  # (sum, index1, index2)

    # Step 3: Extract k pairs with smallest sums
    result = []
    while k > 0 and min_heap:
        current_sum, i, j = heapq.heappop(min_heap)  # Extract the smallest pair
        result.append((nums1[i], nums2[j]))         # Add to result list

        # Step 4: Push the next pair from nums2
        if j + 1 < len(nums2):                      # Check bounds
            heapq.heappush(min_heap, (nums1[i] + nums2[j + 1], i, j + 1))

        k -= 1

    return result
```

---

### **5. Review**

#### Example Walkthrough:
**Input:**
```python
nums1 = [1, 7, 11]
nums2 = [2, 4, 6]
k = 3
```

**Execution Steps:**
1. Initialize heap:
   - Push `(1+2, 0, 0)`, `(7+2, 1, 0)`, `(11+2, 2, 0)` into heap.
   - Heap: `[(3, 0, 0), (9, 1, 0), (13, 2, 0)]`.

2. Extract `[(1, 2)]`:
   - Push `(1+4, 0, 1)` into heap.
   - Heap: `[(5, 0, 1), (13, 2, 0), (9, 1, 0)]`.

3. Extract `[(1, 2), (1, 4)]`:
   - Push `(1+6, 0, 2)` into heap.
   - Heap: `[(7, 0, 2), (13, 2, 0), (9, 1, 0)]`.

4. Extract `[(1, 2), (1, 4), (1, 6)]`:
   - No further pushes as `j+1` exceeds bounds.

**Output:**
```python
[(1, 2), (1, 4), (1, 6)]
```

---

### **6. Evaluate**

#### Time Complexity:
- **Initialization:** O(min(k, n1)) to push initial pairs into the heap.
- **Heap Operations:** O(k * log(min(k, n1))).
- **Overall:** O(k * log(min(k, n1))).

#### Space Complexity:
- **Heap Size:** O(min(k, n1)).

---

### Additional Notes:

#### Why is This Problem Important?
- Demonstrates efficient use of heaps for managing and retrieving smallest/largest elements dynamically.
- Practical in applications involving priority retrieval, such as database queries or scheduling.

#### Prerequisites:
- Understanding of heaps (min-heap, max-heap).
- Familiarity with tuple sorting and heapq operations in Python.

#### Industry Relevance:
- Often encountered in scenarios involving merging or pairing sorted datasets efficiently, such as search engines or recommendation systems.

#### Follow-up Practice Problems:
1. LeetCode 378: Kth Smallest Element in a Sorted Matrix
2. LeetCode 215: Kth Largest Element in an Array
3. LeetCode 719: Find K-th Smallest Pair Distance
