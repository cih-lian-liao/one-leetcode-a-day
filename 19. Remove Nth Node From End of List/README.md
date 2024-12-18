# LeetCode 19: Remove Nth Node From End of List

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (E)valuate

### **1. Understand**

#### Problem Statement
Given the `head` of a singly linked list and an integer `n`, remove the nth node from the end of the list and return its head.

#### Inputs
- A singly linked list with nodes containing integer values.
- An integer `n` representing the position from the end of the list.

#### Outputs
- The head of the modified linked list after removing the nth node from the end.

#### Constraints
- The number of nodes in the list is at least 1.
- `1 <= n <= size of the linked list`.

#### Assumptions
- The input list is valid, and `n` will always be a valid position.
- Modifications to the input list are allowed.
---
### **2. Match**

#### Problem Type
This is a classic **Linked List** problem that can be solved efficiently using the **Two-Pointer (fast and slow pointers)** technique.

#### Data Structures
- Use a **dummy node** to simplify edge case handling (e.g., removing the head).
- Utilize two pointers, `fast` and `slow`, to identify the node to remove in one traversal.
---
### **3. Plan**

#### Step-by-Step Plan
1. **Add a dummy node** before the head of the linked list.
2. **Initialize two pointers** (`fast` and `slow`) at the dummy node.
3. Move the `fast` pointer `n+1` steps ahead.
4. Move both `fast` and `slow` pointers one step at a time until `fast` reaches the end.
5. Adjust the `next` pointer of `slow` to skip the nth node.
6. Return `dummy.next` as the new head of the modified list.

#### Edge Cases
- List with only one node and `n = 1`.
- Removing the head of the list (e.g., `n = size of the list`).

#### Time Complexity
O(n), where `n` is the number of nodes in the list.

#### Space Complexity
O(1), as no additional data structures are used.
---
### **4. Implement**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head, n):
    """
    Remove the nth node from the end of the list.
    :param head: ListNode, the head of the linked list
    :param n: int, the position from the end
    :return: ListNode, the head of the modified list
    """
    # Step 1: Create a dummy node
    dummy = ListNode(0, head)
    fast = dummy
    slow = dummy

    # Step 2: Move fast pointer n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next

    # Step 3: Move both fast and slow pointers until fast reaches the end
    while fast:
        fast = fast.next
        slow = slow.next

    # Step 4: Skip the nth node
    slow.next = slow.next.next

    # Step 5: Return the new head
    return dummy.next
```

#### Notes for Implementation
- **Dummy Node**: The dummy node simplifies edge cases like removing the head.
- **Two-Pointer Technique**: Maintaining a gap of `n` steps ensures that `slow` points to the node before the one to be removed.
- **Edge Cases in Code**: The `for` loop ensures the fast pointer is properly positioned even if the list has only one node.
---
### **5. Review**

#### Debugging and Dry Run
Test the code with the following scenarios:

1. **Case 1: Removing a middle node**
   ```python
   head = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
   n = 2
   result = removeNthFromEnd(head, n)
   # Expected Output: [1, 2, 3, 5]
   ```

2. **Case 2: Removing the head**
   ```python
   head = ListNode(1, ListNode(2))
   n = 2
   result = removeNthFromEnd(head, n)
   # Expected Output: [2]
   ```

3. **Case 3: Single node list**
   ```python
   head = ListNode(1)
   n = 1
   result = removeNthFromEnd(head, n)
   # Expected Output: []
   ```

#### Explanation for Dry Run (Case 1)
- Input: `1 -> 2 -> 3 -> 4 -> 5`, `n = 2`.
- Initialize: `dummy -> 1 -> 2 -> 3 -> 4 -> 5`, `fast` and `slow` point to `dummy`.
- Move `fast` 3 steps ahead (`n+1`): `fast` points to `3`.
- Move both pointers:
  - `fast` to `4`, `slow` to `1`.
  - `fast` to `5`, `slow` to `2`.
  - `fast` becomes `null`, `slow` points to `3`.
- Skip node: Adjust `slow.next = slow.next.next`, resulting in `1 -> 2 -> 3 -> 5`.
---
### **6. Evaluate**

#### Time Complexity
O(n), as the list is traversed once.

#### Space Complexity
O(1), as only pointers are used.

### **Additional Notes**

#### Why This Problem is Important
- Teaches the two-pointer technique, which is highly efficient for linked list problems.
- Frequently appears in technical interviews to test understanding of pointers and linked list manipulation.

#### Prerequisites for Practicing This Problem
- Understanding of linked list basics (e.g., traversal, node manipulation).
- Familiarity with the two-pointer technique.

#### Industry Relevance
- Commonly asked in FAANG interviews as it evaluates problem-solving skills with pointers.
- Variants of this problem appear in real-world applications, such as list processing and memory management.

#### Follow-up Practice Problems
- [LeetCode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
- [LeetCode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
- [LeetCode 142: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
