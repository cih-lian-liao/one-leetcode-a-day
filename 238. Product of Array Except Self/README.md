
# 🌟 LeetCode 238. Product of Array Except Self


### ✏️ English Version | UMPIRE Method

### ❓ U — Understand

We are given an integer array `nums`. We need to return an output array `answer`, where `answer[i]` is the product of all elements in `nums` except `nums[i]`.

Constraints:

* Do not use division.
* Time complexity must be O(n).

### 🔍 M — Match

This problem matches the **prefix/postfix product** pattern.
For each index `i`, the result can be computed as:

```text
answer[i] = product of all elements before i × product of all elements after i
```

We can compute this with:

* One forward pass to compute prefix products
* One backward pass to compute postfix products

### 🧠 P — Plan

1. Initialize an array `answer` with 1s.
2. Traverse `nums` from left to right.
   For each index `i`, set `answer[i]` to the prefix product before `i`.
3. Traverse `nums` from right to left.
   For each index `i`, multiply `answer[i]` with the postfix product after `i`.

### 💻 I — Implement

#### 🐍 Python

```python
from typing import List

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        answer = [1] * n  # Initialize result array with 1s

        # Step 1: Compute prefix product
        prefix = 1
        for i in range(n):
            answer[i] = prefix  # Store current prefix value
            prefix *= nums[i]   # Update prefix with current number

        # Step 2: Compute postfix product
        postfix = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= postfix  # Multiply with postfix value
            postfix *= nums[i]    # Update postfix

        return answer
```

#### 🌐 JavaScript

```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const answer = new Array(n).fill(1); // Initialize with 1s

    // Step 1: Compute prefix product
    let prefix = 1;
    for (let i = 0; i < n; i++) {
        answer[i] = prefix;      // Store prefix at current index
        prefix *= nums[i];       // Update prefix
    }

    // Step 2: Compute postfix product
    let postfix = 1;
    for (let i = n - 1; i >= 0; i--) {
        answer[i] *= postfix;    // Multiply with postfix
        postfix *= nums[i];      // Update postfix
    }

    return answer;
};
```

### 🔁 R — Review

Test with `nums = [1, 2, 3, 4]`
Expected output: `[24, 12, 8, 6]`

### 📈 E — Evaluate

* Time: O(n) — 2 passes over the array
* Space: O(1) — ignoring the output array
* No division used ✅

---

### ✏️ 中文版本 | UMPIRE 解題法

### ❓ U — 了解問題

給定一個整數陣列 `nums`，回傳一個新陣列 `answer`，其中 `answer[i]` 是「除了 `nums[i]` 自己以外」其他所有元素的乘積。

條件：

* **不能用除法**
* **時間複雜度 O(n)**

---

### 🔍 M — 對應模式

這題是典型的「前綴乘積 / 後綴乘積」模式。
對於每個位置 `i`，我們可以這樣計算：

```text
answer[i] = i 左邊所有數的乘積 × i 右邊所有數的乘積
```

這可以透過兩次遍歷來完成：

* 第一次從左往右計算 prefix（前綴乘積）
* 第二次從右往左計算 postfix（後綴乘積）

---

### 🧠 P — 解題計畫

1. 建立一個與 `nums` 長度相同、值全為 1 的 `answer` 陣列
2. 從左到右走，計算每個位置左邊的乘積（prefix），放到 `answer[i]`
3. 從右到左走，邊計算右邊乘積（postfix），邊乘進 `answer[i]` 中

---

### 💻 I — 實作代碼

#### 🐍 Python

```python
from typing import List

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        answer = [1] * n  # 初始化一個全為 1 的結果陣列

        # 第一步：前綴乘積
        prefix = 1
        for i in range(n):
            answer[i] = prefix        # 記錄目前的 prefix
            prefix *= nums[i]        # 更新 prefix

        # 第二步：後綴乘積
        postfix = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= postfix     # 把後綴乘積乘進答案裡
            postfix *= nums[i]       # 更新 postfix

        return answer
```

