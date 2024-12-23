# LeetCode Problem 853: Car Fleet

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

**Problem Statement:**
We are given a target destination `target` and two lists:
1. `position`: Starting positions of cars.
2. `speed`: Speed of cars.

Each car moves at its constant speed. A car fleet is formed when one car catches up with another car or cars and they travel together at the same speed. We need to calculate how many car fleets will arrive at the target.

**Inputs:**
- `target`: An integer representing the destination.
- `position`: A list of integers where each element represents the starting position of a car.
- `speed`: A list of integers where each element represents the speed of a car.

**Outputs:**
- An integer representing the number of car fleets.

**Constraints:**
- The positions and speeds of cars are non-negative.
- Each position is unique.
- All cars are initially behind the target.

**Assumptions:**
- Cars can only move forward.
- No car starts at or beyond the target.

---

### 2. Match

This problem is closely related to:
- **Sorting:** Sorting the cars based on their starting position helps us analyze their interaction as they approach the target.
- **Simulation:** We simulate the process of cars forming fleets by comparing their arrival times.

**Key Data Structures:**
- Sorting (`list.sort` or `sorted`) to arrange cars by position.
- Stack to track car fleets.

---

### 3. Plan

1. **Sort Cars:**
   - Pair each car's position with its speed.
   - Sort the cars in descending order of their positions.

2. **Calculate Arrival Times:**
   - For each car, calculate the time it takes to reach the target: \((\text{target} - \text{position}) / \text{speed}\).

3. **Simulate Fleets Using a Stack:**
   - Use a stack to store the arrival times of the fleets.
   - Traverse through the sorted cars and check:
     - If the current car's time is greater than the time at the top of the stack, it forms a new fleet.
     - Otherwise, it joins the fleet represented by the top of the stack.

4. **Return the Size of the Stack:**
   - The size of the stack represents the total number of fleets.

---

### 4. Implement

**Python Code:**

```python
def carFleet(target, position, speed):
    # Step 1: Pair position and speed, then sort by position in descending order
    cars = sorted(zip(position, speed), key=lambda x: -x[0])

    # Step 2: Calculate the arrival times
    times = [(target - pos) / spd for pos, spd in cars]

    # Step 3: Use a stack to track fleets
    stack = []
    for time in times:
        if not stack or time > stack[-1]:  # New fleet
            stack.append(time)
        # If time <= stack[-1], it joins the current fleet

    # Step 4: The number of fleets is the size of the stack
    return len(stack)
```

---

### 5. Review

**Edge Cases:**
1. No cars: `position = []`, `speed = []`.
2. Single car: `position = [5]`, `speed = [2]`.
3. All cars have the same speed.

**Dry Run Example:**

**Input:**
```python
target = 12
position = [10, 8, 0, 5, 3]
speed = [2, 4, 1, 1, 3]
```

**Execution:**
1. Sort cars: `[(10, 2), (8, 4), (5, 1), (3, 3), (0, 1)]`
2. Calculate times: `[1.0, 1.0, 7.0, 3.0, 12.0]`
3. Stack simulation:
   - Push `1.0`: Stack = `[1.0]`
   - Skip `1.0`: Stack = `[1.0]`
   - Push `7.0`: Stack = `[1.0, 7.0]`
   - Skip `3.0`: Stack = `[1.0, 7.0]`
   - Push `12.0`: Stack = `[1.0, 7.0, 12.0]`
4. Return result: `len(stack) = 3`

**Output:**
```python
3
```

---

### 6. Evaluate

**Time Complexity:**
- Sorting: O(n log n)
- Iterating through times: O(n)
- Total: O(n log n)

**Space Complexity:**
- Stack for arrival times: O(n)
- Total: O(n)

---

### Additional Notes

**Why This Problem is Important:**
- It tests your ability to combine sorting and simulation.
- Demonstrates the use of stack for managing states.

**Prerequisites:**
- Understanding of sorting and its time complexity.
- Familiarity with stack-based solutions.

**Industry Relevance:**
- Fleet simulation can be applied in logistics and traffic management.
- Demonstrates the ability to optimize group behaviors.

**Follow-up Practice Problems:**
1. **LeetCode 739:** Daily Temperatures
2. **LeetCode 496:** Next Greater Element I
3. **LeetCode 1019:** Next Greater Node In Linked List
4. **LeetCode 42:** Trapping Rain Water
