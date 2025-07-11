
# 🐇 LeetCode 55 – Jump Game (UMPIRE Method – English)

### 📘 Problem Statement

#### Given an array of non-negative integers `nums`, where each element represents your maximum jump length at that position, determine if you can reach the last index.

---

### 🧠 Understand

#### Inputs:

* `nums`: List\[int], each element `nums[i]` is the max jump you can take from index `i`.

#### Outputs:

* Return `True` if it's possible to reach the last index from the first index, otherwise `False`.

#### Constraints:

* `1 <= nums.length <= 10^4`
* `0 <= nums[i] <= 10^5`

---

### 🔍 Match

#### Problem Type:

* **Greedy Algorithm**

#### Why Greedy?

* At each step, we only care about the farthest index we can reach.
* We don’t need to try all paths — just need to track if we can reach the end.

---

### 📝 Plan

1. Initialize a variable `farthest` to 0 — represents the farthest index we can reach so far.
2. Loop through the array:

   * If current index `i` is beyond `farthest`, return False.
   * Update `farthest = max(farthest, i + nums[i])`.
3. If the loop completes, return True.

---

### 💻 Implement

#### 🐍 Python Version

```python
from typing import List

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        farthest = 0  # The farthest position we can currently reach

        for i in range(len(nums)):
            if i > farthest:
                # If our current index is beyond our max reach, we’re stuck
                return False
            # Update the farthest reach from the current position
            farthest = max(farthest, i + nums[i])
        
        # If we finished the loop, it means we can reach the end
        return True
```

#### 🌐 JavaScript Version

```javascript
var canJump = function(nums) {
    let farthest = 0; // Tracks the farthest reachable index

    for (let i = 0; i < nums.length; i++) {
        if (i > farthest) {
            // If current index is unreachable, we can't continue
            return false;
        }
        // Update farthest reach based on current position
        farthest = Math.max(farthest, i + nums[i]);
    }

    // If we completed the loop, it means the last index is reachable
    return true;
};
```

---

### 🧪 Review – Test Cases

#### Example 1:

```text
Input: [2,3,1,1,4]
Output: True
```

#### Example 2:

```text
Input: [3,2,1,0,4]
Output: False
```

---

### 📈 Evaluate

#### Time Complexity:

* O(n): One pass through the array.

#### Space Complexity:

* O(1): No extra space used.

#### Key Insight:

* If we ever land on an index we can't reach, we return `False`.

#
#
#

# 🐢 LeetCode 55 – 跳躍遊戲（UMPIRE 方法 – 中文）

### 📘 題目說明

#### 給定一個非負整數陣列 `nums`，每個元素代表從該位置最多可以跳的步數。請判斷能否從索引 0 跳到最後一格。

---

### 🧠 理解（Understand）

#### 輸入：

* `nums`: List\[int]，其中每個 `nums[i]` 表示從索引 i 最多可以跳幾格。

#### 輸出：

* 如果可以跳到最後一格，回傳 `True`；否則回傳 `False`。

#### 條件限制：

* `1 <= nums.length <= 10^4`
* `0 <= nums[i] <= 10^5`

---

### 🔍 套用模式（Match）

#### 題型：

* **貪婪演算法**

#### 為什麼可以用貪婪？

* 因為只要追蹤「目前能到的最遠距離」就能判斷是否能走完陣列。
* 不需要窮舉所有路徑。

---

### 📝 解題計畫（Plan）

1. 建立一個變數 `farthest = 0`，代表目前可以跳到的最遠位置。
2. 從頭走訪陣列：

   * 如果 `i > farthest`，代表我們卡住了，直接回傳 False。
   * 否則就更新 `farthest = max(farthest, i + nums[i])`
3. 如果成功走完陣列，代表可以跳到最後一格，回傳 True。

---

### 💻 程式實作（Implement）

#### 🐍 Python 實作

```python
from typing import List

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        farthest = 0  # 紀錄目前最遠可到達的索引

        for i in range(len(nums)):
            if i > farthest:
                # 若目前索引超出最遠可達位置，代表無法前進
                return False
            # 更新最遠可達位置
            farthest = max(farthest, i + nums[i])
        
        # 順利走完代表可達終點
        return True
```

#### 🌐 JavaScript 實作

```javascript
var canJump = function(nums) {
    let farthest = 0; // 記錄當前可達的最遠索引

    for (let i = 0; i < nums.length; i++) {
        if (i > farthest) {
            // 當前位置無法到達
            return false;
        }
        // 更新最遠可達索引
        farthest = Math.max(farthest, i + nums[i]);
    }

    // 成功遍歷所有元素，代表可達終點
    return true;
};
```

---

### 🧪 測試案例（Review）

#### 範例 1：

```text
輸入: [2,3,1,1,4]
輸出: True
```

#### 範例 2：

```text
輸入: [3,2,1,0,4]
輸出: False
```

---

### 📈 評估（Evaluate）

#### 時間複雜度：

* O(n)：只需一次遍歷整個陣列。

#### 空間複雜度：

* O(1)：只使用一個整數變數。

#### 核心觀念：

* 只要你能追蹤「目前能到達的最遠距離」，就能判斷是否能走到終點。
#
#
#

# 🏃 LeetCode 55: Jump Game — Interview Explanation (English + 中文對照)

