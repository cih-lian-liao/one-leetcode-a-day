
# LeetCode 169 — Majority Element

#### Description:

Given an array `nums` of size `n`, return the majority element.
The majority element is the element that appears more than `⌊n / 2⌋` times.
You may assume that the majority element **always exists** in the array.

---

(UMPIRE Method)

### U — Understand

#### Input:

* A non-empty list of integers, `nums`.

#### Output:

* An integer representing the majority element.

#### Constraints:

* The input array is guaranteed to have a majority element.
* The majority element appears more than half of the size of the array.

---

### M — Match

#### Pattern:

* Frequency counting / Voting

#### Related Techniques:

* HashMap (naive)
* Sorting (middle element)
* **Boyer-Moore Voting Algorithm** (optimal)

---

### P — Plan

#### Boyer-Moore Voting Algorithm:

1. Initialize a `count = 0` and a `candidate = None`.
2. Traverse the array:

   * If `count == 0`, set `candidate = current number`.
   * If `current number == candidate`, increment count.
   * Else, decrement count.
3. Return `candidate`.

This works because the majority element is guaranteed to be more than half of the array length, so it always survives the "voting" process.

---

### I — Implement

#### ✅ Python Version:

```python
from typing import List

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num  # Update candidate
            count += (1 if num == candidate else -1)

        return candidate
```

#### ✅ JavaScript Version:

```javascript
var majorityElement = function(nums) {
    let count = 0;
    let candidate = null;

    for (let num of nums) {
        if (count === 0) {
            candidate = num; // Update candidate
        }
        count += (num === candidate) ? 1 : -1;
    }

    return candidate;
};
```

---

### R — Review (Test Cases)

#### Example 1:

```text
Input: [3,2,3]
Output: 3
```

#### Example 2:

```text
Input: [2,2,1,1,1,2,2]
Output: 2
```

---

### E — Evaluate

#### Time Complexity:

* O(n) — single pass through the array.

#### Space Complexity:

* O(1) — constant space, no extra data structure used.

---

# LeetCode 169 — 多數元素

#### 題目描述：

給定一個大小為 `n` 的整數陣列 `nums`，返回其中的多數元素。
多數元素是指在陣列中出現**次數超過 `⌊n / 2⌋` 次**的元素。
你可以假設輸入中一定存在多數元素。

---

### U — 理解（Understand）

#### 輸入：

* 一個非空整數陣列 `nums`

#### 輸出：

* 陣列中出現超過一半次數的元素

#### 限制條件：

* 題目保證一定存在多數元素

---

### M — 類型匹配（Match）

#### 類型：

* 頻率統計 / 投票機制

#### 解法選擇：

* HashMap 統計法（簡單但需額外空間）
* 排序取中法（中位數即為答案）
* ✅ Boyer-Moore 投票法（最優）

---

### P — 解題計畫（Plan）

#### 使用 Boyer-Moore 投票法：

1. 設定初始變數：`count = 0`，`candidate = None`
2. 遍歷陣列：

   * 如果 `count == 0`，就更新候選人為當前數字
   * 若當前數字與候選人相同，`count += 1`
   * 否則，`count -= 1`
3. 遍歷結束後，`candidate` 即為多數元素

由於多數元素出現超過一半，最終一定會留下來當作候選人。

---

### I — 程式碼實作（Implement）

#### ✅ Python 實作：

```python
from typing import List

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num  # 更新候選人
            count += (1 if num == candidate else -1)

        return candidate
```

#### ✅ JavaScript 實作：

```javascript
var majorityElement = function(nums) {
    let count = 0;
    let candidate = null;

    for (let num of nums) {
        if (count === 0) {
            candidate = num; // 更新候選人
        }
        count += (num === candidate) ? 1 : -1;
    }

    return candidate;
};
```

---

### R — 測試（Review）

#### 範例 1：

```text
輸入: [3,2,3]
輸出: 3
```

#### 範例 2：

```text
輸入: [2,2,1,1,1,2,2]
輸出: 2
```

---

### E — 評估（Evaluate）

#### 時間複雜度：

* O(n)，遍歷一次陣列

#### 空間複雜度：

* O(1)，使用常數空間，無需額外記憶體
