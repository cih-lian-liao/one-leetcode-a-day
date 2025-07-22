
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

#
#
#

# 🎤 Full Spoken-Style Interview Answer (Covers Both Algorithms)


### ✅ 1. Clarify the problem

> “Let me make sure I understand the problem correctly.

We’re given an array of integers called `citations`, where each element represents the number of times a research paper has been cited.
We need to calculate the researcher's **H-Index**, which is the maximum value `h` such that the researcher has at least `h` papers that have been cited at least `h` times.

So basically, it’s a threshold-finding problem where we’re trying to balance the number of papers with the number of citations each has.”

---

### 🧪 2. Discuss edge cases

> “Some edge cases I want to keep in mind are:

* An empty array should return 0, since there are no papers.
* If all values are 0, like `[0, 0, 0]`, the H-index is also 0.
* If a paper is cited more than the number of papers, we should cap its value when considering H-index, since H-index can’t exceed the number of papers.
* Another tricky example would be `[1, 3, 1]` — the answer is 1, not 2.”

---

### 🧱 3. Consider brute-force and optimal approach

> “The brute-force way would be to check for every possible h from 0 to n, whether there are at least h papers with at least h citations. But that would take O(n^2) time — not efficient.

There are two optimized approaches we can use here:

* **Approach 1**: Sort the array in descending order and check when the citation count drops below the paper’s rank. This gives us O(n log n) time.
* **Approach 2**: Use a bucket counting strategy to count how many papers fall into each citation range and scan from high to low to find the maximum valid H-index. This approach is O(n) time and uses O(n) space.”

---

## 🧠 4. Explain and implement optimal code

---

### 🔹 Approach 1: Sort-Based (O(n log n))

> “Let me first show you the sort-based solution.
> We sort the array in descending order so the most cited papers come first.
> We loop through each paper and compare its citation count to its rank (i + 1).
> If the citation count is greater than or equal to the rank, we update the H-index. If not, we break early.”

#### Python (with explanation)

```python
def hIndex(citations):
    citations.sort(reverse=True)  # Sort citations descending
    h = 0
    for i, c in enumerate(citations):
        if c >= i + 1:  # If this paper has at least (i+1) citations
            h = i + 1   # Update h-index
        else:
            break       # If not, stop checking
    return h
```

#### Spoken explanation:

> “Here, I sort the list in descending order. Then I go through each paper, and for each one, I check if the number of citations is at least the number of papers seen so far — that's `i + 1`.
> If that’s true, I update the H-index. Otherwise, I stop the loop early because no further papers can qualify. Finally, I return the last valid H-index.”

---

### 🔹 Approach 2: Counting Bucket (O(n))

> “Now let me show you the bucket-counting approach.
> This avoids sorting by using a frequency array to count how many papers have exactly 0 citations, 1 citation, and so on, up to `n` citations.

If any paper has more than `n` citations, we just cap it at `n` because H-index cannot exceed the number of papers.
We then loop backwards and accumulate how many papers have at least `i` citations.
Once that total count reaches or exceeds `i`, we’ve found the H-index.”

#### Python (with explanation)

```python
def hIndex(citations):
    n = len(citations)
    bucket = [0] * (n + 1)  # bucket[i] = number of papers with i citations

    # Count citations
    for c in citations:
        if c >= n:
            bucket[n] += 1  # Cap values ≥ n
        else:
            bucket[c] += 1

    total = 0
    for i in range(n, -1, -1):  # Check from high to low
        total += bucket[i]
        if total >= i:
            return i
    return 0
```

#### Spoken explanation:

> “I first create a bucket array of size `n + 1`, where each index `i` keeps track of how many papers have exactly `i` citations.
> If a paper has more than `n` citations, I cap it at bucket\[n].
> Then, I scan from right to left (from high citation counts to low), and keep a running sum of the number of papers with at least `i` citations.
> When that running total becomes greater than or equal to `i`, that means we found our H-index — and I return it.”

---

### ⏱️ 5. Discuss time and space complexity

#### Sort-based approach:

> * Time complexity: O(n log n) due to the sort
> * Space complexity: O(1) if the sort is done in-place

#### Bucket-counting approach:

> * Time complexity: O(n) — one pass to build the bucket, and one pass to scan from the end
> * Space complexity: O(n) — due to the bucket array

---

### 🤔 6. Mention follow-up questions

> “Some natural follow-up questions could be:

* How would you modify this if new citations are added dynamically?
* Can you compute H-index in a streaming fashion, without knowing the full array?
* What if citation data is extremely large — how do you reduce memory usage?
* How would you write unit tests for this?”

#
#
#

# 🧮 LeetCode 274 - H-Index | Interview Spoken Explanation 中英對照筆記


### 🗣️ 1. Clarify the Problem｜釐清題目

#### 📝 English
Let me make sure I understand the problem correctly.  
We’re given an array `citations`, where each element represents how many times a research paper has been cited.  
We need to calculate the **H-Index**, which is the maximum `h` such that the researcher has **at least `h` papers** with **at least `h` citations** each.

