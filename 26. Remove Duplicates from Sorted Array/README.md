
# LeetCode 26: Remove Duplicates from Sorted Array

### Problem Statement

Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Return the new length of the array.

Do not allocate extra space for another array. You must modify the input array in place with O(1) extra memory.

#### Input
- `nums`: An array of integers sorted in non-decreasing order.

#### Output
- An integer representing the new length of the array with no duplicates.

#### Constraints
- `0 <= nums.length <= 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in non-decreasing order.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

#### Inputs:
- A sorted integer array `nums`.

#### Outputs:
- The new length of the array after removing duplicates.

#### Constraints:
- No extra space allocation; in-place modification only.
- The order of elements must remain the same.

#### Assumptions:
- If `nums` has a length of 0 or 1, it already satisfies the conditions.

#### Clarifications:
- Any part of `nums` beyond the new length is irrelevant.

---

### 2. Match

This is a **two-pointer problem**, where:
- One pointer (`slow`) keeps track of the position for the next unique element.
- The other pointer (`fast`) traverses the array to find unique elements.

**Pattern**: Array manipulation with two pointers.  
**Algorithm**: Iterate with one pointer, copy values to the position indicated by the other pointer.

---

### 3. Plan

#### Step-by-step Plan:
1. If the array length is 0, return 0 immediately.
2. Initialize two pointers:
   - `slow = 0`: Keeps track of the position of the last unique element.
   - `fast = 1`: Traverses the array to find unique elements.
3. Iterate through the array using `fast`:
   - If `nums[fast] != nums[slow]`, update `nums[slow + 1]` with `nums[fast]` and move `slow` one step forward.
4. After the iteration, the new length is `slow + 1`.
5. Return `slow + 1` as the result.

#### Edge Cases:
- Empty array (`nums = []`).
- Single element array (`nums = [1]`).
- All elements are duplicates (`nums = [1, 1, 1]`).
- No duplicates (`nums = [1, 2, 3]`).

#### Time Complexity:
O(n) - We traverse the array once.

#### Space Complexity:
O(1) - We modify the array in place without additional storage.

---

### 4. Implement

#### Python Code:

```python
def remove_duplicates(nums):
    """
    Removes duplicates from a sorted array in-place.

    Args:
    nums (List[int]): A sorted array of integers.

    Returns:
    int: The new length of the array with unique elements.
    """
    # Edge case: empty array
    if not nums:
        return 0

    # Initialize slow pointer
    slow = 0

    # Iterate with fast pointer
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]  # Update the next unique position

    # The length of the array with unique elements
    return slow + 1
