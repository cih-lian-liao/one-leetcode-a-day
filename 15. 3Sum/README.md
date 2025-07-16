
# 🧊 LeetCode 15: 3Sum — UMPIRE Method (English Version)

### 📖 Understand

#### What is the input?

* An integer array `nums`.

#### What is the output?

* A list of **unique** triplets `[nums[i], nums[j], nums[k]]` such that:

  * `i != j != k`
  * `nums[i] + nums[j] + nums[k] == 0`

#### Constraints:

* Do **not** include duplicate triplets.
* The order of elements within the triplet doesn’t matter.

---

### 🧩 Match

This problem is related to the **Two Pointer** technique combined with:

* **Sorting** (to allow efficient traversal)
* **Duplicate skipping** (to avoid repeated triplets)
* It extends the well-known **Two Sum** problem by adding a third fixed element.

---

### 🧠 Plan

1. **Sort** the array so we can use two pointers efficiently.
2. Loop `i` from `0` to `len(nums) - 2` to fix the first number.

   * If `nums[i] == nums[i - 1]`, skip to avoid duplicate triplets.
3. Use two pointers `left = i + 1` and `right = len(nums) - 1`:

   * If sum < 0 → move `left` to the right
   * If sum > 0 → move `right` to the left
   * If sum == 0 → save the triplet, move both pointers, and skip duplicates

---

### 🧑‍💻 Implement

#### 🐍 Python Version

```python
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # Step 1: Sort the array to prepare for two-pointer strategy
        res = []
        n = len(nums)

        for i in range(n - 2):  # Step 2: Fix the first element
            if i > 0 and nums[i] == nums[i - 1]:  # Step 2.1: Skip duplicates
                continue

            left, right = i + 1, n - 1  # Step 3: Initialize two pointers

            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    left += 1  # Need a larger sum, move left pointer
                elif total > 0:
                    right -= 1  # Need a smaller sum, move right pointer
                else:
                    # Found a valid triplet
                    res.append([nums[i], nums[left], nums[right]])

                    # Step 4: Skip duplicate values for left and right
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    # Move pointers inward
                    left += 1
                    right -= 1

        return res
```

---

#### 🌐 JavaScript Version

```javascript
var threeSum = function(nums) {
    nums.sort((a, b) => a - b); // Step 1: Sort the array
    const res = [];
    const n = nums.length;

    for (let i = 0; i < n - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue; // Step 2.1: Skip duplicates

        let left = i + 1;
        let right = n - 1; // Step 3: Two pointers

        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];

            if (sum < 0) {
                left++; // Too small → move left
            } else if (sum > 0) {
                right--; // Too big → move right
            } else {
                res.push([nums[i], nums[left], nums[right]]); // Found valid triplet

                // Step 4: Skip duplicates
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;

                left++;
                right--;
            }
        }
    }

    return res;
};
```

---

### 🧪 Review

* Test Input: `[-1, 0, 1, 2, -1, -4]`
* Sorted: `[-4, -1, -1, 0, 1, 2]`
* Valid Output: `[[-1, -1, 2], [-1, 0, 1]]`
* Duplicates avoided correctly via skipping logic

---

### 📊 Evaluate

* **Time Complexity**: `O(n^2)` — due to nested loops with two pointers
* **Space Complexity**: `O(1)` (excluding output space)

#
#
#

# 🍡 LeetCode 15：3Sum — UMPIRE 方法（中文版本）

### 📖 U — 理解題目 Understand

#### 輸入：

* 一個整數陣列 `nums`

#### 輸出：

* 所有不重複的三元組 `[nums[i], nums[j], nums[k]]`，滿足：

  * `i ≠ j ≠ k`
  * `nums[i] + nums[j] + nums[k] == 0`

#### 限制：

* 三元組不能重複
* 三個數字的順序不影響結果

---

### 🧩 M — 套用模板 Match

這題是：

* **Two Pointers 雙指針技巧**
* 搭配 **排序** 讓雙指針可以有效運行
* 延伸自經典 Two Sum 題目，但加入了第三個固定元素

---

### 🧠 P — 規劃 Plan

