# 🐇 LeetCode 45 - Jump Game II (UMPIRE - English Version)


### 🧠 Understand

#### What is the problem asking?

We are given an array `nums`, where each element represents the **maximum jump length** from that position. We need to **return the minimum number of jumps required** to reach the last index.
It is guaranteed that you can reach the last index.

#### Inputs

* `nums`: List\[int] (length ≥ 1)

#### Outputs

* Integer: minimum number of jumps

---

### 🔍 Match

#### What kind of problem is this?

This is a **greedy problem** that can be visualized as:

* Trying to reach the farthest point possible at every jump.
* Deciding when to "make a jump" by checking when our current jump range ends.

Pattern: **Greedy traversal with range tracking**.

---

### 🧭 Plan

#### Step-by-step strategy:

* Initialize:

  * `jumps = 0` to count jumps
  * `end = 0` to record the range end of the current jump
  * `farthest = 0` to record the farthest reachable index so far
* Loop through `nums` from index `0` to `len(nums) - 2`:

  * Continuously update `farthest = max(farthest, i + nums[i])`
  * If we reach `end`, we increment jump count and set `end = farthest`

---

### 🧑‍💻 Implement

#### ✅ Python Version

```python
from typing import List

class Solution:
    def jump(self, nums: List[int]) -> int:
        jumps = 0       # Counts the number of jumps made
        end = 0         # End of the current jump range
        farthest = 0    # The farthest index we can currently reach

        # We don't include the last element since once we reach it, we're done
        for i in range(len(nums) - 1):
            # Update the farthest we can reach from current position
            farthest = max(farthest, i + nums[i])

            # If we've reached the end of the current jump range
            if i == end:
                jumps += 1            # We need to jump again
                end = farthest        # Update the new range's end

        return jumps
```

#### ✅ JavaScript Version

```javascript
var jump = function(nums) {
    let jumps = 0;       // Number of jumps taken
    let end = 0;         // End of the current jump range
    let farthest = 0;    // The farthest reachable index so far

    for (let i = 0; i < nums.length - 1; i++) {
        // Update the farthest position we can reach
        farthest = Math.max(farthest, i + nums[i]);

        // If we reached the end of current jump range
        if (i === end) {
            jumps++;           // Make a jump
            end = farthest;    // Update end to farthest reachable
        }
    }

    return jumps;
};
```

---

### 🧪 Review (Test Cases)

#### Test Case 1

Input: `[2, 3, 1, 1, 4]` → Output: `2`
Explanation: Jump from index 0 to 1, then to 4.

#### Test Case 2

Input: `[2, 1]` → Output: `1`

#### Test Case 3

Input: `[1, 1, 1, 1]` → Output: `3`

---

### 📈 Evaluate

#### Time Complexity: `O(n)`

We traverse the array once.

#### Space Complexity: `O(1)`

No extra data structures used.

#
#
#

# 🐇 LeetCode 45 - 跳躍遊戲 II（UMPIRE 中文版）

---

### 🧠 Understand

#### 題目是什麼？

給定一個整數陣列 `nums`，每個元素代表從當前位置最多可以跳幾步。請返回「從第 0 格跳到最後一格所需的最少跳躍次數」。

保證一定可以跳到終點。

#### 輸入

* `nums`: List\[int]，長度至少為 1

#### 輸出

* 整數：最少跳躍次數

---

### 🔍 Match

#### 這是哪種類型的問題？

這是一道典型的 **貪心策略問題**（Greedy）
目標是：

* 每次在「當前可跳範圍內」找出最遠能到的點
* 在到達當前範圍終點時執行一次跳躍

---

### 🧭 Plan

#### 解題計畫：

* 初始：

  * `jumps = 0` 紀錄跳躍次數
  * `end = 0` 表示當前這一跳的結束範圍
  * `farthest = 0` 當前最遠能跳的位置
* 遍歷 index `0` 到 `len(nums) - 2`：

  * 不斷更新 `farthest = max(farthest, i + nums[i])`
  * 如果抵達 `end`，代表該跳結束 → 執行跳躍，更新 `end = farthest`

---

### 👨‍💻 Implement

#### ✅ Python 實作（含註解）

```python
from typing import List

class Solution:
    def jump(self, nums: List[int]) -> int:
        jumps = 0       # 紀錄跳躍次數
        end = 0         # 當前跳躍的範圍結束點
        farthest = 0    # 可到達的最遠位置

        # 遍歷到倒數第二格就好，因為最後一格不需要跳
        for i in range(len(nums) - 1):
            # 更新目前最遠可以跳的位置
            farthest = max(farthest, i + nums[i])

            # 如果到了當前跳躍的終點，跳一次，更新範圍
            if i == end:
                jumps += 1
                end = farthest

        return jumps
```

#### ✅ JavaScript 實作（含註解）

```javascript
var jump = function(nums) {
    let jumps = 0;       // 紀錄跳躍次數
    let end = 0;         // 當前跳躍的範圍終點
    let farthest = 0;    // 可到達的最遠位置

    for (let i = 0; i < nums.length - 1; i++) {
        // 更新能跳到的最遠位置
        farthest = Math.max(farthest, i + nums[i]);

        // 若到達目前範圍終點，跳一次並更新
        if (i === end) {
            jumps++;
            end = farthest;
        }
    }

    return jumps;
};
```

---

### 🧪 Review（測試案例）

#### 測資 1

