
# LeetCode 253: Meeting Rooms II

### Problem Statement

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

**Inputs:**

- `intervals`: A list of lists where each sublist contains two integers, `[start, end]`, representing the start and end time of a meeting.

**Outputs:**

- An integer indicating the minimum number of meeting rooms required.

**Constraints:**

- ```1 <= intervals.length <= 104```
- ```0 <= starti < endi <= 106```

**Assumptions:**

- Meetings do not end at the same time they start.
- The input intervals may be unsorted.

**Key Questions:**

1. What if there are no meetings?  
   **Answer:** If the `intervals` list is empty, the output should be `0`.

2. Can meetings overlap?  
   **Answer:** Yes, meetings may overlap, which is why multiple rooms may be needed.

---

### 2. Match

This problem is related to:

- **Sweep Line Algorithm**: Process time events (start and end) to track overlapping intervals.
- **Priority Queue (Heap)**: Use a min-heap to keep track of the earliest end times.

**Why Min-Heap?**  
The min-heap efficiently tracks the next available meeting room by keeping the earliest ending meeting at the top.

---

### 3. Plan

**Steps:**

1. If `intervals` is empty, return `0`.
2. Sort the intervals by their start times.
3. Create a min-heap to store the end times of meetings.
4. Iterate through the intervals:
   - If the current meeting's start time is greater than or equal to the heap's top (earliest end time), remove the top.
   - Push the current meeting's end time into the heap.
5. The size of the heap at the end represents the minimum number of meeting rooms required.

**Edge Cases:**

1. No meetings: `[]` → Output: `0`
2. Non-overlapping meetings: `[[1, 2], [3, 4]]` → Output: `1`
3. Fully overlapping meetings: `[[1, 10], [2, 9], [3, 8]]` → Output: `3`

**Time Complexity:**  
- Sorting: ```O(nlogn)```  
- Heap operations: ```O(nlogn)```   
Total: ```O(nlogn)``` 

**Space Complexity:**  
- Min-Heap: ```O(n)``` 

---

### 4. Implement

```python
import heapq
from typing import List

class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        # Edge case: no meetings
        if not intervals:
            return 0
        
        # Step 1: Sort intervals by start time
        intervals.sort(key=lambda x: x[0])
        
        # Step 2: Initialize min-heap for meeting end times
        min_heap = []
        
        # Step 3: Process each meeting
        for interval in intervals:
            # If the current meeting starts after the earliest meeting ends, reuse the room
            if min_heap and interval[0] >= min_heap[0]:
                heapq.heappop(min_heap)
            
            # Add the current meeting's end time to the heap
            heapq.heappush(min_heap, interval[1])
        
        # Step 4: The size of the heap is the minimum number of rooms required
        return len(min_heap)
```

**Implementation Notes:**

1. Sorting is done to process meetings chronologically.
2. Min-Heap (`heapq`) maintains the end times, ensuring efficient room allocation.

---

### 5. Review

**Example Input and Output:**

#### Test Case 1:
```python
intervals = [[0, 30], [5, 10], [15, 20]]
solution = Solution()
print(solution.minMeetingRooms(intervals))  # Output: 2
```

**Explanation:**

- Meeting 1: `[0, 30]` → Requires Room 1.
- Meeting 2: `[5, 10]` → Overlaps with Room 1, so Room 2 is needed.
- Meeting 3: `[15, 20]` → Can reuse Room 2 after Meeting 2 ends.

#### Test Case 2:
```python
intervals = [[7, 10], [2, 4]]
print(solution.minMeetingRooms(intervals))  # Output: 1
```

**Dry Run (Complex Case):**

Input:
```python
intervals = [[1, 10], [2, 6], [5, 8], [9, 12]]
```

Steps:
1. Sort: `[[1, 10], [2, 6], [5, 8], [9, 12]]`
2. Min-Heap after processing:
   - `[10]` → Room 1 for `[1, 10]`
   - `[6, 10]` → Room 2 for `[2, 6]`
   - `[8, 10]` → Room 3 for `[5, 8]`
   - `[10, 12]` → Room 1 reused for `[9, 12]`
3. Final heap: `[10, 12]` → Output: `2`

---

### 6. Evaluate

**Time Complexity:**

- Sorting: ```O(nlogn)```
- Heap operations: ```O(nlogn)```
- Total: ```O(nlogn)```

**Space Complexity:**

- Min-Heap: ```O(n)```

---

### Additional Notes

**Why This Problem Is Important:**

- It is a classic interval scheduling problem, testing sorting and heap knowledge.
- Frequently asked in FAANG interviews.

**Prerequisites:**

- Sorting algorithms
- Heap operations (`heapq` in Python)

**Industry Relevance:**

- Scheduling systems like Google Calendar or Zoom meeting planner.
- Allocation problems in resource management.

**Follow-Up Practice Problems:**

1. [LeetCode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)
2. [LeetCode 252: Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)
3. [LeetCode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