1. 對陣列進行排序（便於去除重複與使用雙指針）
2. 用 `i` 從頭到倒數第三個元素為止，固定第一個數字

   * 如果 `nums[i] == nums[i-1]`，就跳過（去重）
3. 設置 `left = i + 1`，`right = len(nums) - 1`：

   * 如果總和 < 0，`left++`
   * 如果總和 > 0，`right--`
   * 如果總和 == 0，記錄這個三元組，然後跳過重複的值，移動指針

---

### 🧑‍💻 I — 程式實作 Implement

#### 🐍 Python 寫法

```python
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # 步驟1：排序
        res = []
        n = len(nums)

        for i in range(n - 2):  # 固定第一個元素
            if i > 0 and nums[i] == nums[i - 1]:  # 跳過重複
                continue

            left, right = i + 1, n - 1  # 雙指針初始化

            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    left += 1  # 總和太小，移動左指針
                elif total > 0:
                    right -= 1  # 總和太大，移動右指針
                else:
                    # 成功找到一組
                    res.append([nums[i], nums[left], nums[right]])

                    # 跳過重複的 left 和 right
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    left += 1
                    right -= 1

        return res
```

---

#### 🌐 JavaScript 寫法

```javascript
var threeSum = function(nums) {
    nums.sort((a, b) => a - b); // 步驟1：排序
    const res = [];
    const n = nums.length;

    for (let i = 0; i < n - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue; // 跳過重複第一個元素

        let left = i + 1;
        let right = n - 1;

        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];

            if (sum < 0) {
                left++; // 太小往右
            } else if (sum > 0) {
                right--; // 太大往左
            } else {
                res.push([nums[i], nums[left], nums[right]]); // 找到一組解

                // 跳過重複的 left 和 right
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;

                left++;
                right--;
            }
        }
    }

    return res;
};
```

---

### 🔍 R — 複查 Review

測試輸入：

```python
[-1, 0, 1, 2, -1, -4]
```

排序後：

```python
[-4, -1, -1, 0, 1, 2]
```

正確輸出：

```python
[[-1, -1, 2], [-1, 0, 1]]
```

---

### 📊 E — 效能分析 Evaluate

* **時間複雜度**：`O(n^2)`（外層 + 內層 two pointers）
* **空間複雜度**：`O(1)`（如果不算輸出空間）


#
#
#

# 🧊 LeetCode 15: 3Sum – Interview Explanation (English + 中文對照筆記)

### 🧠 1. Clarify the Problem｜釐清題目

#### **ENGLISH**

"Alright, just to clarify, we are given an integer array called `nums`, and we need to return **all unique triplets** `[nums[i], nums[j], nums[k]]` such that the sum of the three numbers is zero.
The key conditions are:

* The indices `i`, `j`, and `k` must all be different.
* The triplets in the output must be **unique**, meaning we can’t return duplicate combinations.
  Also, the output does not have to be in any particular order."

#### **中文**

「我們要處理的題目是：給定一個整數陣列 `nums`，要找出所有**不重複的三元組** `[nums[i], nums[j], nums[k]]`，使得這三個數的總和為 0。
重要條件包括：

* `i`、`j`、`k` 不能是同一個 index
* 不能有重複的三元組（例如 `[−1, 0, 1]` 出現兩次不可以）
  而且三元組的順序沒有要求。」

---

### 🧩 2. Edge Cases｜邊界情況

#### **ENGLISH**

"Let me quickly think about the edge cases:

* If the array is empty or has fewer than 3 elements, there’s no way to form a triplet — return an empty list.
* If all elements are positive or all negative, it’s impossible to sum to 0.
* If there are many duplicates like `[0, 0, 0, 0]`, we should only return one triplet: `[0, 0, 0]`."

#### **中文**

「我先來思考一些邊界情況：

* 如果陣列為空，或元素少於三個，就不可能組成三元組，直接回傳空陣列。
* 如果全部是正數或全部是負數，也無法湊成 0。
* 如果像 `[0, 0, 0, 0]` 這種重複很多的情況，我們只能回傳一組 `[0, 0, 0]`。」

---

### 🔍 3. Brute Force vs Optimal Approach｜暴力法 vs 最佳解法

