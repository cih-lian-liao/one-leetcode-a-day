

# 📝 LeetCode 57 — Insert Interval



### 🌟 UMPIRE (English Version)

### 🔍 U — Understand

* You are given a list of sorted, non-overlapping intervals.
* You need to insert a new interval into it and merge overlaps if necessary.
* Return the final list sorted and non-overlapping.
* **Example:**
  Input: `[[1,3],[6,9]], new=[2,5]` → Output: `[[1,5],[6,9]]`.

---

### 🧩 M — Match

* This is a **classic interval problem**.
* Because the input intervals are sorted and non-overlapping, we can solve it in **O(n)** with a **three-phase scan**:

  1. Add intervals that end before `new.start`.
  2. Merge intervals that overlap with `new`.
  3. Add intervals that start after `new.end`.

---

### 🗺️ P — Plan

#### Pseudocode

```
res = []
i = 0
[start, end] = newInterval

# Phase 1: add all intervals ending before new starts
while intervals[i].end < start:
    res.append(intervals[i])

# Phase 2: merge overlaps
while intervals[i].start <= end:
    start = min(start, intervals[i].start)
    end   = max(end, intervals[i].end)

# Phase 3: append merged new, then the rest
res.append([start, end])
res += remaining intervals
```

---

### 💻 I — Implement (Python with detailed comments)

```python
from typing import List

class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        """
        Insert a new interval into sorted, non-overlapping intervals.
        Merge overlaps if necessary.
        Time: O(n), Space: O(n)
        """
        res = []   # final result list
        i = 0      # pointer index
        n = len(intervals)

        # Unpack newInterval into start and end
        start, end = newInterval

        # 1) Add all intervals ending before newInterval starts
        #    Condition: current interval's end < new.start
        while i < n and intervals[i][1] < start:
            res.append(intervals[i])
            i += 1

        # 2) Merge all intervals overlapping with newInterval
        #    Overlap condition (closed intervals): intervals[i].start <= new.end
        while i < n and intervals[i][0] <= end:
            start = min(start, intervals[i][0])  # expand left bound
            end = max(end, intervals[i][1])      # expand right bound
            i += 1

        # Add the merged newInterval
        res.append([start, end])

        # 3) Add the rest (all intervals strictly to the right)
        while i < n:
            res.append(intervals[i])
            i += 1

        return res
```

---

### 🧐 R — Review

* Dry run with examples confirms correctness:

  * `[[1,3],[6,9]], new=[2,5]` → `[[1,5],[6,9]]`
  * `[[1,2],[3,5],[6,7],[8,10],[12,16]], new=[4,8]` → `[[1,2],[3,10],[12,16]]`

---

### 📊 E — Evaluate

* **Time Complexity:** O(n), because we scan each interval once.
* **Space Complexity:** O(n), for storing result.

<br>

# 🐼 UMPIRE (中文版本)

### 🔍 U — 理解題意

* 已給定一組**排序且不重疊**的區間。
* 將一個新的區間插入其中，若有重疊則合併。
* 回傳最後仍然**有序且不重疊**的結果。
* **範例:**
  輸入: `[[1,3],[6,9]], new=[2,5]` → 輸出: `[[1,5],[6,9]]`.

---

### 🧩 M — 匹配典型模式

* 這是**經典區間合併**問題。
* 由於輸入已排序且不重疊，可以用**三段式掃描**在 **O(n)** 內完成：

  1. 加入所有在新區間左側的。
  2. 將與新區間重疊的合併。
  3. 加入新區間右側的。

---

### 🗺️ P — 計畫

#### 偽代碼

```
res = []
i = 0
[start, end] = newInterval

# 第一段：加入所有結尾 < new.start 的
while intervals[i].end < start:
    res.append(intervals[i])

# 第二段：合併所有與 new 有重疊的
while intervals[i].start <= end:
    start = min(start, intervals[i].start)
    end   = max(end, intervals[i].end)

# 加入合併後的新區間
res.append([start, end])

# 第三段：補上右側剩下的
res += 剩餘的區間
```

---

### 💻 I — 實作 (Python 詳細註解)

```python
from typing import List

class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        """
        將 newInterval 插入已排序且不重疊的 intervals，並合併所有重疊的。
        時間: O(n)，空間: O(n)
        """
        res = []  # 最終結果
        i = 0
        n = len(intervals)

        start, end = newInterval  # 拆解 newInterval

        # 1) 加入所有在 new 左側的
        # 條件：interval.end < new.start
        while i < n and intervals[i][1] < start:
            res.append(intervals[i])
            i += 1

        # 2) 合併與 new 有重疊的
        # 條件：interval.start <= new.end
        while i < n and intervals[i][0] <= end:
            start = min(start, intervals[i][0])  # 擴張左邊界
            end = max(end, intervals[i][1])      # 擴張右邊界
            i += 1

        # 加入合併後的 newInterval
        res.append([start, end])

        # 3) 加入所有在 new 右側的
        while i < n:
            res.append(intervals[i])
            i += 1

        return res
```

---

### 🧐 R — 檢查

* 手算範例：

  * `[[1,3],[6,9]], new=[2,5]` → `[[1,5],[6,9]]`
  * `[[1,2],[3,5],[6,7],[8,10],[12,16]], new=[4,8]` → `[[1,2],[3,10],[12,16]]`

---

### 📊 E — 評估

* **時間複雜度:** O(n)，單趟掃描。
* **空間複雜度:** O(n)，用來儲存結果。

<br>


# 📌 附加筆記 (Notes & Pitfalls)

* **Overlap condition**：要用 `intervals[i][0] <= end`，因為端點相接（例如 `[3,5]` 和 `[5,7]`）也算重疊。
* **Common mistakes**：
  * 忘記把合併後的 newInterval 加入結果。
  * 忘記補上右側的 intervals。
  * 把 touching endpoints 當作不重疊。
* **Best learning**：
  * 先在紙上畫數軸測試五種案例（完全左邊、完全右邊、跨多段、相接、空陣列）。
  * 熟悉「三段式掃描」模式，這會在許多 interval 題目出現。

<br>

