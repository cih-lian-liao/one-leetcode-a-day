# LeetCode 264: Ugly Number II

### Problem Statement

Find the nth Ugly Number. Ugly Numbers are positive numbers whose only prime factors are 2, 3, or 5. The sequence starts with 1, and each subsequent Ugly Number is generated by multiplying 2, 3, or 5 with an earlier Ugly Number.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### 1. Understand

- **Input**: An integer n (1 ≤ n ≤ 1690).  
- **Output**: The nth Ugly Number.  
- **Constraints**:  
  - The prime factors of Ugly Numbers are limited to 2, 3, and 5.  
  - n is guaranteed to be valid within the range.  

**Clarifications**:  
- Ugly Numbers are generated by repeatedly multiplying 2, 3, and 5 with existing Ugly Numbers.  
- The sequence starts with 1: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, ...].

---

### 2. Match

- This problem involves generating a sequence of numbers in increasing order based on a specific rule. The most relevant pattern is Dynamic Programming with Multiple Pointers.  
- **Data Structure**:  
  - An array `dp` to store the first n Ugly Numbers.  
  - Three pointers (`p2`, `p3`, `p5`) to track the next multiplier for 2, 3, and 5.

---

### 3. Plan

1. **Initialize**:  
   - Create an array `dp` of size n to store the first n Ugly Numbers.  
   - Set the first Ugly Number `dp[0] = 1`.  
   - Use three pointers `p2`, `p3`, `p5` to track the indices for the next multiplication with 2, 3, and 5.  

2. **Iterate from i = 1 to n-1**:  
   - Calculate the next Ugly Number:  
     `next_ugly = min(dp[p2] * 2, dp[p3] * 3, dp[p5] * 5)`.  
   - Store `next_ugly` in `dp[i]`.  
   - Update the pointers:  
     - If `next_ugly == dp[p2] * 2`, increment `p2`.  
     - If `next_ugly == dp[p3] * 3`, increment `p3`.  
     - If `next_ugly == dp[p5] * 5`, increment `p5`.  

3. **Return**:  
   - Return `dp[n-1]`.  

**Time Complexity**: O(n)  
**Space Complexity**: O(n)  

---

### 4. Implement

```python
def nthUglyNumber(n):
    dp = [0] * n  # Array to store Ugly Numbers
    dp[0] = 1     # First Ugly Number is 1
    p2 = p3 = p5 = 0  # Pointers for multipliers of 2, 3, and 5

    for i in range(1, n):
        next_ugly = min(dp[p2] * 2, dp[p3] * 3, dp[p5] * 5)
        dp[i] = next_ugly

        if next_ugly == dp[p2] * 2:
            p2 += 1
        if next_ugly == dp[p3] * 3:
            p3 += 1
        if next_ugly == dp[p5] * 5:
            p5 += 1

    return dp[-1]
```

---

### 5. Review

#### Example Dry Run

**Input**: n = 10  

**Initialization**:  
- `dp = [1, 0, 0, 0, 0, 0, 0, 0, 0, 0]`  
- `p2 = 0, p3 = 0, p5 = 0`  

**Iterations**:  
1. Calculate `next_ugly = min(1*2, 1*3, 1*5) = 2`. Update:  
   - `dp = [1, 2, 0, 0, 0, 0, 0, 0, 0, 0]`  
   - Increment `p2 = 1`.  
2. Calculate `next_ugly = min(2*2, 1*3, 1*5) = 3`. Update:  
   - `dp = [1, 2, 3, 0, 0, 0, 0, 0, 0, 0]`  
   - Increment `p3 = 1`.  
3. Calculate `next_ugly = min(2*2, 2*3, 1*5) = 4`. Update:  
   - `dp = [1, 2, 3, 4, 0, 0, 0, 0, 0, 0]`  
   - Increment `p2 = 2`.  

Repeat this process until `dp = [1, 2, 3, 4, 5, 6, 8, 9, 10, 12]`.  
Return `dp[-1] = 12`.  

#### Edge Cases
- n = 1: Return 1.  
- Large n: Ensure the algorithm runs efficiently within constraints.  

---

### 6. Evaluate

#### Time Complexity
- Generating n Ugly Numbers requires O(n) iterations.  
- Each iteration involves a constant-time operation to find the minimum and update pointers.  
- Total: O(n).  

#### Space Complexity
- Array `dp` of size n: O(n).  
- Additional variables (`p2`, `p3`, `p5`): O(1).  
- Total: O(n).  

---

### Additional Notes

### Min-Heap Solution

#### Plan:
1. Use a min-heap to maintain the smallest Ugly Numbers.  
2. Start with the first Ugly Number 1 in the heap.  
3. For n iterations, extract the smallest number from the heap and add its multiples (2, 3, 5) back into the heap if not already present.  
4. Use a set to avoid duplicate entries in the heap.  
5. After n iterations, the last extracted number is the nth Ugly Number.  

#### Code:
```python
import heapq

def nthUglyNumber(n):
    heap = [1]  # Initialize min-heap with the first Ugly Number
    seen = {1}  # Use a set to avoid duplicates
    primes = [2, 3, 5]  # Factors to multiply with

    for _ in range(n - 1):
        ugly = heapq.heappop(heap)
        for prime in primes:
            new_ugly = ugly * prime
            if new_ugly not in seen:
                seen.add(new_ugly)
                heapq.heappush(heap, new_ugly)

    return heapq.heappop(heap)
```

#### Time Complexity:
- Inserting into and extracting from the heap takes O(log k), where k is the size of the heap.  
- Each number generates three new candidates, and the heap grows approximately proportional to n.  
- Total time complexity: O(n log n).  

#### Space Complexity:
- The heap and the set can grow up to O(3n) in size.  
- Total space complexity: O(n).  

#### Comparison:
- The dynamic programming solution is faster with O(n) time complexity, but the min-heap solution is easier to generalize to similar problems like Super Ugly Number.

---

#### Why This Problem is Important
- Teaches efficient sequence generation with minimal computation.  
- Reinforces concepts of dynamic programming, heap usage, and pointer management.  

#### Prerequisites
- Understanding of arrays, heaps, and dynamic programming basics.  

#### Industry Relevance
- Used in scheduling, resource allocation, and sequence management.  

#### Follow-up Practice Problems
- LeetCode 1201: Ugly Number III  
- LeetCode 313: Super Ugly Number  
- LeetCode 42: Trapping Rain Water  