#### 🌐 JavaScript

```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const answer = new Array(n).fill(1); // 初始化答案陣列為 [1,1,1,...]

    // 第一步：前綴乘積
    let prefix = 1;
    for (let i = 0; i < n; i++) {
        answer[i] = prefix;         // 記錄 prefix
        prefix *= nums[i];          // 更新 prefix 為目前值
    }

    // 第二步：後綴乘積
    let postfix = 1;
    for (let i = n - 1; i >= 0; i--) {
        answer[i] *= postfix;       // 把 postfix 乘進答案
        postfix *= nums[i];         // 更新 postfix
    }

    return answer;
};
```

---

### 🔁 R — 檢查驗證

以 `nums = [1, 2, 3, 4]` 為例：

1. prefix: \[1, 1, 2, 6]
2. postfix: \[24, 12, 4, 1]
   最後答案為 `[24, 12, 8, 6]` ✅

---

### 📈 E — 效能分析

* **時間複雜度**：O(n)，前後各一次遍歷
* **空間複雜度**：O(1)，除了輸出陣列外沒有額外空間
* **滿足題目條件**：不使用除法 ✔️、線性時間 ✔️、優雅簡潔 ✔️

#
#
#

# 🎤 Full Spoken-Style Interview Answer

**LeetCode 238 - Product of Array Except Self**

### 1️⃣ Clarify the Problem

> "Let me make sure I understand the problem correctly.
> We are given an array of integers called `nums`, and for each index `i`, we need to return the product of all elements in the array except `nums[i]`.
> We're told not to use division, and we need to solve it in linear time, O(n).
> Is that correct?"

---

### 2️⃣ Discuss Edge Cases

> "Some edge cases I’d consider are:

* When the array has only one element — in that case, since there are no other elements, maybe we should return `[1]` by default.
* What if the array contains zeros? If there's one zero, then all elements except the one at the zero’s index should be zero. If there are two or more zeros, then the entire result should be all zeros.
* Negative numbers — we should make sure the logic still works with negatives, since the sign of the product will matter."

---

### 3️⃣ Consider Brute-Force and Optimal Approach

> "The brute-force approach would be to loop through each element and multiply all the other elements, skipping the current one. But that would take O(n²) time, which is too slow.

Since we can’t use division, we need to compute the result using prefix and postfix products. The key idea is:
For each index `i`,

* The product of all elements before `i` is the prefix
* The product of all elements after `i` is the postfix
  So, `output[i] = prefix[i] * postfix[i]`

We can compute this in two passes:

* First pass: compute prefix products from left to right
* Second pass: compute postfix products from right to left and multiply into the result"

---

### 4️⃣ Explain and Implement Optimal Code

> "Here’s how I’d implement the optimal solution.
> I'll walk through the code step by step, and explain what each part is doing."

#### 🐍 Python version (spoken style)

```python
def productExceptSelf(nums):
    n = len(nums)
    output = [1] * n

    # First, compute prefix products
    prefix = 1
    for i in range(n):
        output[i] = prefix
        prefix *= nums[i]

    # Then compute postfix products and multiply into output
    postfix = 1
    for i in range(n - 1, -1, -1):
        output[i] *= postfix
        postfix *= nums[i]

    return output
```

> "So here I’m initializing an output array with 1s.
> In the first loop, I use a variable `prefix` to store the product of all elements before index `i`, and assign that to `output[i]`.
> Then I update `prefix` to include `nums[i]` so it’s ready for the next iteration.

In the second loop, I go from right to left and use `postfix` to represent the product of all elements after index `i`.
I multiply that into `output[i]`, which already contains the prefix product.
Finally, I return the `output` array."

---

### 5️⃣ Discuss Time and Space Complexity

> "Time complexity is O(n), since we’re doing two passes through the array.
> Space complexity is O(1) if we don’t count the output array, because we only use two extra variables — `prefix` and `postfix`."

---

### 6️⃣ Mention Follow-up Questions

