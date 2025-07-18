
# 📘 LeetCode 56: Merge Intervals (English Version – UMPIRE)

### 🧠 U — Understand the Problem

We are given an array of intervals, where each interval is a pair `[start, end]`.
Our goal is to merge all overlapping intervals and return an array of the merged intervals.

**Example:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

**Explanation:** Intervals `[1,3]` and `[2,6]` overlap, so they are merged into `[1,6]`.

---

### 🧩 M — Match to Known Problems

This is a classic **interval merging** problem.
Patterns involved:

* Sorting intervals by start time.
* Iterating and merging if current overlaps with the previous.

---

### 📝 P — Plan

1. Sort the intervals by the start value.
2. Initialize an empty result list called `merged`.
3. Iterate through the sorted intervals:

   * If `merged` is empty or the current interval does **not overlap** with the last one, just add it.
   * Otherwise, merge by updating the `end` of the last interval in `merged`.

---

### 💻 I — Implement

#### 🐍 Python Version

```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # Step 1: Sort intervals by the start value
        intervals.sort(key=lambda x: x[0])

        merged = []

        # Step 2: Traverse each interval
        for interval in intervals:
            # Case 1: merged is empty or current interval does not overlap
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                # Case 2: overlap exists, merge intervals by updating the end
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

#### 🌐 JavaScript Version

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    // Step 1: Sort intervals by the start value
    intervals.sort((a, b) => a[0] - b[0]);

    const merged = [];

    // Step 2: Traverse each interval
    for (let interval of intervals) {
        // Case 1: merged is empty or no overlap
        if (merged.length === 0 || merged[merged.length - 1][1] < interval[0]) {
            merged.push(interval);
        } else {
            // Case 2: overlap exists, merge intervals
            merged[merged.length - 1][1] = Math.max(
                merged[merged.length - 1][1],
                interval[1]
            );
        }
    }

    return merged;
};
```

---

### 📈 R — Review and Optimize

**Time Complexity:**

* Sorting takes O(n log n)
* Traversing once takes O(n)
  → Overall: **O(n log n)**

**Space Complexity:**

* Result list may store all intervals → **O(n)**

---

### 🔍 E — Evaluate Edge Cases

* Empty input → return `[]`
* One interval → return it directly
* No overlapping → return all intervals as-is

#
#
#

# 📙 中文版本 – UMPIRE 方法練習 LeetCode 56：合併區間

### 🧠 U — 理解問題

我們給定一個區間陣列，每個區間是 `[start, end]`。
目標是把有重疊的區間合併，回傳一個合併後的區間陣列。

**範例：**

```
輸入: [[1,3],[2,6],[8,10],[15,18]]
輸出: [[1,6],[8,10],[15,18]]
```

**說明：** `[1,3]` 和 `[2,6]` 有重疊，合併成 `[1,6]`。

---

### 🧩 M — 類比已知問題

這是一個經典的「合併區間」問題。
常見技巧包括：

* 先根據起始值排序。
* 逐個檢查區間是否重疊，有的話就合併。

---

### 📝 P — 擬定計畫

1. 先依照每個區間的 start 值做排序。
2. 建立一個空的 `merged` 陣列來放結果。
3. 遍歷排序後的每個區間：

   * 如果 `merged` 是空的或沒有重疊，就直接加入。
   * 否則就合併，把 `end` 更新為兩者的最大值。

---

### 💻 I — 實作程式碼

#### 🐍 Python 版本

```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # 步驟1：根據起始點排序
        intervals.sort(key=lambda x: x[0])

        merged = []

        # 步驟2：遍歷每個區間
        for interval in intervals:
            # 情況1：merged 是空的或沒有重疊
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                # 情況2：有重疊，更新 merged 最後一個區間的結束值
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

#### 🌐 JavaScript 版本

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    // 步驟1：根據起始點排序
    intervals.sort((a, b) => a[0] - b[0]);

    const merged = [];

    // 步驟2：遍歷每個區間
    for (let interval of intervals) {
        // 情況1：merged 是空的或沒有重疊
        if (merged.length === 0 || merged[merged.length - 1][1] < interval[0]) {
            merged.push(interval);
        } else {
            // 情況2：有重疊，合併並更新結束值
            merged[merged.length - 1][1] = Math.max(
                merged[merged.length - 1][1],
                interval[1]
            );
        }
    }

    return merged;
};
```

---

### 📈 R — 檢視與最佳化

**時間複雜度：**

* 排序是 O(n log n)
* 遍歷是 O(n)
  → 總計是 **O(n log n)**

**空間複雜度：**

* 儲存結果的 `merged` 最多 O(n)
  → 所以是 **O(n)**

---

### 🔍 E — 測試邊界條件

* 空陣列 → 回傳 `[]`
* 只有一個區間 → 直接回傳
* 沒有任何重疊 → 每個區間都照原樣回傳

