# 🐰 LeetCode 167: Two Sum II (UMPIRE METHOD)

### 📘 Understand

#### Problem

We are given a **sorted** array of integers in **non-decreasing order**, and a target number. We need to find two numbers such that their sum equals the target. Return the **1-based indices** of the two numbers.

#### Constraints:

* The array is sorted in **non-decreasing** order.
* Each input will have **exactly one solution**.
* Return the **indices (1-based)** of the two numbers.

---

### 🧩 Match

#### Pattern

* This is a **classic Two Pointer problem**.
* Since the input array is sorted, we can use two pointers to efficiently search for the pair.

---

### 📝 Plan

#### Step-by-step strategy:

1. Initialize two pointers:

   * `left` at the beginning (index 0)
   * `right` at the end (index len(nums) - 1)
2. While `left < right`:

   * Calculate `current_sum = numbers[left] + numbers[right]`
   * If `current_sum == target`, return `[left + 1, right + 1]`
   * If `current_sum < target`, move `left` pointer to the right.
   * If `current_sum > target`, move `right` pointer to the left.

#### Time Complexity: O(n)

#### Space Complexity: O(1)

---

### 💻 Implement

#### 🐍 Python

```python
from typing import List

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1

        while left < right:
            current_sum = numbers[left] + numbers[right]

            if current_sum == target:
                # Return 1-based indices
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1  # Move left pointer to the right
            else:
                right -= 1  # Move right pointer to the left

        return []  # This line is never reached due to the problem's guarantee
```

---

#### 🌐 JavaScript

```javascript
var twoSum = function(numbers, target) {
    let left = 0;
    let right = numbers.length - 1;

    while (left < right) {
        const currentSum = numbers[left] + numbers[right];

        if (currentSum === target) {
            // Return 1-based indices
            return [left + 1, right + 1];
        } else if (currentSum < target) {
            left++; // Move left pointer to the right
        } else {
            right--; // Move right pointer to the left
        }
    }

    return []; // This line is never reached
};
```

---

### 🔍 Review

* We leveraged the **sorted property** of the array to apply the two-pointer technique.
* The solution runs in linear time and uses constant space.
* Guaranteed to find one solution, so no need for extra validation.

---

### 📈 Evaluate

* ✅ Efficient and elegant
* ⚠️ Be careful to return **1-based indices**, not 0-based
* ❓Follow-up: What if the array is not sorted? (Would need a hash map instead.)

#
#
#

# 🐇 LeetCode 167: Two Sum II（中文版）

### 📘 理解題目

#### 題目說明

給定一個**遞增排序**的整數陣列 `numbers` 和一個目標值 `target`，請找出兩個數字的**和為 target**。你必須返回這兩個數字的**1-based index**（即從1開始計數）。

#### 限制條件：

* 輸入陣列是**排序好**的。
* 一定有且只有一組解。
* 必須返回這兩個數字的**1-based**索引。

---

### 🧩 關聯匹配

#### 題型模式

* 這是一道典型的**雙指針 Two Pointers** 題目。
* 因為陣列是排序的，所以可以使用頭尾雙指針進行搜尋。

---

### 📝 解題計畫

#### 解法邏輯：

1. 使用兩個指針：

   * `left` 從最左邊開始（index = 0）
   * `right` 從最右邊開始（index = len(nums) - 1）
2. 當 `left < right`：

   * 計算 `current_sum = numbers[left] + numbers[right]`
   * 如果 `current_sum == target`，返回 `[left + 1, right + 1]`
   * 如果 `current_sum < target`，將 `left` 向右移動
   * 如果 `current_sum > target`，將 `right` 向左移動

#### 時間複雜度：O(n)

#### 空間複雜度：O(1)

---

### 💻 程式碼實作

#### 🐍 Python

```python
from typing import List

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1

        while left < right:
            current_sum = numbers[left] + numbers[right]

            if current_sum == target:
                # 回傳 1-based index
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1  # 左指針右移
            else:
                right -= 1  # 右指針左移

        return []  # 題目保證一定有解，不會走到這裡
```

---

#### 🌐 JavaScript

```javascript
var twoSum = function(numbers, target) {
    let left = 0;
    let right = numbers.length - 1;

    while (left < right) {
        const currentSum = numbers[left] + numbers[right];

        if (currentSum === target) {
            // 回傳 1-based index
            return [left + 1, right + 1];
        } else if (currentSum < target) {
            left++; // 左指針右移
        } else {
            right--; // 右指針左移
        }
    }

    return []; // 題目保證一定有解，不會走到這裡
};
```

---

### 🔍 檢查與優化

* 本題使用雙指針技巧，效率高且程式簡潔。
* 題目保證一定會有解，所以不需要特別處理例外。
* 要特別小心的是：**回傳的是 1-based index，不是 0-based！**

---

### 📈 評估與延伸

* ✅ 漂亮的 O(n) 時間與 O(1) 空間解法
* 🤔 Follow-up：如果陣列不是排序的該怎麼辦？（需要使用哈希表）

#
#
#


# 🧩 LeetCode 167 — Two Sum II: Input Array is Sorted Interview Spoken Style English Explanation

### 🔍 Clarify the Problem | 釐清問題

#### 🗣️ English

