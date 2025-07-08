
# LeetCode 27 — Remove Element (UMPIRE Method)

### 🧠 Problem Statement

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` **in-place**.  
Return the new length of the array after removal.  
**Do not allocate extra space** — the operation must be performed in-place with O(1) extra memory.

---

### 🧠 U — Understand

#### Input:
- `nums`: List[int]
- `val`: int

#### Output:
- Return the new length of `nums` after removing all instances of `val`

#### Constraints:
- The relative order of the elements may be changed.
- Values beyond the returned length do not matter.

#### Example:
```python
Input: nums = [3, 2, 2, 3], val = 3
Output: 2  # nums becomes [2, 2, _, _]

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5  # nums becomes [0,1,3,0,4,_,_,_]
````

---

### 🔍 M — Match

This is a **two-pointer** problem, often seen in:

* In-place filtering
* Array rearrangement
* Space optimization problems

---

### 📝 P — Plan

1. Initialize a pointer `k = 0` to track where the next valid number (not equal to `val`) should be placed.
2. Loop through each index `i` in `nums`:

   * If `nums[i] != val`, assign `nums[k] = nums[i]` and increment `k`
   * Otherwise, skip
3. After the loop, return `k` as the new length.

---

### 💻 I — Implementation (with in-code comments)

```python
from typing import List

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        k = 0  # Pointer for the position to insert valid values

        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]  # Overwrite value at position k
                k += 1             # Move k forward to next insertion spot

        return k  # The length of the filtered array
```

---

### ✅ R — Review (Test Cases)

#### Test Case 1:

```python
nums = [3, 2, 2, 3]
val = 3
res = Solution().removeElement(nums, val)
print(res)         # Output: 2
print(nums[:res])  # Output: [2, 2]
```

#### Test Case 2:

```python
nums = [0,1,2,2,3,0,4,2]
val = 2
res = Solution().removeElement(nums, val)
print(res)         # Output: 5
print(nums[:res])  # Output: [0, 1, 3, 0, 4]
```

---

### 🧮 E — Evaluate

#### Time Complexity:

* O(n), where n is the length of the array

#### Space Complexity:

* O(1), since all modifications are done in-place

---

### 🗣️ Interview Explanation (What to Say)

> “I’ll use a two-pointer strategy. One pointer `i` iterates through the array, and another pointer `k` keeps track of where to place the next valid value.
> If the current element is not equal to `val`, I move it to index `k` and increment `k`.
> This way, I overwrite all the unwanted values in-place while preserving the rest.
> At the end, I return `k`, which represents the new length of the array.”

---
---

# LeetCode 27 — 移除元素（UMPIRE 方法）

### 🧠 題目說明（Problem Statement）

給定一個整數陣列 `nums` 和一個整數 `val`，請你就地（in-place）移除所有等於 `val` 的元素，並回傳移除後的陣列長度。  
**請使用 O(1) 額外空間進行原地操作，不可使用額外陣列。**

---

### 🧠 U — 理解（Understand）

#### 輸入：
- `nums`: List[int]
- `val`: int

#### 輸出：
- 回傳移除所有 `val` 後的陣列新長度

#### 條件限制：
- 可以改變剩餘元素的順序
- 超過新長度的元素內容不重要

#### 範例：
```python
輸入: nums = [3, 2, 2, 3], val = 3
輸出: 2  # nums 變為 [2, 2, _, _]

輸入: nums = [0,1,2,2,3,0,4,2], val = 2
輸出: 5  # nums 變為 [0,1,3,0,4,_,_,_]
````

---

### 🔍 M — 類型匹配（Match）

這是一題經典的 **雙指針（two-pointer）+ 原地操作** 題目。
常見於：

* 就地過濾
* 原陣列修改
* 空間優化

---

### 📝 P — 解題計畫（Plan）

1. 建立指標 `k = 0`，表示下次要放入有效元素的位置。
2. 用 `i` 遍歷整個陣列：

   * 如果 `nums[i] != val`，就將 `nums[i]` 放到 `nums[k]`，並將 `k` 加一
   * 如果是 `val` 就跳過
3. 迴圈結束後，`k` 就是移除後剩下的元素數量

---

### 💻 I — 程式碼實作（含中文註解）

```python
from typing import List

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        k = 0  # 指標 k：表示下次要放置有效元素的位置

        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]  # 將有效元素往前搬
                k += 1             # 下一個預計可能可以放的位置

        return k  # 回傳值即是移除後的有效元素的長度（因為最後k + 1）
```

---

### ✅ R — 測試驗證（Review）

#### 測試案例 1：

```python
nums = [3, 2, 2, 3]
val = 3
res = Solution().removeElement(nums, val)
print(res)         # 預期輸出: 2
print(nums[:res])  # 預期輸出: [2, 2]
```

#### 測試案例 2：

```python
nums = [0,1,2,2,3,0,4,2]
val = 2
res = Solution().removeElement(nums, val)
print(res)         # 預期輸出: 5
print(nums[:res])  # 預期輸出: [0, 1, 3, 0, 4]
```

---

### 🧮 E — 複雜度分析（Evaluate）

#### 時間複雜度：

* O(n)，n 為陣列長度，需要遍歷一次所有元素

#### 空間複雜度：

* O(1)，原地操作，不使用額外空間

---

### 🗣️ 面試說明建議（Interview 說法建議）

> 「我會使用雙指針策略，一個指針 `i` 負責遍歷陣列，另一個指針 `k` 用來追蹤下一個要寫入有效數字的位置。
> 每當我遇到不是 `val` 的數值，就把它放到 `nums[k]`，並將 `k` 往後移動。
> 這樣可以在不使用額外空間的情況下，就地完成過濾。
> 最後我回傳 `k`，代表陣列長度，也代表剩下的有效元素數量。

