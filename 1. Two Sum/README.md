
# 🐰 LeetCode 1 — Two Sum (UMPIRE Method) — English Version

### 🧠 Understand

#### Problem

Given an array of integers `nums` and an integer `target`, return **indices** of the two numbers such that they **add up to `target`**.
Each input will have **exactly one solution**, and you may not use the same element twice.

#### Input

* `nums`: List of integers
* `target`: Integer

#### Output

* A list of two indices `[i, j]` such that `nums[i] + nums[j] == target`

---

### 🧩 Match

This is a classic **hash map lookup** problem.
We're searching for a **complementary pair** — as we iterate, we want to check if the "needed number" has already appeared before.
Hash maps allow **constant-time lookups**, making the solution efficient.

---

### 📝 Plan

1. Initialize an empty dictionary `seen` to store `{number: index}`.
2. Iterate through the array with both index `i` and value `num`.
3. For each number, calculate the `complement = target - num`.
4. If `complement` exists in `seen`, return its index and `i`.
5. Otherwise, store the current number and index in `seen`.

---

### 👨‍💻 Implement

#### 🐇 Python Code with Comments

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # Store number and its index

        for i, num in enumerate(nums):
            complement = target - num  # What number we need to reach the target
            if complement in seen:
                return [seen[complement], i]  # Return indices if complement is found
            seen[num] = i  # Otherwise, store this number for future lookups
```

#### 🐣 JavaScript Code with Comments

```javascript
var twoSum = function(nums, target) {
    const seen = {}; // Store number as key and index as value

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i]; // What we need to reach target
        if (seen.hasOwnProperty(complement)) {
            return [seen[complement], i]; // Return indices if match is found
        }
        seen[nums[i]] = i; // Store current number and index
    }
};
```

---

### 🔍 Review

#### Test Cases

```python
# Test Case 1
nums = [2, 7, 11, 15]
target = 9
# Output: [0, 1]

# Test Case 2
nums = [3, 2, 4]
target = 6
# Output: [1, 2]

# Test Case 3
nums = [3, 3]
target = 6
# Output: [0, 1]
```

---

### 📊 Evaluate

* **Time Complexity**: O(n)
  One pass through the array with constant-time hash map operations.
* **Space Complexity**: O(n)
  We store at most `n` elements in the hash map.

#
#
#

# 🐱 LeetCode 1 — Two Sum (UMPIRE 方法) — 中文版本

### 🧠 Understand

#### 題目說明

給你一個整數陣列 `nums` 和一個整數 `target`，請你回傳兩個加起來等於 `target` 的數字之**索引值**。
每組輸入保證有且僅有一組解，且不能重複使用同一元素。

#### 輸入

* `nums`: 整數陣列
* `target`: 目標整數

#### 輸出

* 索引值的陣列 `[i, j]`，滿足 `nums[i] + nums[j] == target`

---

### 🧩 Match

這是一道經典的「哈希表」查找題。
目標是找出每個數字的 **補數**（`target - num`）是否已經在之前的陣列中出現過。
使用哈希表（字典）可以實現 **常數時間查找**，使整體效率變成 O(n)。

---

### 📝 Plan

1. 建立一個空字典 `seen`，用來儲存 `{數字: 索引}`
2. 使用 `enumerate()` 遍歷 `nums`，取得每個索引與數字
3. 每次計算補數 `complement = target - num`
4. 如果補數在 `seen` 中，代表已經找到答案，回傳 `[seen[complement], i]`
5. 否則，將 `num` 和其索引存入 `seen` 中以供後續查找

---

### 👨‍💻 Implement

#### 🐇 Python 程式碼（含註解）

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # 用來儲存數字與其索引的對應關係

        for i, num in enumerate(nums):
            complement = target - num  # 所需補數
            if complement in seen:
                return [seen[complement], i]  # 找到補數，回傳兩個索引
            seen[num] = i  # 尚未找到，先將此數字加入字典中
```

#### 🐣 JavaScript 程式碼（含註解）

```javascript
var twoSum = function(nums, target) {
    const seen = {}; // 建立物件儲存數字與索引對應關係

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i]; // 補數
        if (seen.hasOwnProperty(complement)) {
            return [seen[complement], i]; // 補數存在，回傳索引
        }
        seen[nums[i]] = i; // 補數不存在，記錄目前數字與索引
    }
};
```

---

### 🔍 Review

#### 測試案例

```python
# 案例 1
nums = [2, 7, 11, 15]
target = 9
# 輸出: [0, 1]

# 案例 2
nums = [3, 2, 4]
target = 6
# 輸出: [1, 2]

# 案例 3
nums = [3, 3]
target = 6
# 輸出: [0, 1]
```

---

### 📊 Evaluate

