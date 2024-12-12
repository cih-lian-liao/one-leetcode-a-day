# LeetCode Problem 263: Ugly Number

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

**Problem Statement:**  

An **Ugly Number** is a positive integer whose prime factors are limited to 2, 3, and 5. Given an integer `n`, write a function to determine if it is an Ugly Number.

**Input:**  
- A single integer `n` (can be positive, negative, or zero).

**Output:**  
- A boolean value: `True` if `n` is an Ugly Number, `False` otherwise.

**Constraints:**  
- `n` can be any integer (positive, negative, or zero).
- `1` is considered an Ugly Number (edge case).

**Clarifications:**  
- If `n <= 0`, it is **not** an Ugly Number.
- The problem involves repeatedly dividing `n` by 2, 3, and 5 until it no longer can be divided by them.

---

### **2. Match**

This problem relates to **number factorization** and **modular arithmetic**:
- The task requires checking if a number can be reduced to `1` by dividing it repeatedly by specific factors (2, 3, 5).
- A straightforward approach involves loops for modular division.

**Patterns Recognized:**  
- This problem does not require advanced algorithms or data structures.
- A simple iterative loop suffices for dividing `n` by the allowed factors.

**Data Structures:**  
- No special data structures are needed.

---

### **3. Plan**

**Step-by-Step Plan:**
1. If `n <= 0`, immediately return `False` (negative numbers and zero are not Ugly Numbers).
2. Define the allowed factors: `[2, 3, 5]`.
3. Iterate through each factor:
   - While `n` is divisible by the current factor, divide `n` by the factor (using integer division).
4. After processing all factors, check if `n` equals `1`:
   - If `n == 1`, return `True`.
   - Otherwise, return `False`.

**Pseudocode:**
```text
if n <= 0:
    return False

for factor in [2, 3, 5]:
    while n is divisible by factor:
        divide n by factor

if n equals 1:
    return True
else:
    return False
```

**Time Complexity:**  
- O(log n) because each division reduces `n` significantly (by at least half for factor 2).  
- Iterating over 3 factors results in constant overhead, so the overall complexity is O(log n).

**Space Complexity:**  
- O(1) as no additional space is used beyond the input integer `n`.

---

### **4. Implement**

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        # Step 1: Handle invalid input
        if n <= 0:
            return False  # Negative numbers and zero are not Ugly Numbers
        
        # Step 2: Define allowed factors
        allowed_factors = [2, 3, 5]
        
        # Step 3: Reduce n by dividing with allowed factors
        for factor in allowed_factors:
            while n % factor == 0:  # Check divisibility
                n //= factor  # Reduce n by factor
        
        # Step 4: Check if n is reduced to 1
        return n == 1
```

**Implementation Notes:**  
- **Input Validation:** Start by checking if `n` is positive; otherwise, immediately return `False`.
- **Loop Explanation:** Each factor (2, 3, 5) is iterated, and `n` is reduced until it is no longer divisible by the factor.
- **Final Check:** After reducing all allowed factors, verify if `n` equals `1`. If yes, it is an Ugly Number; otherwise, it is not.

---

### **5. Review**

**Edge Cases:**  
- `n = 0`: Return `False` (zero is not an Ugly Number).  
- `n = -6`: Return `False` (negative numbers are not Ugly Numbers).  
- `n = 1`: Return `True` (1 is considered an Ugly Number).  
- `n = 14`: Return `False` (14 contains 7 as a prime factor, which is not allowed).  
- `n = 30`: Return `True` (30 = 2 × 3 × 5).

**Dry Run (Complex Case):**  
Input: `n = 1800`  
1. Initial check: `n > 0`, continue.  
2. Allowed factors: `[2, 3, 5]`.  
3. Start dividing:
   - Divide by 2: `1800 → 900 → 450 → 225`.
   - Divide by 3: `225 → 75 → 25`.
   - Divide by 5: `25 → 5 → 1`.  
4. Final check: `n == 1`, return `True`.

---

### **6. Evaluate**

**Time Complexity:**  
- O(log n): Each division reduces `n` significantly.

**Space Complexity:**  
- O(1): No additional memory is allocated.

**Optimizations:**  
- The solution is already optimal for the constraints.

**Alternative Approaches:**  
- Recursive implementation could achieve the same result but may have higher overhead due to function calls.

---

### **Additional Notes**

**Why This Problem is Important:**  
- Fundamental for understanding modular arithmetic and factorization.
- Helps in learning iterative loops and conditionals effectively.

**Prerequisites for Practicing This Problem:**  
- Basic knowledge of loops, conditionals, and modular arithmetic.

**Industry Relevance:**  
- Though not directly used in real-world applications, this problem helps sharpen algorithmic thinking and problem-solving skills.

**Follow-Up Practice Problems:**  
1. LeetCode 264: Ugly Number II (Find the nth Ugly Number).  
2. LeetCode 1201: Ugly Number III (Find the k-th smallest number divisible by a, b, or c).  
3. LeetCode 1710: Greatest Common Divisor of Strings (relates to divisibility).  
