
# LeetCode 252: Meeting Rooms

### Problem Statement

Given an array of meeting time intervals where each interval is represented as `[start, end]`, determine if a person can attend all meetings.

### **Input**
- An array of intervals `intervals`, where each interval is a list `[start, end]`:
  - `start` is the start time of the meeting (integer).
  - `end` is the end time of the meeting (integer).
  
### **Output**
- Return `True` if the person can attend all meetings, otherwise return `False`.

### **Constraints**
- Each `start` and `end` time is a non-negative integer.
- The array may contain zero or more intervals.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand
#### Key Points:
1. A meeting overlaps with another if:
   - The end time of one meeting is greater than the start time of the next.
2. The goal is to determine if there is any overlap in the meetings.

#### Inputs and Outputs:
- **Input Example**: `[[0, 30], [5, 10], [15, 20]]`
- **Output Example**: `False` (because `[0, 30]` overlaps with `[5, 10]`).

#### Assumptions:
- Intervals are valid; `start < end` for each interval.
- The list can be empty. In this case, return `True` since there are no meetings.

---

### 2. Match
#### Pattern:
- This problem involves checking for overlapping intervals, which is a **sorting and comparison problem**.

#### Algorithm:
- Sort the intervals by their start times.
- Iterate through the sorted intervals and compare the `end` time of the current meeting with the `start` time of the next meeting.

---

### 3. Plan
#### Steps:
1. **Sort the intervals** based on their `start` times. Sorting ensures that overlapping intervals will be adjacent in the array.
2. **Iterate through the intervals**:
   - Compare the `end` time of the current interval with the `start` time of the next interval.
   - If there is overlap (`current.end > next.start`), return `False`.
3. If no overlaps are found, return `True`.

#### Pseudocode:
```
Sort intervals by start time
For i from 1 to length of intervals:
    If intervals[i - 1].end > intervals[i].start:
        Return False
Return True
```

#### Time Complexity:
- Sorting: O(n log n)
- Iteration: O(n)
- **Overall**: O(n log n)

#### Space Complexity:
- Sorting is in-place, so **O(1)**.

---

### 4. Implement
```python
def canAttendMeetings(intervals):
    """
    Determines if a person can attend all meetings.

    Parameters:
    intervals (List[List[int]]): List of meeting intervals.

    Returns:
    bool: True if all meetings are non-overlapping, False otherwise.
    """
    # Step 1: Sort intervals by start time
    intervals.sort(key=lambda x: x[0])

    # Step 2: Iterate through intervals to check for overlaps
    for i in range(1, len(intervals)):
        # If the current meeting's end time is greater than the next meeting's start time, return False
        if intervals[i - 1][1] > intervals[i][0]:
            return False

    # Step 3: If no overlaps, return True
    return True
```

---

### 5. Review
#### Dry Run:
Input: `[[0, 30], [5, 10], [15, 20]]`

1. **Sort Intervals**:
   - Input: `[[0, 30], [5, 10], [15, 20]]`
   - Sorted: `[[0, 30], [5, 10], [15, 20]]` (Already sorted by `start` time).

2. **Iterate Through Intervals**:
   - `i = 1`: Compare `intervals[0][1] (30)` with `intervals[1][0] (5)`.
     - `30 > 5`, overlap found. Return `False`.

**Output**: `False`

#### Test Cases:
```python
# Example 1: Overlapping intervals
print(canAttendMeetings([[0, 30], [5, 10], [15, 20]]))  # Output: False

# Example 2: Non-overlapping intervals
print(canAttendMeetings([[7, 10], [2, 4]]))  # Output: True

# Example 3: Empty intervals
print(canAttendMeetings([]))  # Output: True

# Example 4: Single interval
print(canAttendMeetings([[5, 8]]))  # Output: True

# Example 5: Complex case
print(canAttendMeetings([[1, 5], [10, 15], [5, 10], [0, 1]]))  # Output: True
```

---

### 6. Evaluate
#### Complexity Analysis:
- **Time Complexity**:
  - Sorting: O(n log n)
  - Iteration: O(n)
  - **Overall**: O(n log n)
- **Space Complexity**: O(1) (in-place sorting).

#### Optimization:
- The solution is already optimal given the sorting requirement.

---

### Additional Notes
### Why This Problem is Important
- Interval problems are common in scheduling, networking, and system design.

### Industry Relevance
- Used in real-world systems like Google Calendar and task schedulers.

### Prerequisites
- Understanding of sorting and comparisons.
- Familiarity with array traversal.

### Follow-Up Practice Problems
1. **LeetCode 56: Merge Intervals**
2. **LeetCode 57: Insert Interval**
3. **LeetCode 253: Meeting Rooms II**
4. **LeetCode 435: Non-overlapping Intervals**
