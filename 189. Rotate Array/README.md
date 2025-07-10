
# 📦 LeetCode 189 — Rotate Array (UMPIRE Method - English)

### 🔍 Problem Statement

#### Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

#### Example:

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
```

---

### 🧠 U — Understand

#### Input:

* A non-empty list of integers `nums`
* An integer `k` representing the number of steps to rotate

#### Output:

* The array should be rotated in-place by `k` steps to the right

#### Constraints:

* `1 <= nums.length <= 10^5`
* `-2^31 <= nums[i] <= 2^31 - 1`
* `0 <= k <= 10^5`

---

### 🧩 M — Match

#### Problem Type:

* Array manipulation
* Rotation / In-place modification

#### Algorithm:

* Use the **three-step reverse method**

  1. Reverse the entire array
  2. Reverse the first `k` elements
  3. Reverse the remaining `n - k` elements

---

### 📝 P — Plan

#### Steps:

1. Normalize `k` by taking `k % n` where `n` is the array length
2. Reverse the entire array
3. Reverse the first `k` elements
4. Reverse the rest

---

### 👩‍💻 I — Implement

#### ✅ Python Code

```python
from typing import List

class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n  # Normalize k

        def reverse(start, end):
            while start < end:
                nums[start], nums[end] = nums[end], nums[start]
                start += 1
                end -= 1

        reverse(0, n - 1)   # Step 1: Reverse entire array
        reverse(0, k - 1)   # Step 2: Reverse first k elements
        reverse(k, n - 1)   # Step 3: Reverse the rest
```

#### ✅ JavaScript Code

```javascript
var rotate = function(nums, k) {
    const n = nums.length;
    k %= n; // Normalize k

    const reverse = (start, end) => {
        while (start < end) {
            [nums[start], nums[end]] = [nums[end], nums[start]];
            start++;
            end--;
        }
    };

    reverse(0, n - 1);  // Step 1
    reverse(0, k - 1);  // Step 2
    reverse(k, n - 1);  // Step 3
};
```

---

### 🔁 R — Review

#### Test Case 1:

```text
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
```

#### Test Case 2:

```text
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
```

---

### 📊 E — Evaluate

#### Time Complexity:

* O(n) — Each element is swapped at most once

#### Space Complexity:

* O(1) — In-place operation, no extra space used

---

# 📦 LeetCode 189 — 旋轉陣列（UMPIRE 方法 - 中文版）

### 🔍 題目敘述

#### 給定一個整數陣列 `nums`，請將陣列向右旋轉 `k` 步，其中 `k` 是非負整數。

#### 範例：

```
輸入: nums = [1,2,3,4,5,6,7], k = 3
輸出: [5,6,7,1,2,3,4]
```

---

### 🧠 U — 理解（Understand）

#### 輸入：

* 一個非空整數陣列 `nums`
* 一個整數 `k`，表示向右旋轉的步數

#### 輸出：

* 原地（in-place）修改陣列 `nums` 的內容

#### 限制條件：

* `1 <= nums.length <= 10^5`
* `0 <= k <= 10^5`
* 題目要求不能使用額外的陣列空間（空間複雜度 O(1)）

---

### 🧩 M — 類型匹配（Match）

#### 類型：

* 陣列操作
* 原地旋轉

#### 解法：

使用「**三次反轉法**」：

1. 反轉整個陣列
2. 反轉前 `k` 個元素
3. 反轉後 `n - k` 個元素

---

### 📝 P — 解題計畫（Plan）

#### 步驟說明：

1. 計算 `k = k % n`，防止超出長度
2. 反轉整個陣列
3. 反轉前 `k` 個元素
4. 反轉剩下的元素

---

### 👩‍💻 I — 程式碼實作（Implement）

#### ✅ Python 實作：

```python
from typing import List

class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n  # 正規化 k

        def reverse(start, end):
            while start < end:
                nums[start], nums[end] = nums[end], nums[start]
                start += 1
                end -= 1

        reverse(0, n - 1)   # 第一步：整體反轉
        reverse(0, k - 1)   # 第二步：反轉前 k 個
        reverse(k, n - 1)   # 第三步：反轉剩下的
```

#### ✅ JavaScript 實作：

```javascript
var rotate = function(nums, k) {
    const n = nums.length;
    k %= n; // 正規化 k

    const reverse = (start, end) => {
        while (start < end) {
            [nums[start], nums[end]] = [nums[end], nums[start]];
            start++;
            end--;
        }
    };

    reverse(0, n - 1);  // 步驟一
    reverse(0, k - 1);  // 步驟二
    reverse(k, n - 1);  // 步驟三
};
```

---

### 🔁 R — 測試（Review）

#### 測資 1：

```text
輸入: nums = [1,2,3,4,5,6,7], k = 3
輸出: [5,6,7,1,2,3,4]
```

#### 測資 2：

```text
輸入: nums = [-1,-100,3,99], k = 2
輸出: [3,99,-1,-100]
```

---

### 📊 E — 評估（Evaluate）

#### 時間複雜度：

* O(n)，每個元素最多被交換一次

#### 空間複雜度：

* O(1)，僅使用常數空間
