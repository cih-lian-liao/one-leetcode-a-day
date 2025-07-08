
# LeetCode 88 - Merge Sorted Array (UMPIRE Method)

### 🧠 Problem Statement

You are given two sorted integer arrays `nums1` and `nums2`, with lengths `m` and `n` respectively.
`nums1` has a total length of `m + n`, where the last `n` elements are empty space (set to `0`) to accommodate `nums2`.
You need to **merge `nums2` into `nums1` in-place**, so that `nums1` becomes one sorted array.

---

### 🧠 U — Understand

* **Input:**

  * `nums1`: List\[int] of length `m + n`, where only the first `m` elements are valid
  * `nums2`: List\[int] of length `n`
  * `m`: Number of valid elements in `nums1`
  * `n`: Number of elements in `nums2`

* **Output:**

  * Modify `nums1` in-place to be the merged, sorted array

---

### 🔍 M — Match

This is a **two-pointer** problem, similar to the merge step in merge sort.
We are merging two sorted arrays, and need to do it in-place, without using extra space.

---

### 📝 P — Plan

1. Use three pointers:

   * `p1 = m - 1` → last valid index in `nums1`
   * `p2 = n - 1` → last index in `nums2`
   * `p = m + n - 1` → last index in `nums1` (for the merged array)
2. Compare `nums1[p1]` and `nums2[p2]`
3. Place the larger value at `nums1[p]` and move the corresponding pointer
4. After the loop, if there are remaining elements in `nums2`, copy them to the beginning of `nums1`

---

### 💻 I — Implement

```python
from typing import List

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        p1 = m - 1
        p2 = n - 1
        p = m + n - 1

        while p1 >= 0 and p2 >= 0:
            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1

        while p2 >= 0:
            nums1[p] = nums2[p2]
            p2 -= 1
            p -= 1
```

---

### ✅ R — Review

Test case:

```python
nums1 = [1,2,3,0,0,0]
m = 3
nums2 = [2,5,6]
n = 3

Solution().merge(nums1, m, nums2, n)
print(nums1)  # Output: [1,2,2,3,5,6]
```

---

### 🧮 E — Evaluate

* **Time Complexity:** O(m + n)
* **Space Complexity:** O(1) — in-place merge without extra memory

---

### ❓ Q\&A — Why Only `nums2` May Have Remaining Elements?

#### Question:

Why do we only need to check if `nums2` has remaining elements after merging, but not `nums1`?

#### Explanation:

We are filling `nums1` **from the back** (right to left).
At each step, we compare the largest unmerged values from both arrays and place the larger one at the end.

If `nums1[p1] > nums2[p2]`, we place `nums1[p1]` at the end and move `p1` left.
If `nums2[p2] >= nums1[p1]`, we place `nums2[p2]` and move `p2` left.

Eventually:

* If `nums2` is exhausted first, then all remaining `nums1` elements are **already in the correct position**, so no action is needed.
* If `nums1` is exhausted first, we need to copy the remaining elements of `nums2` into the beginning of `nums1`.

#### Example:

```python
nums1 = [4,5,6,0,0,0]
m = 3
nums2 = [1,2,3]
n = 3
```

We insert 6, then 5, then 4, then still have 1, 2, 3 in `nums2`, so we must copy them in.

#### Conclusion:

✅ Only `nums2` may have unmerged elements that need to be copied.
❌ `nums1`’s remaining elements are already in the right place and do not need to be moved.

---
---

# LeetCode 88 - 合併兩個有序陣列（UMPIRE 方法）

### 🧠 題目說明

給你兩個**升序排序**的整數陣列 `nums1` 和 `nums2`，它們的長度分別為 `m` 和 `n`。
`nums1` 的長度為 `m + n`，後面 `n` 個位置是用來容納 `nums2` 的。
請你**就地（in-place）合併**這兩個陣列，使 `nums1` 成為一個**有序**陣列。