> "Some follow-up questions we might get are:

* What if we were allowed to use division? That would make the solution simpler, but we’d have to handle zeros carefully.
* Can you do this in a single pass?
* Can you solve it in-place without even using an output array?
  Also, maybe in an interview we could be asked to write a version that works for linked lists or streams."

#
#
#

# 🎤 LeetCode 238. Product of Array Except Self — Spoken-Style Interview Answer 英文面試口語筆記

### ❓ 1. Clarify the Problem | 釐清問題

#### 🗣️ English

Let me make sure I understand the problem correctly.
We are given an array of integers called `nums`, and for each index `i`, we need to return the product of all elements in the array **except `nums[i]`**.
We're not allowed to use division, and the solution should run in **O(n)** time.

#### 🧾 中文

我想先確認我對這題的理解是否正確：
給定一個整數陣列 `nums`，對於每個位置 `i`，我們要返回除了 `nums[i]` 本身以外，其他所有元素的乘積。
不能使用除法，並且解法的時間複雜度必須是 O(n)。

---

### 🧊 2. Edge Cases | 邊界情況

#### 🗣️ English

Some edge cases to consider:

* If the array has only one element, we may just return `[1]` by default.
* If there's one `0`, then every result will be `0` except at the index with the zero.
* If there are two or more zeros, all results should be zero.
* The array may contain negative numbers; the sign of the product must be correct.

#### 🧾 中文

以下是我會考慮的一些邊界情況：

* 陣列中只有一個元素時，可能直接回傳 `[1]`。
* 若陣列中有一個 `0`，那麼除了 `0` 本身的位置，其他位置的乘積都會是 0。
* 若有兩個以上的 `0`，則整個輸出陣列都應該是 0。
* 陣列中若有負數，乘積的正負號要處理正確。

---

### 🧱 3. Brute Force vs Optimal | 暴力解與最佳解

#### 🗣️ English

A brute-force solution would be to loop through the array and, for each index, multiply all the other elements — skipping `nums[i]`.
But this takes O(n²) time, which is inefficient.

Instead, we can use the **prefix and postfix product** idea:

* From the left side, we record the product of all elements before index `i`
* From the right side, we do the same for all elements after index `i`

Multiply these two together to get the result for `answer[i]`.

#### 🧾 中文

暴力解法是對每個元素 `i`，重新遍歷整個陣列，乘上除了 `nums[i]` 以外的其他數。但這樣的時間複雜度是 O(n²)，太慢了。

更好的方法是使用「前綴乘積」與「後綴乘積」的技巧：

* 從左邊先記錄每個位置左邊的所有數字乘積（不包含自己）
* 再從右邊記錄每個位置右邊的乘積
* 最後左右相乘就得到 `answer[i]`

---

### 🧠 4. Optimal Solution & Code Walkthrough | 最佳解與程式碼說明

#### 🗣️ English

Let me walk you through the code step by step.

##### 🐍 Python Implementation:

```python
def productExceptSelf(nums):
    n = len(nums)
    output = [1] * n  # Initialize with 1s

    # Prefix product
    prefix = 1
    for i in range(n):
        output[i] = prefix      # Assign current prefix
        prefix *= nums[i]       # Update prefix

    # Postfix product
    postfix = 1
    for i in range(n - 1, -1, -1):
        output[i] *= postfix    # Multiply with postfix
        postfix *= nums[i]      # Update postfix

    return output
```

##### 🌐 JavaScript Implementation:

```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const output = new Array(n).fill(1);

    // Prefix product
    let prefix = 1;
    for (let i = 0; i < n; i++) {
        output[i] = prefix;
        prefix *= nums[i];
    }

    // Postfix product
    let postfix = 1;
    for (let i = n - 1; i >= 0; i--) {
        output[i] *= postfix;
        postfix *= nums[i];
    }

    return output;
};
```

#### 🧾 中文

我現在來一步步說明程式邏輯。

