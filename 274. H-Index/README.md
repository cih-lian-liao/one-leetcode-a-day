
# 🧠 LeetCode 274 - H-Index (Sort-Based Solution)

### 📘 UMPIRE Method (English Version)

### 🔍 U - Understand
We are given an array `citations`, where `citations[i]` represents the number of times the ith paper was cited.  
We are to compute the **H-Index**, which is defined as the **maximum h** such that there are at least **h papers with at least h citations**.

**Example:**
```

Input: [3, 0, 6, 1, 5]
Output: 3

````
There are 3 papers with at least 3 citations.

---

### 🧩 M - Match
This problem can be matched to a counting or threshold-finding problem.  
By **sorting the array in descending order**, we can evaluate how many top papers satisfy the H-index condition.

---

### 📝 P - Plan
1. Sort the `citations` array in descending order.
2. Initialize `h = 0`.
3. Loop through each paper:
   - If `citations[i] >= i + 1`, then we can update `h = i + 1`.
   - Else, break the loop.
4. Return `h`.

---

### 💻 I - Implement

#### 🐍 Python
```python
from typing import List

class Solution:
    def hIndex(self, citations: List[int]) -> int:
        citations.sort(reverse=True)  # Step 1: Sort citations descending
        h = 0
        for i, c in enumerate(citations):  # Step 2: Loop over sorted citations
            if c >= i + 1:  # Step 3: Check if current paper meets H-index condition
                h = i + 1  # Update h-index
            else:
                break  # Stop if condition breaks
        return h  # Step 4: Return the final h-index
````

#### 🌐 JavaScript

```javascript
var hIndex = function(citations) {
    citations.sort((a, b) => b - a); // Step 1: Sort citations descending
    let h = 0;
    for (let i = 0; i < citations.length; i++) { // Step 2: Loop through citations
        if (citations[i] >= i + 1) { // Step 3: Check if paper meets H-index condition
            h = i + 1; // Update h-index
        } else {
            break; // Stop if condition fails
        }
    }
    return h; // Step 4: Return the final h-index
};
```

---

### 🔍 R - Review

Try edge cases:

* Empty array → 0
* All zero citations → 0
* High citation values → check logic still holds

---

### 📈 E - Evaluate

* **Time Complexity**: O(n log n) (due to sorting)
* **Space Complexity**: O(1) (in-place sorting)
* **Strength**: Simple and intuitive
* **Weakness**: Can be optimized to O(n) using a counting approach

#
#

### 📗 UMPIRE 方法（中文版本）

### 🔍 U - Understand（理解）

給定一個整數陣列 `citations`，其中 `citations[i]` 代表第 i 篇論文被引用的次數，請你計算出該研究者的 **H-Index**。

**H-Index 定義**：最多有 `h` 篇論文，每篇至少被引用 `h` 次。

**範例：**

```
輸入：[3, 0, 6, 1, 5]
輸出：3
```

說明：有三篇論文至少被引用三次。

---

### 🧩 M - Match（對應）

這題本質上是「閾值判斷問題」，可以透過**排序後進行條件比對**，找出最大的 h 值滿足 H-Index 定義。

---

### 📝 P - Plan（規劃）

1. 將 `citations` 從大到小排序。
2. 初始化 `h = 0`。
3. 對排序後的每篇論文執行：

   * 如果 `citations[i] >= i + 1`，表示符合 H-index 定義，更新 `h = i + 1`
   * 否則中止迴圈。
4. 回傳 h。

---

### 💻 I - Implement（實作）

#### 🐍 Python

```python
from typing import List

class Solution:
    def hIndex(self, citations: List[int]) -> int:
        citations.sort(reverse=True)  # 步驟1：排序（由大到小）
        h = 0
        for i, c in enumerate(citations):  # 步驟2：逐一檢查
            if c >= i + 1:  # 步驟3：如果引用數 >= 當前論文數
                h = i + 1  # 更新 h-index
            else:
                break  # 若不符合，停止檢查
        return h  # 步驟4：回傳結果
```

#### 🌐 JavaScript

```javascript
var hIndex = function(citations) {
    citations.sort((a, b) => b - a); // 步驟1：排序（由大到小）
    let h = 0;
    for (let i = 0; i < citations.length; i++) { // 步驟2：遍歷每篇論文
        if (citations[i] >= i + 1) { // 步驟3：判斷是否符合 h-index
            h = i + 1; // 更新 h
        } else {
            break; // 若不符，立即跳出
        }
    }
    return h; // 步驟4：回傳結果
};
```

---

### 🔍 R - Review（回顧）

試幾個邊界測資來確認邏輯正確性：

* 空陣列 → 0
* 所有引用為 0 → 0
* 引用數過大 → 不影響 h-index 的定義，仍有效運作

---

### 📈 E - Evaluate（評估）

* **時間複雜度**：O(n log n)（排序）
* **空間複雜度**：O(1)（就地排序）
* **優點**：簡單、直覺、容易實作
* **缺點**：不是最佳效率，面試中可再補充 O(n) 解法做加分