* **時間複雜度**：O(n)
  只需一次遍歷，查找補數為常數時間
* **空間複雜度**：O(n)
  最多需要儲存 `n` 個元素於字典中

#
#
#

# 🧸 LeetCode 1: Two Sum — Interview Speaking Notes (EN/中)

### 🎯 Clarify the Problem  
#### ✅ English  
"Alright, just to make sure I understand the problem correctly:  
We are given an array of integers, called `nums`, and an integer `target`.  
Our task is to return the **indices** of two numbers in the array such that their sum equals the `target`.  

We’re also told that:
- Each input will have **exactly one solution**
- And we **can’t use the same element twice**  
For example, if `nums = [2, 7, 11, 15]` and `target = 9`, then we return `[0, 1]` because `nums[0] + nums[1] = 2 + 7 = 9`.

Is that correct?”

#### ✅ 中文  
「好的，為了確認我是否正確理解題目：  
給我們一個整數陣列 `nums` 和一個整數 `target`，  
我們需要找出陣列中兩個數字的**索引**，使得它們的和等於 `target`。

題目還說明了：
- 每個輸入一定會有**一組唯一的解**
- 我們**不能重複使用相同的元素**  
例如：`nums = [2, 7, 11, 15]`，`target = 9`，答案是 `[0, 1]`，因為 `2 + 7 = 9`。」

---

### 🐰 Discuss Edge Cases  
#### ✅ English  
"Let’s think about some edge cases.  
If the array is empty or has only one element, it’s impossible to find a pair.  
But the problem guarantees that there is exactly one solution,  
so we don’t need to handle empty or invalid cases explicitly.  

Still, we should test cases like `[3, 3]` and `target = 6` to ensure we don’t use the same index twice."

#### ✅ 中文  
「來討論一些邊界情況：  
如果陣列是空的或只有一個元素，那就不可能找到一對數字。  
但題目保證一定有一個解，因此我們不需要特別處理無效的輸入。

但我們還是應該測試像 `[3, 3]` 且目標是 `6` 的情況，確保不會重複使用相同的索引。」

---

### 🍓 Brute-Force vs Optimal  
#### ✅ English  
"The brute-force way is to check all pairs with nested loops, which takes O(n²) time.  
Instead, we can use a hash map to record the values we've seen and their indices.  
As we iterate, we check if the `complement = target - num` exists in the map."

#### ✅ 中文  
「暴力解法是使用雙層迴圈檢查所有數對，時間複雜度為 O(n²)。  
更好的方法是使用 Hash Map（字典）來記錄我們看過的數值及其索引。  
每次遍歷時，計算 `complement = target - num`，並查看它是否已經存在於 map 中。」

---

### 🐥 Explain + Implement  
#### ✅ English  
"Here is the optimal Python solution with explanation:"  

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # Create a dictionary to map number -> index

        for i, num in enumerate(nums):
            complement = target - num  # Calculate what number we need to reach target
            if complement in seen:
                return [seen[complement], i]  # Found the pair, return indices
            seen[num] = i  # Otherwise, store current number and its index
````

"And the JavaScript version:"

```javascript
function twoSum(nums, target) {
    const seen = {};  // Map number to index

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (seen.hasOwnProperty(complement)) {
            return [seen[complement], i];  // Found the answer
        }
        seen[nums[i]] = i;  // Store the index of current number
    }
}
```

#### ✅ 中文

「以下是最佳的 Python 解法與說明：」

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # 建立一個字典，將數字映射到其索引

        for i, num in enumerate(nums):
            complement = target - num  # 計算補數
            if complement in seen:
                return [seen[complement], i]  # 找到答案，回傳索引
            seen[num] = i  # 否則，儲存目前的數字及其索引
```

「JavaScript 版本如下：」

```javascript
function twoSum(nums, target) {
    const seen = {};  // 將數字映射到索引

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (seen.hasOwnProperty(complement)) {
            return [seen[complement], i];  // 找到答案
        }
        seen[nums[i]] = i;  // 儲存目前數字的索引
    }
}
```

---

### 🍩 Time & Space Complexity

#### ✅ English

* Time Complexity: O(n), we only iterate once.
* Space Complexity: O(n), for storing elements in the hash map.

#### ✅ 中文

* 時間複雜度：O(n)，我們只遍歷一次陣列。
* 空間複雜度：O(n)，用來儲存 hash map。

---

### 🐇 Follow-Up Questions

#### ✅ English

* What if the array is sorted? Can we use two pointers?
* What if there are multiple pairs that sum to the target?
* What if we're not allowed to use extra space?

#### ✅ 中文

* 如果陣列已經排序，可以用雙指標嗎？
* 如果有多組符合條件的數對該怎麼辦？
* 如果不能用額外空間該如何解？