---

### 🧠 U — Understand | 理解題目

* **輸入：**

  * `nums1`: 長度為 `m + n`，前 `m` 個是有效資料
  * `nums2`: 長度為 `n`
  * `m`: `nums1` 中的有效元素個數
  * `n`: `nums2` 中的元素個數

* **輸出：**

  * 直接修改 `nums1` 使其變成合併後的有序陣列

---

### 🔍 M — Match | 類型匹配

這是一道典型的 **雙指針（two-pointer）+ 從後往前填入** 類型的問題，
思路類似於 Merge Sort 中合併兩個有序陣列的過程。

---

### 📝 P — Plan | 解題計畫

1. 使用三個指標：

   * `p1 = m - 1`: 指向 `nums1` 中最後一個有效元素
   * `p2 = n - 1`: 指向 `nums2` 中最後一個元素
   * `p = m + n - 1`: 指向 `nums1` 的最後一個位置（即將填入的位置）

2. 比較 `nums1[p1]` 和 `nums2[p2]`：

   * 把較大的數放到 `nums1[p]` 中
   * 然後相應地移動指針

3. 如果 `nums2` 還有剩下的元素，就補到 `nums1` 的前面

---

### 💻 I — Implement | 程式碼實作 + 中文註解

```python
from typing import List

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        p1 = m - 1  # 指向 nums1 最後一個有效元素
        p2 = n - 1  # 指向 nums2 最後一個元素
        p = m + n - 1  # 指向 nums1 最後的空位

        # 從後面開始填入較大的數字，避免覆蓋未處理的資料
        while p1 >= 0 and p2 >= 0:
            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]  # 把 nums1[p1] 放到 nums1 的尾部
                p1 -= 1
            else:
                nums1[p] = nums2[p2]  # 把 nums2[p2] 放到 nums1 的尾部
                p2 -= 1
            p -= 1  # 移動合併後的尾部指標

        # 如果 nums2 還有剩下的，就補到 nums1 開頭（nums1 剩下的不需要動）
        while p2 >= 0:
            nums1[p] = nums2[p2]
            p2 -= 1
            p -= 1
```

---

### ✅ R — Review | 測試驗證

```python
nums1 = [1,2,3,0,0,0]
m = 3
nums2 = [2,5,6]
n = 3

Solution().merge(nums1, m, nums2, n)
print(nums1)  # 預期輸出: [1,2,2,3,5,6]
```

---

### 🧮 E — Evaluate | 時間與空間複雜度

* **時間複雜度：** `O(m + n)`
* **空間複雜度：** `O(1)`（原地操作，不使用額外空間）

---

### ❓ 常見問題：為什麼只需要處理 `nums2` 的剩餘元素？

#### 問題：

為什麼程式中最後只處理 `nums2` 的剩餘部分，卻不處理 `nums1`？

#### 解釋：

我們是**從後往前**填入最大的數字。每次比較 `nums1[p1]` 和 `nums2[p2]`，把較大的放到 `nums1[p]`：

* 如果 `nums1[p1]` 比較大 → 放進去並移動 `p1`
* 如果 `nums2[p2]` 比較大或相等 → 放進去並移動 `p2`

當有一邊的數字處理完了，另一邊可能還有剩下的數字。

* 如果 `nums2` 處理完了，`nums1` 剩下的數字**本來就在正確位置** → 不用動
* 如果 `nums1` 處理完了，`nums2` 還有 → 必須補上到前面

#### 範例：

```python
nums1 = [4,5,6,0,0,0]
m = 3
nums2 = [1,2,3]
n = 3
```

先填入 6、5、4（從後往前），此時 `nums1` 耗盡，但 `nums2` 還剩下 1、2、3，就要補上。

---

#### 結論：

✅ 只需要補上 `nums2` 的剩餘元素
❌ `nums1` 剩下的數已在正確位置，不需移動