#### **ENGLISH**

"A brute-force method would involve three nested loops to check all triplets and test their sum. But that would be **O(n³)**, which is too slow.
Instead, I’ll use an optimized approach:

* First, **sort** the array
* Then, for each fixed number `nums[i]`, use a **two-pointer** strategy to find the other two numbers.
  This gives us a much better time complexity of **O(n²)**."

#### **中文**

「暴力法就是用三層巢狀迴圈去試每一組三元組並檢查總和是否為 0。但這樣會是 **O(n³)**，太慢了。
我們可以使用最佳解法：

* 首先把陣列排序
* 然後固定一個數 `nums[i]`，再用**雙指針**從兩邊往中間找其他兩個數
  這樣時間複雜度可以降到 **O(n²)**。」

---

### 👩‍💻 4. Code with Explanation｜寫程式並講解

#### 🐍 **Python Code with Explanation**

```python
def threeSum(nums):
    nums.sort()  # Step 1: Sort the array
    res = []     # Step 2: Initialize result list

    for i in range(len(nums) - 2):  # Step 3: Loop through array to fix first element
        if i > 0 and nums[i] == nums[i - 1]:
            continue  # Skip duplicate fixed elements

        left, right = i + 1, len(nums) - 1  # Step 4: Set two pointers

        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total < 0:
                left += 1  # Need larger sum → move left pointer right
            elif total > 0:
                right -= 1  # Need smaller sum → move right pointer left
            else:
                res.append([nums[i], nums[left], nums[right]])  # Found triplet

                # Step 5: Skip duplicates
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1

                left += 1
                right -= 1

    return res
```

#### **中文解說**

* 先排序整個陣列（必要條件）
* 固定一個 `i`，從 `i+1` 到右邊設置 `left` 和 `right` 指針
* 如果總和小於 0，左指針右移；大於 0，右指針左移；等於 0 則加入結果
* 同時跳過重複的數字以避免產生重複的三元組

---

#### 🌐 **JavaScript Code with Explanation**

```javascript
var threeSum = function(nums) {
    nums.sort((a, b) => a - b);  // Step 1: Sort the array
    const res = [];

    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;  // Skip duplicate `i`

        let left = i + 1, right = nums.length - 1;

        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];

            if (sum < 0) {
                left++;  // Too small → move left pointer
            } else if (sum > 0) {
                right--;  // Too big → move right pointer
            } else {
                res.push([nums[i], nums[left], nums[right]]);  // Found triplet

                // Skip duplicates
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;

                left++;
                right--;
            }
        }
    }

    return res;
};
```

#### **中文解說**

* 排序陣列以使用雙指針
* 外層固定一個元素，內層雙指針掃描其右側範圍
* 遇到總和為 0 的組合就加入結果
* 跳過重複元素來避免重複三元組

---

### ⏱️ 5. Time and Space Complexity｜時間與空間複雜度

#### **ENGLISH**

"Sorting takes **O(n log n)**, and the two-pointer loop takes **O(n²)** in the worst case, so overall it’s **O(n²)**.
We only use constant extra space except for the output list, so space complexity is **O(1)**."

#### **中文**

「排序是 **O(n log n)**，雙指針的迴圈最多是 **O(n²)**，所以整體是 **O(n²)**。
除了儲存結果的 list 以外，沒有額外空間需求，所以空間複雜度是 **O(1)**。」

---

### 💬 6. Follow-up Questions｜延伸問題

#### **ENGLISH**

* "Can you solve **4Sum** or **kSum**?"
  → Yes, 4Sum just adds another loop outside; kSum can be solved with recursion.
* "What if the array is already sorted?"
  → Then we can skip the sort step and save time.
* "What if we only need one valid triplet?"
  → We can break early after finding the first match.

#### **中文**

* 「你可以解 `4Sum` 或 `kSum` 嗎？」
  → 可以，4Sum 多加一層迴圈，kSum 可以用遞迴處理
* 「如果陣列已經排序好怎麼辦？」
  → 可以省略排序步驟，節省時間
* 「如果只需要一組解？」
  → 找到後可以提早 `return`，節省時間
