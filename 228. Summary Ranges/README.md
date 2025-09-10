

# 📝 LeetCode 228: Summary Ranges (English UMPIRE Notes)

### 🔍 Understand

#### Problem

You are given a **sorted array of unique integers**. Summarize the consecutive numbers into compact ranges:

* Single number → `"x"`
* Range from `a` to `b` → `"a->b"`

#### Examples

* `[-1,0,1,2,4,5,7] → ["-1->2","4->5","7"]`
* `[0,2,3,4,6,8,9] → ["0","2->4","6","8->9"]`
* `[] → []`
* `[5] → ["5"]`

---

### 🧩 Match

* Array is sorted and strictly increasing.
* We can group consecutive runs (`nums[i+1] == nums[i] + 1`).
* Two typical approaches:

  1. **Single pointer (while loop cursor)**
  2. **Two pointers (`left`, `right`)**

---

### 🛠 Plan

#### Single Pointer (while loop)

* Use `i` to scan.
* Mark `start = nums[i]`.
* Extend while consecutive.
* Append `"start->end"` or single `"x"`.

#### Two Pointers

* `left` marks the start of a range.
* `right` expands until break.
* Append range and move `left = right + 1`.

---

### 💻 Implementations

#### 🌟 Version 1: Single Pointer (while loop)

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        ans = []   # Store the final result
        i = 0      # Cursor pointer

        while i < len(nums):
            start = nums[i]  # Mark the beginning of a range

            # Extend the range while consecutive
            while i < len(nums) - 1 and nums[i] + 1 == nums[i + 1]:
                i += 1

            # nums[i] is now the end of the range
            if start != nums[i]:
                ans.append(f"{start}->{nums[i]}")
            else:
                ans.append(str(nums[i]))

            i += 1  # Move cursor forward
        return ans
```

---

#### 🌟 Version 2: Two Pointers (left/right)

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        result = []
        n = len(nums)
        left = 0  # Left pointer marks start

        for right in range(n):
            # If last element OR break in consecutiveness
            if right == n - 1 or nums[right] + 1 != nums[right + 1]:
                if left == right:
                    result.append(str(nums[left]))
                else:
                    result.append(f"{nums[left]}->{nums[right]}")
                # Reset left for next range
                left = right + 1
        return result
```

---

### 🔎 Review

* Both methods handle:

  * Empty input
  * Single element
  * All consecutive
  * All isolated numbers

---

### ⚡ Evaluate

* **Time Complexity**: `O(n)` (each element visited at most twice).
* **Space Complexity**: `O(1)` extra (excluding output).

<br>

# 🌸 中文 UMPIRE 筆記 (LeetCode 228: 區間總結)

### 🔍 理解題意

給定一個 **已排序且不重複的整數陣列**，需要將連續的區間轉換成字串：

* 單一數字 → `"x"`
* 區間 `a` 到 `b` → `"a->b"`

#### 範例

* `[-1,0,1,2,4,5,7] → ["-1->2","4->5","7"]`
* `[0,2,3,4,6,8,9] → ["0","2->4","6","8->9"]`
* `[] → []`
* `[5] → ["5"]`

---

### 🧩 思路匹配

* 陣列已經排序、嚴格遞增。
* 可以分成兩種典型解法：

  1. **單指針 while 游標法**
  2. **雙指針 left/right 法**

---

### 🛠 解題計劃

#### 單指針

* 用 `i` 掃陣列。
* `start = nums[i]` 當區間起點。
* 內層 while 延伸區間直到不連續。
* 輸出 `"start->end"` 或單一數字。

#### 雙指針

* `left` 指向區間起點。
* `right` 移動直到不連續。
* 輸出 `left..right`，然後把 `left = right + 1`。

---

### 💻 程式碼實作

#### 🌟 版本一：單指針 while 游標法

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        ans = []   # 存放結果
        i = 0      # 游標指標

        while i < len(nums):
            start = nums[i]  # 起點

            # 延伸這段區間直到不連續
            while i < len(nums) - 1 and nums[i] + 1 == nums[i + 1]:
                i += 1

            # nums[i] 是區間的終點
            if start != nums[i]:
                ans.append(f"{start}->{nums[i]}")
            else:
                ans.append(str(nums[i]))

            i += 1  # 移到下一個數字
        return ans
```

---

#### 🌟 版本二：雙指針 left/right 法

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        result = []
        n = len(nums)
        left = 0   # 區間起點

        for right in range(n):
            # 如果是最後一個元素或下一個不連續
            if right == n - 1 or nums[right] + 1 != nums[right + 1]:
                if left == right:
                    result.append(str(nums[left]))
                else:
                    result.append(f"{nums[left]}->{nums[right]}")
                # 更新 left 指向下一段起點
                left = right + 1
        return result
```

---

### 🔎 檢查

* ✅ 空陣列 → `[]`
* ✅ 單一元素 → `["x"]`
* ✅ 全部連續 → `["a->b"]`
* ✅ 全部不連續 → 每個獨立輸出

---

### ⚡ 複雜度分析

* **時間**：`O(n)`
* **空間**：`O(1)` 額外空間

<br>

# 📌 附加筆記

* **單指針法**：程式碼更短，適合熟悉 while loop 的人。
* **雙指針法**：邏輯直觀，適合面試時清楚表達「起點 → 終點 → 收段 → 繼續下一段」。
* 都必須注意 **flush 最後一段**，否則最後區間會漏掉。
* 記得測試邊界案例：`[]`、`[5]`、`[1,2,3]`、`[1,3,5]`。

<br>