* 第一段：我們先建立一個全為 1 的陣列，然後用 `prefix` 累積從左邊來的乘積。
* 第二段：從右邊開始，用 `postfix` 去乘進 `output[i]` 裡。
* 因為我們一開始就把左乘積放進 `output[i]`，現在再乘進右邊的 postfix，就得到答案。

這樣就不需要用到除法，空間也只用了兩個額外變數。

---

### ⏱️ 5. Time & Space Complexity | 時間與空間複雜度

#### 🗣️ English

* Time complexity is **O(n)** because we traverse the array twice.
* Space complexity is **O(1)** if we exclude the output array.
* No division is used ✅

#### 🧾 中文

* 時間複雜度是 O(n)，因為只遍歷兩次陣列。
* 空間複雜度是 O(1)，除了輸出陣列以外沒有額外使用空間。
* 沒有使用除法，符合題目要求 ✅

---

### 💭 6. Follow-up Questions | 延伸問題

#### 🗣️ English

Some follow-up questions could be:

* What if division was allowed?
* Can we solve it in one pass?
* Can we reduce memory even further, maybe in-place?
* How would you extend this to a linked list?

#### 🧾 中文

可能的延伸提問包含：

* 如果允許使用除法，會怎麼改寫？
* 有辦法只用一次遍歷解決嗎？
* 能不能再減少空間使用？例如就地（in-place）處理？
* 如果輸入是一個 linked list 呢？還能用這種解法嗎？

#
#
#

### ❓WHY｜為什麼要練習這道題

#### 🗣️ English

This problem is a classic example of how to manipulate arrays with minimal space while avoiding the use of division. It’s often asked in interviews to test whether a candidate can recognize and implement **prefix/postfix scanning**, handle **in-place transformations**, and reason about **time-space tradeoffs**.

#### 🧾 中文

這題是一個經典的考點，目的是訓練你在不使用除法的前提下，如何有效地處理陣列轉換，並控制空間使用。它常出現在面試中，用來測試候選人是否能掌握 **前綴/後綴掃描** 的技巧、是否能設計 **就地轉換（in-place）** 的演算法，以及是否能理解 **時間與空間複雜度的權衡**。

---

### 🎯 Real-World Applications｜實際應用場景

#### 🗣️ English

1. **Data Normalization** in analytics pipelines:
   Suppose you want to compute a value for each element that represents its weight relative to the rest of the dataset, excluding itself. This "all-others" pattern often appears in statistics or ML feature transformations.

2. **Inventory Adjustment in E-Commerce Platforms**:
   In systems like Amazon, you may want to compute the product sales forecast *excluding* a certain category or item, which requires aggregate calculations minus one component.

3. **Financial Risk Modeling**:
   In portfolio simulations, when evaluating risk or exposure, we often compute metrics that exclude one asset to simulate leave-one-out scenarios — similar to how this problem omits one element at a time.

4. **Parallel Processing or Distributed Systems**:
   When you need to compute results that depend on all elements *except* the current node or partition — such as calculating partial sums, balancing workloads, or rerouting requests excluding the current shard.

#### 🧾 中文

1. **資料正規化處理（Data Normalization）**：
   在資料分析或機器學習前處理中，常需要對每個元素進行轉換，例如計算「相對其他元素的比重」，而這種排除自己、使用其他元素的模式正是這題的核心。

2. **電商平台中的庫存/銷售計算**：
   在 Amazon 等電商系統中，可能需要計算「排除某個商品類別」的總銷售量預測，這會牽涉到「總和排除某一項」的處理邏輯，類似這題的結構。

3. **金融風險建模**：
   在模擬投資組合風險時，常用「leave-one-out」策略，也就是每次排除一個資產，計算剩下資產的總風險貢獻，這與本題的排除某元素後再計算乘積的概念非常接近。

4. **平行運算與分散式系統設計**：
   在分散式系統中，有時需針對某個節點或資料分片以外的其他部分進行總計、再分配或錯誤處理。這類「排除自己、統計他人」的模式也能用本題的 prefix/postfix 技巧來實現。


