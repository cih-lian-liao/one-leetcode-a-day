# LeetCode 80 — Remove Duplicates from Sorted Array II (UMPIRE Method)

### 🧠 Problem Statement

#### Description:
Given a sorted integer array `nums`, remove duplicates in-place such that **each unique element appears at most twice**, and return the new length.  
Do not allocate extra space for another array.

#### Input:
- `nums`: List[int], sorted in non-decreasing order

#### Output:
- An integer representing the new length of the array after removing excess duplicates

---

### 🧠 U — Understand

#### Inputs:
- A sorted array of integers `nums`

#### Outputs:
- The new length of the modified array with each number appearing at most twice

#### Constraints:
- Elements must appear at most twice
- In-place operation (O(1) extra space)
- Relative order must be preserved

---

### 🔍 M — Match

#### Pattern:
- Two-pointer pattern

#### Strategy:
- One pointer (`j`) scans the array
- One pointer (`i`) writes valid entries in-place
- We track whether a value is appearing more than twice by checking `nums[j] != nums[i - 2]`

---

### 📝 P — Plan

#### Step-by-step Plan:
1. If `len(nums) <= 2`, return the length directly
2. Initialize `i = 2` (first two values are always kept)
3. Iterate `j` from 2 to end of array:
   - If `nums[j] != nums[i - 2]`, copy `nums[j]` to `nums[i]` and increment `i`
4. Return `i` as the final length

---

### 💻 I — Implementation

```python
from typing import List

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 2:
            return len(nums)

        i = 2  # first two values are always kept
        for j in range(2, len(nums)):
            if nums[j] != nums[i - 2]:
                nums[i] = nums[j]
                i += 1

        return i
````

---

### ✅ R — Review

#### Example 1:

Input: `[1,1,1,2,2,3]`
Output: `5`, `nums = [1,1,2,2,3,_]`

#### Example 2:

Input: `[0,0,1,1,1,1,2,3,3]`
Output: `7`, `nums = [0,0,1,1,2,3,3,_,_]`

---

### 🧮 E — Evaluate

#### Time Complexity:

* O(n), single pass

#### Space Complexity:

* O(1), in-place modification



---
---
---

# LeetCode 80 — 刪除排序陣列中的重複項 II（UMPIRE 方法）

### 🧠 題目說明

#### 題目描述：
給定一個已排序的整數陣列 `nums`，請你**就地刪除多餘的重複元素**，使每個元素**最多出現兩次**，並返回移除後的新長度。  
請勿使用額外陣列空間（必須為 O(1) 空間）。

#### 輸入：
- `nums`: 非遞減排序的整數陣列

#### 輸出：
- 整數，表示移除後的陣列長度

---

### 🧠 U — 理解（Understand）

#### 輸入：
- 一個排序後的整數陣列 `nums`

#### 輸出：
- 移除多餘重複項後的陣列長度，且每個元素最多出現兩次

#### 限制：
- 原地操作，空間複雜度為 O(1)
- 要保留元素的相對順序

---

### 🔍 M — 類型匹配（Match）

#### 類型：
- 雙指針（Two Pointers）

#### 解法邏輯：
- 一個指針 `j` 負責遍歷整個陣列
- 一個指針 `i` 表示下次寫入的位置
- 若 `nums[j] != nums[i - 2]`，表示可以保留，避免第三次重複

---

### 📝 P — 解題計畫（Plan）

#### 解法步驟：
1. 若 `nums` 長度小於等於 2，直接回傳長度
2. 設定指針 `i = 2`，前兩個數字一定保留
3. 用 `j` 從第 2 個 index 開始遍歷：
   - 如果 `nums[j] != nums[i - 2]`，就把 `nums[j]` 寫入 `nums[i]`，並 `i += 1`
4. 遍歷結束後，回傳 `i` 作為新陣列長度

---

### 💻 I — 程式碼實作

```python
from typing import List

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 2:
            return len(nums)

        i = 2  # 前兩個數字一定保留
        for j in range(2, len(nums)):
            if nums[j] != nums[i - 2]:
                nums[i] = nums[j]
                i += 1

        return i
````

---

### ✅ R — 測試案例（Review）

#### 範例 1：

輸入：`[1,1,1,2,2,3]`
輸出：`5`，`nums = [1,1,2,2,3,_]`

#### 範例 2：

輸入：`[0,0,1,1,1,1,2,3,3]`
輸出：`7`，`nums = [0,0,1,1,2,3,3,_,_]`

---

### 🧮 E — 複雜度分析（Evaluate）

#### 時間複雜度：

* O(n)，遍歷一次陣列

#### 空間複雜度：

* O(1)，就地修改，不用額外空間