### 🎯 1. Clarify the Problem  
#### ENGLISH  
"Let me first clarify the problem to make sure I fully understand it."

"We're given an array of non-negative integers. Each element represents the **maximum number of steps** we can jump forward from that position. The goal is to determine if we can reach the **last index**, starting from the first index."

"For example, if `nums = [2,3,1,1,4]`, then the output should be `true` because we can jump from index 0 → 1 → 4, which is the last index."

#### 中文  
「我想先確認題目的意思，確保我完全理解它。」

「這題給我們一個非負整數的陣列，陣列的每一個值代表從那個位置**最多能跳的步數**。我們的目標是判斷，從第 0 個位置出發，能否跳到陣列的**最後一個索引**。」

「舉例來說，如果 `nums = [2,3,1,1,4]`，答案應該是 `true`，因為我們可以從第 0 跳到 1，再跳到 4，剛好到最後。」

---

### 🧪 2. Discuss Edge Cases  
#### ENGLISH  
"Now I’ll consider some edge cases to make sure we handle them correctly."

- "If the array has only **one element**, like `[0]`, we're already at the end, so return `true`."
- "If the **first element is 0** and the array has more than one element, like `[0,1,2]`, we can't move, so return `false`."
- "If all values are large, like `[10,9,8,7]`, return `true`."

#### 中文  
「接下來我會列出一些 edge case，確保程式正確處理。」

- 「如果陣列只有一個元素，比如 `[0]`，我們一開始就在終點，應該回傳 `true`。」
- 「如果第一個值是 0 且有超過一個元素，例如 `[0,1,2]`，我們根本不能動，應回傳 `false`。」
- 「如果全部的值都很大，例如 `[10,9,8,7]`，那肯定能到終點，回傳 `true`。」

---

### 🧠 3. Consider Brute-force and Optimal Approach  
#### ENGLISH  
"A brute-force way would be to try all paths recursively, but that will be too slow."

"A better solution is to use a **greedy approach**: at each index, we track the **farthest index** we can reach. If we ever reach an index beyond that, we return false."

"If we can reach the last index or beyond, we return true."

#### 中文  
「暴力法可以試著用遞迴列出所有跳法，但這樣時間複雜度會太高。」

「更好的方法是使用**貪心法**：我們在每個位置記錄目前可以跳到的**最遠距離**。如果我們目前的位置超過了這個範圍，就代表我們卡住了，回傳 false。」

「只要我們能抵達或超過最後一個位置，就回傳 true。」

---

### 💻 4. Explain and Implement Optimal Code  
#### ENGLISH  
"Now let me walk you through the optimal solution using greedy strategy."

##### Python 🐍
```python
def canJump(nums):
    # farthest keeps track of the furthest index we can reach
    farthest = 0

    # Loop through each index
    for i in range(len(nums)):
        # If the current index is beyond what we can reach, we’re stuck
        if i > farthest:
            return False

        # Update the farthest index we can reach
        farthest = max(farthest, i + nums[i])

    # If the loop completes, we were able to reach the end
    return True
````

##### JavaScript 🌐

```javascript
function canJump(nums) {
    let farthest = 0;

    for (let i = 0; i < nums.length; i++) {
        if (i > farthest) return false;
        farthest = Math.max(farthest, i + nums[i]);
    }

    return true;
}
```

#### 中文

「現在讓我用貪心法一步步說明 optimal 解法。」

##### Python 🐍（附註解）

```python
def canJump(nums):
    # 記錄目前能跳到的最遠位置
    farthest = 0

    # 走訪整個陣列
    for i in range(len(nums)):
        # 如果走到比目前最遠距離還遠的地方，表示我們跳不到這裡
        if i > farthest:
            return False
        
        # 更新能跳到的最遠位置
        farthest = max(farthest, i + nums[i])

    # 能走完全程就回傳 True
    return True
```

##### JavaScript 🌐（附註解）

```javascript
function canJump(nums) {
    let farthest = 0;

    for (let i = 0; i < nums.length; i++) {
        // 如果當前位置已經超出能到的範圍
        if (i > farthest) return false;

        // 更新最遠能到的位置
        farthest = Math.max(farthest, i + nums[i]);
    }

    return true;
}
```

---

### ⏱️ 5. Discuss Time and Space Complexity

#### ENGLISH

"The time complexity is **O(n)** because we loop through the array once."

"The space complexity is **O(1)** since we only use a few variables."

#### 中文

「時間複雜度是 **O(n)**，因為我們只走訪一次陣列。」

「空間複雜度是 **O(1)**，只用到一個變數來記錄最遠位置。」

---

### 📚 6. Mention Follow-up Questions

#### ENGLISH

"Possible follow-up questions could include:"

* "What if we want to return the **actual path**?"
* "What if jumps have **costs**, and we want the **minimal total cost**?"
* "What if we want the **minimum number of jumps** to reach the end? (Leetcode 45)"

#### 中文

「可能的 follow-up 問題包括：」

* 「如果我們想要回傳實際的跳躍路徑該怎麼做？」
* 「如果每一步跳躍有**花費**，如何找出總花費最少的方式？」
* 「如果我們想要用**最少的跳數**到達最後一格？（這是 LeetCode 45 題）」


