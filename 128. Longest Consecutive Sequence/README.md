
# 🧩 LeetCode 128 - Longest Consecutive Sequence


### 🌎 UMPIRE (English Version)

### 🔍 Understand
- **Goal**: Find the length of the longest sequence of consecutive integer values in `nums`.  
- **Examples**:  
  - `[100,4,200,1,3,2] → 4` (sequence: `1,2,3,4`)  
  - `[0,3,7,2,5,8,4,6,0,1] → 9` (sequence: `0..8`)  
- **Edge cases**: Empty array, single element, duplicates, negative values.

---

### 🧠 Match
- Matches problems solved by **Set-based scanning** (O(n)).  
- Another option: **Sorting** (O(n log n)), simpler but slower.  
- Union-Find possible but overkill.

---

### 📝 Plan
**Approach A: Set + start-of-streak (O(n))**  
1. Insert all numbers into a set.  
2. For each number, if `num-1` not in set → treat as start.  
3. Expand forward (`num+1, num+2...`) and count.  
4. Track maximum streak length.  

**Approach B: Sorting (O(n log n))**  
1. Sort numbers.  
2. Iterate through sorted list:  
   - Skip duplicates.  
   - If current = prev+1 → increase streak.  
   - Else → reset streak.  
3. Track maximum streak length.  

---

### 💻 Implement (Python 3)

#### ✅ Approach A: Set + start-of-streak (O(n))
```python
from typing import List

class SolutionSet:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        O(n) average time using a set and "start-of-streak" detection.
        """
        if not nums:
            return 0

        num_set = set(nums)
        longest = 0

        for num in num_set:
            # only start if num-1 is not in the set
            if (num - 1) not in num_set:
                current = num
                streak = 1

                while (current + 1) in num_set:
                    current += 1
                    streak += 1

                longest = max(longest, streak)

        return longest
````

#### ✅ Approach B: Sorting (O(n log n))

```python
from typing import List

class SolutionSorting:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        O(n log n) solution using sorting.
        Simpler but slower than the set-based approach.
        """
        if not nums:
            return 0

        nums.sort()

        longest = 1
        streak = 1

        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                # skip duplicates
                continue
            elif nums[i] == nums[i - 1] + 1:
                # consecutive → extend streak
                streak += 1
            else:
                # reset streak
                longest = max(longest, streak)
                streak = 1

        return max(longest, streak)
```

---

### 🧐 Review

* `[100,4,200,1,3,2]`:

  * Set method → longest = 4.
  * Sorting → after sort `[1,2,3,4,100,200]` → streak=4.
* `[0,3,7,2,5,8,4,6,0,1]`:

  * Set method → longest = 9.
  * Sorting → `[0,0,1,2,3,4,5,6,7,8]` → skip dup, streak=9.

---

### ⚖️ Evaluate

* **Set method**: O(n) average, O(n) space.
* **Sorting method**: O(n log n), O(1) or O(n) space depending on sort.
* Both handle duplicates.
* The set-based method is optimal for large input; sorting is easier to code.



# 🍵 UMPIRE (中文版)

### 🔍 理解題意

* **目標**：找到最長「連續數值序列」的長度。
* **例子**：

  * `[100,4,200,1,3,2] → 4` (序列 `1,2,3,4`)
  * `[0,3,7,2,5,8,4,6,0,1] → 9` (序列 `0..8`)
* **邊界情況**：空陣列、單元素、重複值、負數。

---

### 🧠 比對已知問題類型

* **最佳方法**：哈希集合 (Set) + 起點偵測 → O(n)。
* **次佳方法**：排序法 → O(n log n)，較直觀但效能差一點。
* 並查集也能做，但不划算。

---

### 📝 規劃解法

**方法 A：Set + 起點偵測 (O(n))**

1. 將所有數字放進 set。
2. 若 `num-1` 不在 set，num 是序列起點。
3. 往右延伸直到斷掉，計算長度。
4. 更新最長長度。

**方法 B：排序法 (O(n log n))**

1. 先排序陣列。
2. 遍歷排序後的數列：

   * 遇到重複值跳過。
   * 若 `nums[i] == nums[i-1]+1` → streak++。
   * 否則重置 streak=1。
3. 回傳最長 streak。

---

### 💻 程式實作（Python 3）

#### ✅ 方法 A：Set + 起點偵測 (O(n))

```python
from typing import List

class SolutionSet:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        使用 set 的 O(n) 平均解法
        """
        if not nums:
            return 0

        num_set = set(nums)
        longest = 0

        for num in num_set:
            if (num - 1) not in num_set:
                current = num
                streak = 1

                while (current + 1) in num_set:
                    current += 1
                    streak += 1

                if streak > longest:
                    longest = streak

        return longest
```

#### ✅ 方法 B：排序法 (O(n log n))

```python
from typing import List

class SolutionSorting:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        使用排序法 O(n log n)
        直觀但效能略差
        """
        if not nums:
            return 0

        nums.sort()
        longest = 1
        streak = 1

        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                continue
            elif nums[i] == nums[i - 1] + 1:
                streak += 1
            else:
                longest = max(longest, streak)
                streak = 1

        return max(longest, streak)
```

---

### 🧐 驗證

* 測資 `[100,4,200,1,3,2]`：

  * Set → 4
  * 排序後 `[1,2,3,4,100,200]` → streak=4。
* 測資 `[0,3,7,2,5,8,4,6,0,1]`：

  * Set → 9
  * 排序後 `[0,0,1,2,3,4,5,6,7,8]` → streak=9。

---

### ⚖️ 複雜度分析

* **Set 法**：O(n) 平均，空間 O(n)。
* **排序法**：O(n log n)，空間 O(1) 或 O(n)。
* 兩者皆能處理重複值。
* **選擇建議**：大數據用 Set；小數據或考試心急可用排序法。


# ✨ Additional Notes｜附加筆記

#### 🚩 注意事項

1. **不要從每個數字都往右數** → 會退化成 O(n²)。
2. **確保只從起點開始** → 這是 O(n) 的關鍵。
3. **處理重複值** → Set 自然去重；排序法需要 `continue` 跳過。
4. **排序法適合快速寫**，Set 法適合面試最佳解。
5. **題目核心是「連續數值」不是「相鄰下標」**。