輸入：`[2, 3, 1, 1, 4]` → 輸出：`2`
說明：從 index 0 → 跳到 1 → 再跳到 4

#### 測資 2

輸入：`[2, 1]` → 輸出：`1`

#### 測資 3

輸入：`[1, 1, 1, 1]` → 輸出：`3`

---

### 📈 Evaluate（時間與空間）

#### 時間複雜度：`O(n)`

只遍歷一次陣列

#### 空間複雜度：`O(1)`

無額外空間使用

#
#
#

# 🐰 45. Jump Game II - Spoken Interview Notes (English & 中文對照)

### 🐇 Clarify the Problem  
#### In English:
Let me first make sure I understand the problem correctly.  
We are given an array of integers called `nums`, where each element represents the maximum number of steps we can jump forward from that position.  
Our goal is to determine the **minimum number of jumps** required to reach the **last index**, starting from the first index.  
We are guaranteed that we can always reach the last index.

#### 中文說明：
我先確認一下我是否正確理解了這題。  
我們得到一個整數陣列 `nums`，每個元素代表我們從這個位置最多可以往前跳的步數。  
我們的目標是從第 0 個位置出發，跳到最後一個 index，並求出**最少需要跳幾次**。  
題目保證一定可以到達最後一格。

---

### 🐰 Discuss Edge Cases  
#### In English:
Let’s quickly consider a few edge cases:  
- If the array has only one element, like `[0]`, then we’re already at the last index, so we need 0 jumps.  
- If all the elements are 1, for example `[1, 1, 1, 1]`, we need `n-1` jumps.  
- If the input is an empty array, that’s technically not valid since the problem guarantees a reachable last index.

#### 中文說明：
我們來快速討論幾個邊界案例：  
- 如果陣列只有一個元素，例如 `[0]`，那我們已經在最後一格，不需要跳，答案是 0。  
- 如果全部都是 1，例如 `[1, 1, 1, 1]`，那每次只能跳一步，總共需要 `n-1` 次。  
- 如果輸入是空陣列，這不符合題目條件，因為題目保證可以到達最後一格。

---

### 🐇 Consider Brute-Force and Optimal Approach  
#### In English:
A brute-force approach would be to use BFS and explore all reachable positions at each index. But this will be too slow, with time complexity potentially up to O(n²).  
Instead, I’ll use a **greedy approach**. The idea is to always jump to the farthest point we can reach within the current jump, and only jump when we reach the end of our current range.

#### 中文說明：
暴力解法可以用 BFS（廣度優先搜索），嘗試每一步所有可以跳的位置。但這樣太慢了，時間複雜度可能高達 O(n²)。  
我們改用**貪心法**：每一步都選擇在目前跳躍範圍內可以跳到的最遠的位置，並在抵達當前跳躍範圍的終點時才做一次跳躍。

---

### 🐰 Explain and Implement Optimal Code  
#### In English:
We maintain three variables:
- `jumps`: counts the number of jumps taken.
- `currentEnd`: marks the farthest point we can go in the current jump.
- `farthest`: tracks the farthest point reachable as we scan the array.

##### ✅ Python Code:
```python
from typing import List

class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return 0  # Already at the last index

        jumps = 0          # Total number of jumps made
        currentEnd = 0     # The end of the current jump range
        farthest = 0       # The farthest we can reach so far

        for i in range(len(nums) - 1):
            # Update the farthest point we can reach
            farthest = max(farthest, i + nums[i])

            # If we reach the end of the current jump range,
            # we need to make another jump
            if i == currentEnd:
                jumps += 1
                currentEnd = farthest

        return jumps
````

##### ✅ JavaScript Code:

```javascript
var jump = function(nums) {
    let jumps = 0;          // Number of jumps made
    let currentEnd = 0;     // End of the current jump range
    let farthest = 0;       // Farthest we can reach so far

    for (let i = 0; i < nums.length - 1; i++) {
        // Update farthest reachable position
        farthest = Math.max(farthest, i + nums[i]);

        // If we reach the end of current range, do a jump
        if (i === currentEnd) {
            jumps++;
            currentEnd = farthest;
        }
    }

    return jumps;
};
```

#### 中文說明：

我們需要三個變數：

* `jumps`：目前跳躍的次數
* `currentEnd`：當前跳躍範圍的最遠位置
* `farthest`：目前可達的最遠位置

我們遍歷陣列，更新 `farthest`，並在抵達 `currentEnd` 時增加跳躍次數，並更新下一跳的終點為 `farthest`。

---

### 🐇 Discuss Time and Space Complexity

#### In English:

This greedy approach runs in **O(n)** time, since we loop through the array only once.
It uses **O(1)** space because we only store a few variables regardless of the input size.

#### 中文說明：

貪心法的時間複雜度是 **O(n)**，因為我們只遍歷一次陣列。
空間複雜度是 **O(1)**，因為只用了常數個變數。

---

### 🐰 Mention Follow-Up Questions

#### In English:

* What if we needed to return the **actual path** (the indices of the jumps)?
* What if not all positions are guaranteed to be reachable? How would we detect that?
* Can you solve this using **dynamic programming**?

#### 中文說明：

* 如果題目要求回傳**實際跳躍的路徑**（index 組成的 list），該怎麼做？
* 如果**不保證能到終點**，該如何檢查是否可達？
* 可以用 **動態規劃** 解這一題嗎？