"Alright, let me first make sure I understand the problem correctly.
We're given a sorted array of integers called `numbers`, and a target value. Our goal is to find **two numbers** in the array that add up to the target, and return their **indices** as a list.
One important detail is that the array is **1-indexed**, so when we return the indices, we need to add 1 to the 0-based positions.
Also, it's guaranteed that there is **exactly one solution**, and we **cannot use the same element twice**.

Does that sound correct before I move forward?"

#### 📘 中文

「好的，我先確認一下我對題目的理解是否正確。
我們會被給一個排序過的整數陣列 `numbers`，還有一個目標值 `target`。我們的任務是找到**兩個數字**，它們的總和等於 `target`，然後回傳它們的**索引**作為一個列表。
有一點要特別注意的是，這個陣列是**從1開始的索引**，所以我們回傳索引時要在原本的 0-based index 上加 1。
而且題目保證**一定有一個解**，並且**不能重複使用相同的元素**。

我可以繼續往下解釋嗎？」

---

### 🧊 Discuss Edge Cases | 討論邊界條件

#### 🗣️ English

"Let’s think about some edge cases, just to be safe.

* The array might contain negative numbers or zeros — so we shouldn't assume all numbers are positive.
* It might have only two numbers — the smallest possible size.
* The smallest and largest number might make up the target.

But since the problem says there’s always one solution, we don’t have to worry about missing or multiple solutions."

#### 📘 中文

「我們來想一下可能的邊界狀況，以確保穩妥。

* 陣列裡可能會有負數或零，所以我們不能假設所有數都是正數。
* 陣列可能只有兩個數字，這是最小的合法輸入大小。
* 可能是最小值和最大值剛好組成 `target`。

不過因為題目說**一定會有一個解**，所以我們不需要擔心找不到或有多個解這種情況。」

---

### 🧠 Consider Brute-Force and Optimal Approach | 思考暴力與最佳解法

#### 🗣️ English

"The brute-force way is to check every pair using a nested loop. For each `i`, try every `j > i`, and see if `numbers[i] + numbers[j]` equals the target. But that’s O(n²), which is too slow.

Luckily, the array is sorted — that’s a huge hint.
We can use the **two-pointer** approach:

* Start with `left` at index 0 and `right` at the end of the array.
* While `left < right`:

  * If the sum equals the target, return the indices.
  * If it’s less, move `left` to the right.
  * If it’s more, move `right` to the left."

#### 📘 中文

「暴力解法是用兩層迴圈去檢查每一組可能的數字對。對每個 `i`，試試看所有 `j > i`，看看 `numbers[i] + numbers[j]` 是否等於 `target`。但這樣的時間複雜度是 O(n²)，太慢了。

幸好陣列是排序過的，這是一個很大的提示。
我們可以使用\*\*雙指針（two-pointer）\*\*的方式來解：

* 設定 `left` 為起始位置，`right` 為陣列末端。
* 當 `left < right` 時：

  * 如果總和等於 target，就回傳索引。
  * 如果總和太小，就把 `left` 向右移。
  * 如果總和太大，就把 `right` 向左移。」

---

### 💻 Explain and Implement Code | 解釋並實作程式碼

#### ✅ Python Version:

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1

        while left < right:
            current_sum = numbers[left] + numbers[right]
            if current_sum == target:
                return [left + 1, right + 1]  # 1-based index
            elif current_sum < target:
                left += 1
            else:
                right -= 1

        return []
```

#### 🗣️ Spoken Explanation:

> "I first define `left` and `right` pointers.
> In the while loop, I calculate `current_sum`.
> If it matches the target, I return the 1-based indices.
> If the sum is too small, I move `left` rightward.
> If too large, I move `right` leftward.
> Since we’re guaranteed a solution, we’ll always find an answer."

#### 📘 中文解釋：

> 「我先定義左右兩個指針。
> 在 while 迴圈中，我計算兩者的總和。
> 如果剛好等於目標，就回傳 1-based 的索引。
> 如果太小，就把 `left` 往右移。
> 如果太大，就把 `right` 往左移。
> 因為題目保證有解，所以一定找得到。」

---

### ⏱️ Time and Space Complexity | 時間與空間複雜度

#### 🗣️ English

"Time complexity is O(n) because we move each pointer at most once.
Space complexity is O(1) since we’re only using two variables and no extra memory."

#### 📘 中文

「時間複雜度是 O(n)，因為每個指針最多走一遍。
空間複雜度是 O(1)，因為我們只用了兩個指針，沒用額外空間。」

---

### 💬 Follow-up Questions | 延伸問題

#### 🗣️ English

1. **What if the array isn’t sorted?**
   → We can’t use two pointers. Instead, use a hash map to store complements.

2. **What if multiple solutions are possible?**
   → We’d return a list of all valid pairs. Might need extra storage.

3. **Can we modify the array to do it in-place?**
   → Not necessary here, but for similar problems like removing duplicates, yes.

#### 📘 中文

1. **如果陣列沒有排序呢？**
   → 就不能用雙指針，可以用 hash map 存 complement。

2. **如果有多個解怎麼辦？**
   → 需要回傳所有符合條件的組合，可能會用到額外空間。

3. **這題可以修改原陣列來做嗎？**
   → 不需要。不過有些類似題（像是移除重複）會需要。

