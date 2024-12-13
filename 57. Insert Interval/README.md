# LeetCode 57: Insert Interval

### Problem Statement
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]` represent the start and end of the `i`th interval, and `intervals` is sorted in ascending order by `start_i`. You are also given an interval `newInterval = [start, end]` that you need to insert into `intervals`. 

The intervals should still be sorted and non-overlapping after you insert `newInterval`.

**Example Input**:
```plaintext
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
```

**Example Output**:
```plaintext
[[1,2],[3,10],[12,16]]
```

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

#### **1. Understand**
- **Input**: A list of intervals `intervals`, where each interval is `[start, end]` (sorted and non-overlapping), and a new interval `newInterval = [start, end]`.
- **Output**: A new list of intervals that remains sorted and non-overlapping after inserting `newInterval`.
- **Constraints**:
  - `0 <= intervals.length <= 10^4`
  - `intervals[i].length == 2`
  - `intervals[i][0] <= intervals[i][1]`
  - `newInterval.length == 2`
  - `newInterval[0] <= newInterval[1]`
- **Ambiguity**:
  - If `intervals` is empty, simply return `[newInterval]`.
  - If `newInterval` overlaps multiple intervals, they should be merged into a single interval.

---

#### **2. Match**
- **Problem Type**: Interval merging and insertion.
- **Pattern**: This problem aligns with the "two-pointer" or "linear traversal" approach due to the sorted nature of the intervals.
- **Data Structures**:
  - Use a result list to collect the merged intervals.
  - Use variables to track and merge overlapping intervals.

---

#### **3. Plan**
1. Traverse the intervals using a pointer `i`.
2. Add all intervals that appear **before** `newInterval` (do not overlap).
3. Merge all intervals that **overlap** with `newInterval`.
   - Update the start and end of `newInterval` based on overlaps.
4. Add all intervals that appear **after** `newInterval` (do not overlap).
5. Return the `result` list.

**Pseudocode**:
```plaintext
Initialize result = []
Set i = 0

1. Add intervals that end before newInterval starts.
While i < intervals.length and intervals[i][1] < newInterval[0]:
    Add intervals[i] to result
    Increment i

2. Merge overlapping intervals.
While i < intervals.length and intervals[i][0] <= newInterval[1]:
    Update newInterval[0] = min(newInterval[0], intervals[i][0])
    Update newInterval[1] = max(newInterval[1], intervals[i][1])
    Increment i
Add newInterval to result

3. Add intervals that start after newInterval ends.
While i < intervals.length:
    Add intervals[i] to result
    Increment i

Return result
```

---

#### **4. Implement**

```python
def insert(intervals, newInterval):
    result = []  # Final list of merged intervals
    i = 0  # Pointer to traverse intervals

    # Step 1: Add intervals before newInterval
    while i < len(intervals) and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1

    # Step 2: Merge overlapping intervals
    while i < len(intervals) and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])  # Update start
        newInterval[1] = max(newInterval[1], intervals[i][1])  # Update end
        i += 1
    result.append(newInterval)  # Add the merged interval

    # Step 3: Add intervals after newInterval
    while i < len(intervals):
        result.append(intervals[i])
        i += 1

    return result
```

---

#### **5. Review**
**Test Case**: `intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]`

**Dry Run**:
1. Step 1: Add `[1,2]` to `result`.
   - `result = [[1,2]]`
   - `i = 1`
2. Step 2: Merge `[3,5]`, `[6,7]`, and `[8,10]` with `[4,8]`:
   - `newInterval = [3,10]`
   - `i = 4`
   - `result = [[1,2],[3,10]]`
3. Step 3: Add `[12,16]` to `result`.
   - `result = [[1,2],[3,10],[12,16]]`

**Output**:
```plaintext
[[1,2],[3,10],[12,16]]
```

---

#### **6. Evaluate**

- **Time Complexity**:
  - Each interval is processed once.
  - Complexity: O(n), where n is the number of intervals.
- **Space Complexity**:
  - Only the `result` list is used.
  - Complexity: O(1) extra space (excluding output).

---

### Additional Notes

#### **Why This Problem is Important**
- Frequently asked in coding interviews to test array traversal, merging, and condition-based logic.
- Tests understanding of edge cases like overlapping intervals, empty input, and sorting logic.

#### **Prerequisites**
- Understanding of interval merging logic.
- Familiarity with condition-based loops and list operations.

#### **Industry Relevance**
- Scheduling problems in operating systems.
- Calendar and event management applications.
- Data aggregation tasks in databases.

#### **Follow-up Practice Problems**
1. LeetCode 56: Merge Intervals
2. LeetCode 435: Non-overlapping Intervals
3. LeetCode 986: Interval List Intersections
4. LeetCode 1288: Remove Covered Intervals
