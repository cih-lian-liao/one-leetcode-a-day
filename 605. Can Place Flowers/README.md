
# LeetCode 605 - Can Place Flowers

### Problem Statement

You are given a flowerbed (an array of 0s and 1s), where 0 means an empty plot and 1 means a plot already planted with a flower. Adjacent flowers cannot be planted in neighboring plots. Determine if you can plant `n` flowers without violating the planting rule.

**Inputs:**
- `flowerbed` (List[int]): Array representing the flowerbed.
- `n` (int): Number of flowers to plant.

**Output:**
- `bool`: Return `True` if it's possible to plant all `n` flowers, otherwise `False`.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Goal**: Place `n` flowers in the flowerbed without planting adjacent flowers. Return `True` if possible, otherwise `False`.
- **Constraints**:
  - `flowerbed` length is at least 1 and at most 2 * 10^4.
  - `flowerbed` contains only 0s and 1s.
  - `n` is a non-negative integer.
- **Edge Cases**:
  - If `n == 0`, return `True`.
  - If `flowerbed` has all `0`s and is large enough, plant all flowers.
  - If `flowerbed` has no space to plant, return `False`.

---

### 2. Match

- **Pattern**: Greedy approach, as we check each plot and decide if we can plant a flower.
- **Data Structures**:
  - Use a list to track flowerbed status.
  - Count to keep track of planted flowers.

---

### 3. Plan

1. Traverse the `flowerbed`.
2. For each plot, check if it is empty (`flowerbed[i] == 0`).
   - If yes, check its neighbors:
     - If `i == 0` or `flowerbed[i - 1] == 0`, left neighbor is empty.
     - If `i == len(flowerbed) - 1` or `flowerbed[i + 1] == 0`, right neighbor is empty.
   - If both neighbors are empty, plant a flower, update the count, and mark the plot (`flowerbed[i] = 1`).
3. If the count reaches or exceeds `n` during the traversal, return `True`.
4. After traversal, return `count >= n`.

---

### 4. Implement

```python
from typing import List

class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        # Initialize a count for planted flowers
        count = 0
        
        # Traverse the flowerbed
        for i in range(len(flowerbed)):
            # Check if the current plot is empty
            if flowerbed[i] == 0:
                # Check if the left and right plots are empty
                empty_left_plot = (i == 0) or (flowerbed[i - 1] == 0)
                empty_right_plot = (i == len(flowerbed) - 1) or (flowerbed[i + 1] == 0)
                
                # If both plots are empty, plant a flower
                if empty_left_plot and empty_right_plot:
                    flowerbed[i] = 1  # Mark the plot as planted
                    count += 1       # Increment the planted flower count
                    
                    # If enough flowers are planted, return True
                    if count >= n:
                        return True
        
        # If after traversal, not enough flowers are planted, return False
        return count >= n
```

**Implementation Notes:**
- This code uses a single traversal of the array (`O(n)`).
- The inline comments provide a step-by-step explanation for clarity.

---

### 5. Review

#### Debugging:
- **Logic Check**:
  - Ensure the neighbors are checked properly.
  - Ensure that `flowerbed` is updated correctly.
  - Ensure the count is incremented only when a flower is planted.

#### Edge Case Testing:
1. **All Empty Plots**:
   - Input: `flowerbed = [0, 0, 0, 0, 0], n = 2`
   - Expected Output: `True`
   - Explanation: Flowers can be planted in indices 0 and 2.
2. **No Space to Plant**:
   - Input: `flowerbed = [1, 0, 1, 0, 1], n = 1`
   - Expected Output: `False`
   - Explanation: No empty spaces between flowers.
3. **Boundary Case**:
   - Input: `flowerbed = [0], n = 1`
   - Expected Output: `True`
   - Explanation: The single plot can accommodate one flower.

#### Dry Run:
Input: `flowerbed = [1, 0, 0, 0, 1], n = 1`
1. Start with `count = 0`.
2. Traverse the array:
   - `i = 0`: Plot is `1` (occupied), skip.
   - `i = 1`: Plot is `0`, but left plot is `1`, skip.
   - `i = 2`: Plot is `0`, left and right plots are empty. Plant a flower. Update `flowerbed = [1, 0, 1, 0, 1]`, `count = 1`.
   - `i = 3`: Plot is `0`, but left plot is `1`, skip.
   - `i = 4`: Plot is `1` (occupied), skip.
3. End of traversal: `count = 1`, which is >= `n = 1`. Return `True`.

---

### 6. Evaluate

- **Time Complexity**: `O(n)`  
  The flowerbed is traversed once.
- **Space Complexity**: `O(1)`  
  The solution modifies the `flowerbed` in place and uses only a few variables.

---

### Additional Notes

#### Importance of the Problem:
- Highlights problem-solving with greedy algorithms.
- Emphasizes edge case handling.

#### Prerequisites:
- Understanding of array traversal.
- Knowledge of greedy algorithms.

#### Industry Relevance:
- Practical scenarios like resource allocation and scheduling.
- Problem-solving techniques applicable to competitive programming and interviews.

#### Follow-up Practice Problems:
1. [LeetCode 121 - Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
2. [LeetCode 134 - Gas Station](https://leetcode.com/problems/gas-station/)
3. [LeetCode 392 - Is Subsequence](https://leetcode.com/problems/is-subsequence/)
