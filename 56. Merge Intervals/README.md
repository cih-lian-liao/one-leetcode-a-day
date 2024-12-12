# LeetCode Problem 56: Merge Intervals

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

#### Problem Statement
You are given an array of intervals where each interval is represented as `[start, end]`. Your task is to merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

#### Input:
- A list of intervals, `intervals`, where each interval is an array of two integers `[start, end]`.
- Example: `[[1,3],[2,6],[8,10],[15,18]]`

#### Output:
- A list of merged intervals where overlapping intervals are combined.
- Example Output: `[[1,6],[8,10],[15,18]]`

#### Constraints:
1. `1 <= intervals.length <= 10^4`
2. `intervals[i].length == 2`
3. `0 <= start <= end <= 10^4`

---

### 2. Match

#### Problem Type
This problem involves merging overlapping ranges and can be categorized as:
- **Sorting and Iteration**

#### Key Observations:
- Intervals need to be sorted based on their starting times.
- Adjacent intervals should be merged if they overlap.

#### Suitable Data Structure:
- Use a **list** to store merged intervals for simplicity.

---

### 3. Plan

#### Steps:
1. **Sort the Intervals**:
   - Sort the input list `intervals` based on the start time of each interval.
2. **Initialize a Result List**:
   - Use an empty list `merged` to store the merged intervals.
3. **Iterate and Merge**:
   - For each interval:
     - If `merged` is empty or the last interval in `merged` does not overlap with the current interval, add the current interval to `merged`.
     - Otherwise, update the end of the last interval in `merged` to include the current interval.
4. **Return the Merged Intervals**:
   - After iterating through all intervals, return `merged`.

#### Time Complexity:
- Sorting: O(n log n)
- Merging: O(n)
- Total: O(n log n)

#### Space Complexity:
- O(n) for storing merged intervals.

---

### 4. Implement

```python
def merge(intervals):
    # Step 1: Sort the intervals by their start times
    intervals.sort(key=lambda x: x[0])
    merged = []  # Step 2: Initialize a result list

    # Step 3: Iterate through the intervals
    for interval in intervals:
        # If merged is empty or there is no overlap with the last interval
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)  # Add the interval to merged
        else:
            # Merge the current interval with the last interval in merged
            merged[-1][1] = max(merged[-1][1], interval[1])

    return merged  # Step 4: Return the merged intervals
```

---

### Implementation Notes (Step-by-Step)

1. **Sort the Intervals**:
   ```python
   intervals.sort(key=lambda x: x[0])
   ```
   Sorting ensures that intervals are processed in order of their start times, which simplifies the merging logic.

2. **Initialize Result List**:
   ```python
   merged = []
   ```
   This list will store the final merged intervals.

3. **Iterate and Check Overlap**:
   - If `merged` is empty or `merged[-1][1] < interval[0]`, it means there is no overlap. Add the interval to `merged`.
   - Otherwise, merge the intervals by updating the end time of the last interval in `merged` to the maximum of `merged[-1][1]` and `interval[1]`.

4. **Return the Result**:
   The `merged` list now contains all the merged intervals.

---

### 5. Review

#### Test Cases:
```python
# Test Case 1
print(merge([[1,3],[2,6],[8,10],[15,18]]))  # Output: [[1,6],[8,10],[15,18]]

# Test Case 2
print(merge([[1,4],[4,5]]))  # Output: [[1,5]]

# Test Case 3
print(merge([[1,3],[4,6],[7,9]]))  # Output: [[1,3],[4,6],[7,9]]

# Test Case 4
print(merge([[1,10],[2,6],[8,15]]))  # Output: [[1,15]]
```

#### Edge Cases:
1. Single Interval:
   ```python
   merge([[1, 2]])  # Output: [[1, 2]]
   ```
2. All Intervals Overlapping:
   ```python
   merge([[1, 5], [2, 6], [3, 7]])  # Output: [[1, 7]]
   ```

#### Dry Run Example:
**Input**: `[[1,3],[2,6],[8,10],[15,18]]`
- After sorting: `[[1,3],[2,6],[8,10],[15,18]]`
- Iteration 1: Add `[1,3]` to `merged`.
- Iteration 2: Merge `[1,3]` and `[2,6]` -> `[1,6]`.
- Iteration 3: Add `[8,10]` to `merged`.
- Iteration 4: Add `[15,18]` to `merged`.
- Final Output: `[[1,6],[8,10],[15,18]]`

---

### 6. Evaluate

#### Time Complexity:
- Sorting the intervals: O(n log n)
- Iterating through the intervals: O(n)
- Total: O(n log n)

#### Space Complexity:
- O(n) for the result list `merged`.

#### Optimizations:
This solution is already optimal for the problem constraints.

---

### Additional Notes

#### Why This Problem is Important:
- Teaches sorting and iteration patterns.
- Highlights the importance of understanding interval-based problems.

#### Prerequisites for Practicing:
- Knowledge of sorting algorithms.
- Familiarity with list operations in Python.

#### Industry Relevance:
- Common in scheduling problems (e.g., merging meeting times).
- Used in real-world applications like calendar management systems.

#### Follow-up Practice Problems:
1. **LeetCode 57: Insert Interval**
2. **LeetCode 986: Interval List Intersections**
3. **LeetCode 435: Non-overlapping Intervals**
4. **LeetCode 1288: Remove Covered Intervals**
