# LeetCode Problem 506: Relative Ranks

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Problem Statement**:  
Given an integer array `score` of size `n` representing the scores of athletes, you need to assign ranks to them based on their scores. The rank assignment rules are as follows:

- The highest score gets "Gold Medal".
- The second-highest score gets "Silver Medal".
- The third-highest score gets "Bronze Medal".
- All other scores get their respective ranks in descending order, starting from 4.

**Inputs**:
- `score` (list of integers): An array of scores where each score is unique.

**Outputs**:
- A list of strings representing ranks corresponding to each score.

**Constraints**:
- `1 <= score.length <= 10^4`
- `0 <= score[i] <= 10^6`
- All scores are unique.

**Clarifications**:
- No ties exist since all scores are unique.
- You need to return ranks in the same order as the input array.

---

### 2. Match

- **Pattern**: Sorting and mapping ranks to elements.
- **Data Structures**:
  - Use a dictionary to map scores to their ranks.
  - Sort the scores to determine ranks efficiently.
  - Alternatively, use a heap for partially sorted data when ranks for top elements are sufficient.

---

### 3. Plan

#### Sorting-Based Plan

1. Create a sorted list of scores in descending order.
2. Assign "Gold Medal", "Silver Medal", and "Bronze Medal" to the top three scores, respectively.
3. Assign numeric ranks (`"4"`, `"5"`, etc.) to the remaining scores.
4. Create a dictionary mapping each score to its rank.
5. Use the dictionary to construct the result array corresponding to the original order of the input array.

#### Heap-Based Plan

1. Use a max-heap to extract elements in descending order.
2. Assign ranks while extracting elements.
3. Use a dictionary to map scores to ranks.
4. Construct the result array based on the input order.

**Edge Cases**:
- Only one score: Directly return `["Gold Medal"]`.
- Small input (e.g., 2 or 3 scores): Ensure medals are assigned correctly.
- Large input (e.g., `10^4` scores): Ensure efficiency in sorting or heap operations.

**Complexity**:
- Sorting-Based: O(n log n) time, O(n) space.
- Heap-Based: O(n log k) time, O(n) space (with `k = n` in this problem).

---

### 4. Implement

#### Sorting-Based Implementation

```python
def findRelativeRanks(score):
    # Step 1: Sort scores in descending order
    sorted_scores = sorted(score, reverse=True)
    
    # Step 2: Create a rank mapping
    rank_map = {}
    for i, val in enumerate(sorted_scores):
        if i == 0:
            rank_map[val] = "Gold Medal"
        elif i == 1:
            rank_map[val] = "Silver Medal"
        elif i == 2:
            rank_map[val] = "Bronze Medal"
        else:
            rank_map[val] = str(i + 1)
    
    # Step 3: Map ranks to original score order
    result = [rank_map[s] for s in score]
    return result
```

#### Heap-Based Implementation

```python
import heapq

def findRelativeRanksHeap(score):
    # Step 1: Create a max-heap with negative scores
    max_heap = [(-s, i) for i, s in enumerate(score)]
    heapq.heapify(max_heap)
    
    # Step 2: Assign ranks based on extraction order
    rank_map = {}
    rank = 1
    while max_heap:
        _, idx = heapq.heappop(max_heap)
        if rank == 1:
            rank_map[idx] = "Gold Medal"
        elif rank == 2:
            rank_map[idx] = "Silver Medal"
        elif rank == 3:
            rank_map[idx] = "Bronze Medal"
        else:
            rank_map[idx] = str(rank)
        rank += 1
    
    # Step 3: Build result array based on original order
    return [rank_map[i] for i in range(len(score))]
```

---

### 5. Review

**Test Cases**:
```python
# Test Case 1: General case
print(findRelativeRanks([5, 4, 3, 2, 1]))  # ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]

# Test Case 2: Single athlete
print(findRelativeRanks([10]))  # ["Gold Medal"]

# Test Case 3: Two athletes
print(findRelativeRanks([3, 6]))  # ["Silver Medal", "Gold Medal"]

# Test Case 4: Large array
print(findRelativeRanks([i for i in range(1000, 0, -1)]))  # ["Gold Medal", "Silver Medal", ..., "1000"]

# Heap-Based Test Case
print(findRelativeRanksHeap([5, 4, 3, 2, 1]))  # ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
```

**Dry Run Example**:
Input: `[5, 4, 3, 2, 1]`

Sorting-Based:
1. **Sorted Scores**: `[5, 4, 3, 2, 1]`
2. **Rank Mapping**:
   - `5`: "Gold Medal"
   - `4`: "Silver Medal"
   - `3`: "Bronze Medal"
   - `2`: "4"
   - `1`: "5"
3. **Output**: `["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]`

Heap-Based:
1. **Heapified Data**: `[(-5, 0), (-4, 1), (-3, 2), (-2, 3), (-1, 4)]`
2. **Extraction**:
   - Pop `-5` (idx=0): Assign "Gold Medal".
   - Pop `-4` (idx=1): Assign "Silver Medal".
   - Pop `-3` (idx=2): Assign "Bronze Medal".
   - Pop `-2` (idx=3): Assign "4".
   - Pop `-1` (idx=4): Assign "5".
3. **Output**: `["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]`.

---

### 6. Evaluate

**Complexity Analysis**:
1. **Sorting-Based**:
   - Time Complexity: O(n log n) for sorting.
   - Space Complexity: O(n) for rank mapping and result array.
2. **Heap-Based**:
   - Time Complexity: O(n log n) due to `n` heap operations.
   - Space Complexity: O(n) for heap storage and result array.

**Comparison**:
- Sorting-Based is simpler to implement and debug, making it ideal for interviews.
- Heap-Based is useful for partial sorting but does not offer an efficiency advantage here since we process all scores.
- Both approaches have similar complexities, but sorting is generally faster for full ranking scenarios due to better constant factors.

---

### Additional Notes

**Why This Problem Is Important**:
- Combines sorting with mapping, a common interview pattern.
- Tests understanding of dictionary usage for efficient lookups.
- Offers an opportunity to compare sorting and heap-based solutions.

**Prerequisites**:
- Sorting algorithms.
- Basic dictionary operations in Python.
- Heap operations and properties.

**Industry Relevance**:
- Applicable in scenarios where rankings or prioritization is needed, such as leaderboard generation in gaming or sports applications.

**Follow-Up Practice Problems**:
- LeetCode 215: Kth Largest Element in an Array.
- LeetCode 252: Meeting Rooms.
- LeetCode 1122: Relative Sort Array.
