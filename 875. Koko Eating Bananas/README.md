# LeetCode 875: Koko Eating Bananas

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Problem Description:**
- Koko has `n` piles of bananas, represented as an array `piles`. The i-th pile contains `piles[i]` bananas.
- She eats bananas at a fixed speed of `k` bananas per hour. If a pile has fewer bananas than `k`, she eats the entire pile.
- Koko wants to finish all bananas in `h` hours or less. Find the minimum speed `k` she needs to achieve this.

**Inputs:**
- `piles`: List of integers, where each element is the number of bananas in a pile.
- `h`: Integer, the maximum number of hours allowed.

**Output:**
- Integer, the minimum speed `k`.

**Constraints:**
- 1 <= piles[i] <= 10^9
- 1 <= h <= 10^9
- Length of `piles`: 1 <= n <= 10^4

**Assumptions:**
- Koko can eat at least one banana per hour.
- k is a positive integer.

---

### 2. Match

- **Problem Type:** Binary Search (on the speed `k`)
- **Known Pattern:** We use binary search when we need to find the minimum or maximum of a range that satisfies certain conditions. Here, we search for the smallest `k` that lets Koko finish within `h` hours.
- **Key Operations:**
  - Calculating the hours needed to eat all bananas at a given speed `k`.
  - Using `math.ceil(p / k)` for upward rounding when calculating hours per pile.

---

### 3. Plan

**Algorithm:**
1. Initialize `l` (minimum speed) to 1 and `r` (maximum speed) to the size of the largest pile, `max(piles)`.
2. Use binary search to narrow down the range of `k`:
   - Compute `k = (l + r) // 2`.
   - Calculate the total hours needed to eat all piles at speed `k`.
   - If the total hours `<= h`, update the result and reduce the upper bound `r`.
   - Otherwise, increase the lower bound `l`.
3. Continue until the bounds converge.
4. Return the result.

**Edge Cases:**
- Single pile of bananas.
- Maximum speed (`k = max(piles)`).
- Minimum speed (`k = 1`).

**Time Complexity:**
- Calculating hours takes O(n), and binary search involves O(log max(piles)).
- Total complexity: O(n log max(piles)).

**Space Complexity:**
- O(1), as no extra data structures are used.

---

### 4. Implement

Here is the Python implementation:

```python
from typing import List
import math

class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # Initialize the binary search bounds
        l, r = 1, max(piles)
        res = r  # Start with the maximum possible speed

        # Binary search for the optimal speed
        while l <= r:
            k = (l + r) // 2  # Middle point (current speed to test)
            hours = 0

            # Calculate total hours needed at speed k
            for p in piles:
                hours += math.ceil(p / k)

            # Check if hours is within limit
            if hours <= h:
                res = min(res, k)  # Update result to the current speed
                r = k - 1  # Try a slower speed
            else:
                l = k + 1  # Increase speed

        return res
```

**Notes for Implementation:**
- Use `math.ceil` to calculate the ceiling of `p / k`, which ensures Koko finishes a pile even if `p` is smaller than `k`.
- Binary search focuses on minimizing `k`, so adjust the bounds `l` and `r` accordingly.

---

### 5. Review

**Test Cases:**

#### Example 1:
Input:
```python
piles = [3, 6, 7, 11]
h = 8
```
Output:
```python
4
```
**Explanation:**
- At speed 4:
  - Pile 3: ceil(3 / 4) = 1 hour
  - Pile 6: ceil(6 / 4) = 2 hours
  - Pile 7: ceil(7 / 4) = 2 hours
  - Pile 11: ceil(11 / 4) = 3 hours
- Total: 1 + 2 + 2 + 3 = 8 hours.

#### Example 2:
Input:
```python
piles = [30, 11, 23, 4, 20]
h = 5
```
Output:
```python
30
```

#### Complex Case:
Input:
```python
piles = [1000000000]
h = 1000000000
```
Output:
```python
1
```

### Dry Run

#### Input:
```python
piles = [3, 6, 7, 11]
h = 8
```

1. Initialize: `l = 1, r = 11, res = 11`.
2. Binary Search:
   - k = 6, hours = 1 + 1 + 2 + 2 = 6. Update: `res = 6, r = 5`.
   - k = 3, hours = 1 + 2 + 3 + 4 = 10. Update: `l = 4`.
   - k = 4, hours = 1 + 2 + 2 + 3 = 8. Update: `res = 4, r = 3`.
3. Result: `4`.

---

### 6. Evaluate

**Time Complexity:**
- Binary search: O(log max(piles)).
- Each iteration computes hours: O(n).
- Total: O(n log max(piles)).

**Space Complexity:**
- Constant space: O(1).

### Additional Notes

#### Why This Problem Is Important
- Teaches binary search on non-traditional data (numerical ranges).
- Highlights practical use of mathematical ceiling operations.

#### Prerequisites for Practicing This Problem
- Understanding binary search fundamentals.
- Familiarity with ceiling operations and basic math functions.

#### Industry Relevance
- This type of problem-solving is common in optimization tasks, like resource allocation and scheduling.

#### Follow-up Practice Problems
- [LeetCode 410: Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)
- [LeetCode 1011: Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)
