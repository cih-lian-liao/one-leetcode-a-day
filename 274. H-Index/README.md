
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

#
#
#


# 🧮 LeetCode 274 - H-Index (Counting Bucket O(n) Solution)

### 📘 UMPIRE Method (English Version)

### 🧠 U - Understand
We are given an integer array `citations`, where `citations[i]` represents the number of citations for the `i-th` paper.  
We are to return the **H-Index**, which is defined as the **maximum value h** such that there are **at least h papers with at least h citations**.

**Example:**
```

Input: [3, 0, 6, 1, 5]
Output: 3

````
There are 3 papers with at least 3 citations.

---

### 🧩 M - Match
This problem can be viewed as a **frequency count + threshold check** problem.  
We don’t need to sort. We can use a **counting bucket (histogram)** to record how many papers have each citation count.

---

### 📝 P - Plan

1. Let `n = len(citations)`, which is the number of papers.
2. Create a `bucket` array of size `n + 1`, where `bucket[i]` means "number of papers cited exactly i times".
3. If any paper has citation count >= n, we increment `bucket[n]` (since h-index cannot be more than n).
4. Traverse `bucket` from right to left (from high to low), and keep a running sum `total` of papers counted so far.
5. If `total >= i`, return `i` as the h-index.

---

### 💻 I - Implement

#### 🐍 Python
```python
from typing import List

class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        bucket = [0] * (n + 1)  # Step 1: Create bucket[0...n]

        # Step 2: Fill the bucket
        for c in citations:
            if c >= n:
                bucket[n] += 1  # Cap citations ≥ n into bucket[n]
            else:
                bucket[c] += 1

        # Step 3: Traverse from high to low
        total = 0
        for i in range(n, -1, -1):
            total += bucket[i]  # Accumulate papers with ≥ i citations
            if total >= i:
                return i  # Found the h-index

        return 0  # Default if no valid h-index found
````

#### 🌐 JavaScript

```javascript
var hIndex = function(citations) {
    const n = citations.length;
    const bucket = new Array(n + 1).fill(0); // Step 1: Create bucket[0...n]

    // Step 2: Fill the bucket
    for (let c of citations) {
        if (c >= n) {
            bucket[n]++; // Cap large citations into bucket[n]
        } else {
            bucket[c]++;
        }
    }

    // Step 3: Traverse from high to low
    let total = 0;
    for (let i = n; i >= 0; i--) {
        total += bucket[i]; // Count papers with ≥ i citations
        if (total >= i) {
            return i; // Found the h-index
        }
    }

    return 0; // Default
};
```

---

### 🔍 R - Review

**Test edge cases:**

* \[] → 0
* \[0, 0, 0] → 0
* \[100] → 1
* \[1, 3, 1] → 1
* \[0, 1, 4, 5, 6] → 3

---

### 📈 E - Evaluate

* **Time Complexity**: O(n) (one pass to fill the bucket, one pass to check)
* **Space Complexity**: O(n) (extra bucket array)
* **Advantage**: Optimized solution, avoids sorting
* **Limitation**: Needs extra space, less intuitive for beginners

---

### 📗 UMPIRE 方法（中文版本）

### 🧠 U - Understand（理解）

給你一個整數陣列 `citations`，每個元素代表某篇論文被引用的次數。請你找出該研究者的 **H-Index**。

**H-Index 定義**：最多有 `h` 篇論文，每篇至少被引用 `h` 次。

**範例：**

```
輸入：[3, 0, 6, 1, 5]
輸出：3
```

說明：有 3 篇論文，每篇至少被引用 3 次。

---

### 🧩 M - Match（對應）

這題可以視為**頻率統計 + 閾值判斷**的問題。
不需要排序，直接建立一個 **計數桶（bucket）** 來記錄每個引用次數出現的篇數。

---

### 📝 P - Plan（規劃）

1. 計算 `n = citations.length`。
2. 建立大小為 `n + 1` 的 bucket 陣列，`bucket[i]` 表示被引用 `i` 次的論文數。
3. 如果某篇論文引用次數 ≥ n，統一計入 `bucket[n]`（因為 H 最大不會超過 n）。
4. 從後往前（n 到 0）累加總數 total，代表「引用次數 ≥ i 的論文總數」。
5. 當 `total >= i` 時，即可回傳 i 作為 H-Index。

---

### 💻 I - Implement（實作）

#### 🐍 Python

```python
from typing import List

class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        bucket = [0] * (n + 1)  # 步驟1：建立 bucket 陣列

        # 步驟2：填入計數
        for c in citations:
            if c >= n:
                bucket[n] += 1  # 超過 n 的都歸入 bucket[n]
            else:
                bucket[c] += 1

        # 步驟3：從後往前累加 total
        total = 0
        for i in range(n, -1, -1):
            total += bucket[i]  # 累計被引用 ≥ i 次的論文數
            if total >= i:  # 符合 H-Index 定義
                return i

        return 0  # 預設回傳
```

#### 🌐 JavaScript

```javascript
var hIndex = function(citations) {
    const n = citations.length;
    const bucket = new Array(n + 1).fill(0); // 步驟1：建立 bucket

    // 步驟2：統計每個 citation 的次數
    for (let c of citations) {
        if (c >= n) {
            bucket[n]++; // 引用次數過大 → 歸入 bucket[n]
        } else {
            bucket[c]++;
        }
    }

    // 步驟3：從 n → 0 累加 total
    let total = 0;
    for (let i = n; i >= 0; i--) {
        total += bucket[i]; // 被引用 ≥ i 的論文總數
        if (total >= i) {
            return i; // 找到最大滿足條件的 h-index
        }
    }

    return 0; // 預設值
};
```

---

### 🔍 R - Review（回顧）

嘗試以下測資以驗證邏輯是否正確：

* 空陣列 `[]` → 0
* 全部引用為 0 `[0, 0, 0]` → 0
* 單篇高引用 `[100]` → 1
* 多篇引用不均 `[1, 3, 1]` → 1
* 測試混合情況 `[0, 1, 4, 5, 6]` → 3

---

### 📈 E - Evaluate（評估）

* **時間複雜度**：O(n)，只需兩次線性掃描
* **空間複雜度**：O(n)，需額外 bucket 陣列
* **優點**：最佳效能，不需排序，面試中加分解法
* **缺點**：較難理解，需熟悉 bucket 技巧與累加邏輯