#### 📘 中文  
首先我想確認我對題目的理解是否正確：  
我們有一個整數陣列 `citations`，每個元素表示一篇論文被引用的次數。  
我們要計算出 **H-Index**，也就是「至少有 `h` 篇論文，每篇都被引用至少 `h` 次」的最大值。

---

### 🔎 2. Discuss Edge Cases｜討論邊界案例

#### 📝 English
Some edge cases I want to keep in mind are:
- Empty array → H-index is 0
- All citations are 0 → Result is also 0
- A single highly cited paper → Still capped by number of papers
- Mixed citations like [1, 3, 1] → Answer is 1, not 2

#### 📘 中文  
一些我會特別注意的邊界情況包括：
- 空陣列 → 回傳 0
- 所有引用都是 0 → H-Index 仍然是 0
- 僅一篇論文被大量引用 → H-index 最多還是 1（不超過論文數）
- 混合情況，例如 `[1, 3, 1]` → 正確答案是 1，而不是 2

---

### 🧱 3. Brute Force and Optimized Approaches｜暴力法與最佳解法

#### 📝 English
A brute-force solution would be to try all values of `h` from 0 to n, and check if at least `h` papers have ≥ `h` citations.  
But this is O(n^2) and inefficient.  
Instead, we can use:
- **Approach 1**: Sort the array and compare citations vs. index
- **Approach 2**: Use a bucket to count frequencies and check thresholds (O(n))

#### 📘 中文  
暴力解法是從 0 到 n 嘗試每個可能的 h 值，並檢查是否有至少 h 篇論文滿足條件，這會花費 O(n^2) 的時間，效率很低。  
所以我們可以使用以下兩種最佳化方式：
- **解法一**：排序後，用排序位置與引用數比較
- **解法二**：用計數桶統計每種引用數出現次數，再由高往低找符合條件的 h 值（O(n)）

---

### 📊 4. Approach 1: Sort-Based (O(n log n))｜排序解法

### 💬 Explanation

#### 📝 English
We sort the citations in descending order.  
We loop through each paper, and for paper at index `i`, we check if `citations[i] >= i + 1`.  
If yes, we update h to `i + 1`.  
If not, we break the loop and return the last valid h.

#### 📘 中文  
我們先將 citations 從大到小排序。  
然後依序遍歷每篇論文，對於第 `i` 篇論文，如果 `citations[i] >= i + 1`，就更新 h。  
否則代表已經不符合 H-Index 的定義，可以停止並回傳結果。

#### 🐍 Python Code
```python
def hIndex(citations):
    citations.sort(reverse=True)
    h = 0
    for i, c in enumerate(citations):
        if c >= i + 1:
            h = i + 1
        else:
            break
    return h
````

#### 🌐 JavaScript Code

```javascript
var hIndex = function(citations) {
    citations.sort((a, b) => b - a);
    let h = 0;
    for (let i = 0; i < citations.length; i++) {
        if (citations[i] >= i + 1) {
            h = i + 1;
        } else {
            break;
        }
    }
    return h;
};
```



### 🧮 5. Approach 2: Counting Bucket (O(n))｜計數桶解法

### 💬 Explanation

#### 📝 English

We create a bucket of size `n + 1`.
Each `bucket[i]` means "number of papers with exactly i citations".
If a paper has more than `n` citations, we put it in `bucket[n]`.
Then we accumulate total papers from the end, and when `total >= i`, we return `i` as the h-index.

#### 📘 中文

我們建立一個大小為 n + 1 的桶子陣列。
每個 `bucket[i]` 表示「被引用 i 次的論文數」。
若一篇論文被引用超過 n 次，就統一放入 `bucket[n]`。
接著從後往前累加，當總數 `total >= i` 時，即為 h-index。

#### 🐍 Python Code

```python
def hIndex(citations):
    n = len(citations)
    bucket = [0] * (n + 1)
    for c in citations:
        if c >= n:
            bucket[n] += 1
        else:
            bucket[c] += 1

    total = 0
    for i in range(n, -1, -1):
        total += bucket[i]
        if total >= i:
            return i
    return 0
```

#### 🌐 JavaScript Code

```javascript
var hIndex = function(citations) {
    const n = citations.length;
    const bucket = new Array(n + 1).fill(0);
    for (let c of citations) {
        if (c >= n) {
            bucket[n]++;
        } else {
            bucket[c]++;
        }
    }

    let total = 0;
    for (let i = n; i >= 0; i--) {
        total += bucket[i];
        if (total >= i) {
            return i;
        }
    }
    return 0;
};
```



### ⏱️ 6. Time and Space Complexity｜時間與空間複雜度分析

#### 📝 English

* Sort-based: O(n log n) time, O(1) space
* Bucket-based: O(n) time, O(n) space

#### 📘 中文

* 排序法：時間 O(n log n)，空間 O(1)
* 計數桶法：時間 O(n)，空間 O(n)

---

### 🧠 7. Follow-Up Questions｜延伸問題

#### 📝 English

* How would you handle dynamically added citations?
* Can we make it work in a streaming context?
* What if citation values are extremely large?

#### 📘 中文

* 如果新的引用數不斷新增，要如何處理？
* 若為串流資料，如何在線計算 H-Index？
* 如果 citation 數值非常大，有沒有空間最佳化方案？