```

#### Notes for Code Implementation:
1. Start with edge cases (`len(nums) == 0`).
2. Use the `slow` pointer to track the last unique position.
3. Compare elements using the `fast` pointer.
4. Update the array in place when a new unique element is found.

---

### 5. Review

#### Debugging:
- Test basic cases such as empty arrays and arrays with only one element.
- Run through complex cases where the array contains alternating duplicates.

#### Example Walkthrough:

##### Example 1:
Input: `nums = [1, 1, 2]`

- Start: `slow = 0`, `fast = 1`
- Iteration:
  - `fast = 1`: `nums[fast] == nums[slow]` (skip).
  - `fast = 2`: `nums[fast] != nums[slow]` → Update `nums[slow + 1] = nums[fast]` and move `slow`.
- End: `nums = [1, 2, 2]`
- Result: Return `2`.

##### Example 2:
Input: `nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]`

- Start: `slow = 0`, `fast = 1`
- Iteration:
  - Skip duplicates until `fast = 2`.
  - `fast = 2`: `nums[fast] != nums[slow]` → Update and move `slow`.
  - Repeat for the entire array.
- End: `nums = [0, 1, 2, 3, 4, _, _, _, _, _]`
- Result: Return `5`.

---

### 6. Evaluate

#### Time Complexity:
- O(n): Single traversal of the array.

#### Space Complexity:
- O(1): In-place modification of the array.

#### Optimization:
This is already optimal for the given constraints.

---

### Additional Notes

#### The Reason Why This Problem is Important
- Demonstrates array manipulation techniques.
- Essential for learning in-place algorithms and space optimization.
- Common in real-world scenarios where memory usage is critical.

#### Prerequisites for Practicing This Problem
- Understanding of two-pointer technique.
- Basic array traversal and conditionals.

#### Industry Relevance
- Used in scenarios requiring data deduplication, such as database queries or sorting results.

#### Follow-up Practice Problems
1. **LeetCode 80**: Remove Duplicates from Sorted Array II (allow at most two duplicates).
2. **LeetCode 283**: Move Zeroes.
3. **LeetCode 27**: Remove Element.
4. **LeetCode 977**: Squares of a Sorted Array.

---
---
# LeetCode 26 — 移除排序陣列中的重複項（UMPIRE 方法）

### 🧠 題目說明（Problem Statement）

給定一個**升序排列**的整數陣列 `nums`，請**就地（in-place）移除重複的元素**，使每個唯一元素僅出現一次，並保留原本的相對順序。  
請回傳移除後陣列中**不重複元素的個數**。

#### 輸入：
- `nums`: 一個按非遞減順序排序的整數陣列

#### 輸出：
- 一個整數，代表移除重複項後的新陣列長度

#### 條件限制：
- `0 <= nums.length <= 10^4`
- `-100 <= nums[i] <= 100`
- `nums` 是升序排列的

---

##### 🧩 UMPIRE 方法（U / M / P / I / R / E）


### 1. U — 理解（Understand）

#### 輸入：
- 一個排序後的整數陣列 `nums`

#### 輸出：
- 移除重複元素後的陣列長度

#### 條件：
- 不可使用額外的陣列空間（in-place 原地修改）
- 要保留元素的相對順序

#### 假設：
- 如果陣列長度為 0 或 1，則視為已符合條件

#### 釐清：
- 移除後 `nums` 陣列中「新長度之後」的元素值不重要

---

### 2. M — 類型匹配（Match）

這是一個典型的 **雙指針問題（two-pointer problem）**，常見於需要在原地修改陣列的場景。

- 一個指針 `slow` 負責追蹤下個不重複元素應該放的位置
- 另一個指針 `fast` 負責遍歷整個陣列

#### 類型：陣列就地處理  
#### 技巧：一個指針遍歷，一個指針記錄結果位置

---

### 3. P — 解題計畫（Plan）

#### 步驟：
1. 若陣列為空，直接回傳 0。
2. 初始化兩個指針：
   - `slow = 0`：記錄最後一個唯一值的位置
   - `fast = 1`：負責遍歷陣列
3. 使用 `fast` 遍歷整個陣列：
   - 若 `nums[fast] != nums[slow]`：
     - 將 `nums[fast]` 放到 `nums[slow + 1]`
     - 將 `slow` 向前移動一格
4. 最終返回 `slow + 1`，即為不重複元素的數量

#### 邊界情況：
- 空陣列（`nums = []`）
- 單一元素（`nums = [1]`）
- 所有元素相同（`nums = [1,1,1]`）
- 無重複（`nums = [1,2,3]`）

#### 時間複雜度：
- O(n)：只需遍歷陣列一次

#### 空間複雜度：
- O(1)：不使用額外空間

---

### 4. I — 實作（Implement）

#### Python 程式碼：

```python
def remove_duplicates(nums):
    """
    就地移除排序陣列中的重複元素。

    參數:
    nums (List[int]): 排序後的整數陣列

    回傳:
    int: 移除重複項後的新陣列長度
    """
    if not nums:
        return 0  # 邊界條件：空陣列

    slow = 0  # 初始化 slow 指針

    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]  # 更新下個不重複的位置

    return slow + 1
````

#### 實作說明：

1. 檢查邊界情況（空陣列）
2. 使用 `slow` 記錄最後的不重複位置
3. 使用 `fast` 與前一個比較，若不同就複製
4. 最後回傳 `slow + 1` 為新長度

---

### 5. R — 測試與驗證（Review）

#### 除錯策略：

* 測試空陣列、單一元素、全部重複或完全不重複的情況
* 驗證是否正確保留不重複值

#### 範例走訪：

##### 範例 1：

輸入：`nums = [1, 1, 2]`

* 初始：`slow = 0`, `fast = 1`
* 步驟：

  * `fast = 1`：重複 → 跳過
  * `fast = 2`：不同 → `nums[1] = nums[2]`
* 結果：`nums = [1, 2, 2]`，回傳 `2`

##### 範例 2：

輸入：`nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]`

* 初始：`slow = 0`, `fast = 1`
* 過程：

  * 跳過所有重複，當遇到新值就更新 `nums[slow + 1]`
* 結果：`nums = [0, 1, 2, 3, 4, _, _, _, _, _]`，回傳 `5`

---

### 6. E — 複雜度分析（Evaluate）

#### 時間複雜度：

* O(n)：遍歷陣列一次

#### 空間複雜度：

* O(1)：就地修改，無需額外空間

#### 最佳化：

此解法已符合題目對空間與時間的最優要求

---

### 📌 補充說明（Additional Notes）

#### 為何這題重要？

* 練習陣列操作與雙指針技巧
* 熟悉原地處理與空間最佳化概念
* 在處理資料清洗或排序結果時很常見

#### 建議先備知識：

* 熟悉 for loop 與條件判斷
* 熟悉 two pointers 技巧

#### 延伸練習題：

1. **LeetCode 80**: 每個元素最多保留兩次（Remove Duplicates II）
2. **LeetCode 283**: 移動零（Move Zeroes）
3. **LeetCode 27**: 移除元素（Remove Element）
4. **LeetCode 977**: 平方排序（Squares of a Sorted Array）
