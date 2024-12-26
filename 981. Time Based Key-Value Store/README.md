
# LeetCode 981: Time Based Key-Value Store

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Problem Statement**  
Design a time-based key-value data structure that can store multiple values for the same key at different timestamps and retrieve the key's value at a certain timestamp.

**Requirements**:
1. Implement the class `TimeMap` with two main functions:
   - `set(key, value, timestamp)`: Stores a `value` for the given `key` and associates it with `timestamp`.
   - `get(key, timestamp)`: Retrieves the most recent value for the given `key` such that the associated timestamp is less than or equal to the provided `timestamp`. If no such value exists, return an empty string `""`.

**Inputs**:
- `key` (string), `value` (string), and `timestamp` (integer).

**Outputs**:
- For `get()`, return a string representing the value for the given `key` at or before `timestamp`.

**Constraints**:
- All timestamps for the same key are strictly increasing.
- Multiple `set()` calls for the same key are allowed.

---

### 2. Match

**Pattern Identified**:
- This problem matches the pattern of "binary search on a sorted list."
- The values for each `key` are stored with timestamps in increasing order, making binary search ideal for efficient retrieval.

**Key Data Structures**:
- A dictionary (`self.store`) where the key is the `key` string, and the value is a list of `[value, timestamp]`.

---

### 3. Plan

**Step-by-Step Plan**:
1. Use a dictionary `self.store` to store the mapping of `key` to a list of `[value, timestamp]`.
2. For `set()`: 
   - Append `[value, timestamp]` to the list for the given `key`.
3. For `get()`: 
   - If the `key` does not exist in the dictionary, return `""`.
   - Use binary search to find the largest `timestamp` that is less than or equal to the given `timestamp`.
   - Return the corresponding `value`. If no such `timestamp` exists, return `""`.

**Time Complexity**:
- `set()`: O(1) per call.
- `get()`: O(log n) per call, where `n` is the number of timestamps for the given `key`.

**Space Complexity**:
- O(k * n), where `k` is the number of keys, and `n` is the average number of timestamps per key.

---

### 4. Implement

```python
class TimeMap:
    def __init__(self):
        # Initialize the dictionary to store key: [list of [value, timestamp]]
        self.store = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        # If the key is not in the dictionary, initialize an empty list
        if key not in self.store:
            self.store[key] = []
        # Append the new value and timestamp as a pair
        self.store[key].append([value, timestamp])

    def get(self, key: str, timestamp: int) -> str:
        # Initialize result as an empty string
        res = ""
        # Retrieve the list of values for the given key, or an empty list if the key does not exist
        values = self.store.get(key, [])
        
        # Binary search for the largest timestamp <= given timestamp
        l, r = 0, len(values) - 1
        while l <= r:
            m = (l + r) // 2
            if values[m][1] <= timestamp:  # Check if mid's timestamp is <= given timestamp
                res = values[m][0]  # Update result to the current value
                l = m + 1  # Search the right half
            else:
                r = m - 1  # Search the left half
        return res
```

---

### 5. Review

#### **Dry Run Example**
```python
# Initialize TimeMap
time_map = TimeMap()

# Set key-value pairs
time_map.set("foo", "bar", 1)
time_map.set("foo", "bar2", 4)
time_map.set("foo", "bar3", 7)

# Get key-value pairs
print(time_map.get("foo", 0))  # Expected: ""
print(time_map.get("foo", 3))  # Expected: "bar"
print(time_map.get("foo", 5))  # Expected: "bar2"
print(time_map.get("foo", 7))  # Expected: "bar3"
print(time_map.get("foo", 10)) # Expected: "bar3"
```

#### Step-by-Step Explanation of `time_map.get("foo", 5)`:
1. `values = [["bar", 1], ["bar2", 4], ["bar3", 7]]`
2. Perform binary search:
   - `l = 0, r = 2, m = 1`: `values[1][1] = 4` (≤ 5), update `res = "bar2"`, set `l = 2`.
   - `l = 2, r = 2, m = 2`: `values[2][1] = 7` (> 5), set `r = 1`.
3. Exit loop and return `res = "bar2"`.

#### Test Edge Cases:
1. `get()` with a `timestamp` smaller than all stored timestamps.
2. `get()` with a `timestamp` larger than all stored timestamps.
3. `get()` for a `key` that does not exist.

---

### 6. Evaluate

**Time Complexity**:
- `set()`: O(1).
- `get()`: O(log n).

**Space Complexity**:
- O(k * n), where `k` is the number of keys, and `n` is the average number of timestamps per key.

**Optimizations**:
- This solution is already efficient, leveraging binary search for retrieval.

---

### Additional Notes

#### Why This Problem is Important:
- It tests your ability to design efficient data structures and algorithms for real-world problems involving time-based data.

#### Prerequisites:
- Familiarity with dictionaries and binary search.

#### Industry Relevance:
- Time-series data is commonly used in fields like finance, IoT, and logging systems.

#### Follow-Up Practice Problems:
1. LeetCode 732: My Calendar III
2. LeetCode 729: My Calendar I
3. LeetCode 348: Design Tic-Tac-Toe
