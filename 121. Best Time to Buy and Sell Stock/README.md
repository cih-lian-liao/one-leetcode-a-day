# 🛍️ LeetCode 121 – Best Time to Buy and Sell Stock

### 📖 1. Understand  
#### Problem Summary  
Given an array `prices` where `prices[i]` is the price of a stock on day `i`,  
return the **maximum profit** you can achieve by making **one buy and one sell** transaction.  
If you can't make any profit, return 0.

#### Input  
- `prices`: List of integers representing stock prices on each day

#### Output  
- An integer: the maximum profit possible

#### Constraints  
- Must buy before sell
- Only one transaction allowed
- 1 ≤ len(prices) ≤ 10⁵

---

### 🧩 2. Match  
#### Problem Pattern  
- This is a **greedy** array scan problem  
- Maintain the lowest price so far (`min_price`)
- At each step, calculate profit if we sell today, and update the max profit

---

### 🗺️ 3. Plan  
1. Initialize `min_price` as infinity (no minimum seen yet)
2. Initialize `max_profit` as 0
3. For each price:
   - If current price < min_price → update min_price
   - Else → calculate `price - min_price`, update max_profit if greater
4. Return `max_profit`

#### Edge Cases  
- Empty or one-element array → return 0

---

### 🐍 4. Implement (Python)

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices or len(prices) < 2:
            return 0  # Boundary check

        min_price = float('inf')  # Store the lowest price seen so far
        max_profit = 0  # Track the maximum profit

        for price in prices:
            if price < min_price:
                min_price = price  # Update the lowest price
            else:
                profit = price - min_price  # Potential profit if sold today
                max_profit = max(max_profit, profit)  # Update max profit

        return max_profit
````

---

### 🌐 4. Implement (JavaScript)

```javascript
var maxProfit = function(prices) {
    if (!prices || prices.length < 2) return 0; // Boundary check

    let minPrice = Infinity; // Lowest price so far
    let maxProfit = 0; // Max profit

    for (let price of prices) {
        if (price < minPrice) {
            minPrice = price; // Update lowest price
        } else {
            let profit = price - minPrice; // Profit if we sell today
            maxProfit = Math.max(maxProfit, profit); // Update max profit
        }
    }

    return maxProfit;
};
```

---

### 🔍 5. Review

#### Test Cases

* `[7,1,5,3,6,4]` → Buy at 1, Sell at 6 → Output: 5
* `[7,6,4,3,1]` → No profit → Output: 0
* `[1,2]` → Buy at 1, Sell at 2 → Output: 1

---

### 🎯 6. Evaluate

#### Time Complexity: O(n)

#### Space Complexity: O(1)

* One pass, constant space, greedy decision at each step
* This is optimal for the given constraint

# 
# 
# 
# 🛍️ LeetCode 121 – 買賣股票的最佳時機

### 📖 1. 理解題目 Understand  
#### 題目說明  
給你一個整數陣列 `prices`，其中 `prices[i]` 代表第 `i` 天的股票價格，  
你只能選擇一天買入、另一天賣出，買入日必須早於賣出日。  
請回傳你所能獲得的**最大利潤**；若無法獲利，請回傳 0。

#### 輸入  
- `prices`: 整數列表，表示每天的股價

#### 輸出  
- 一個整數，最大獲利金額

#### 條件限制  
- 只能進行一次交易  
- 必須先買後賣  
- `1 <= prices.length <= 10⁵`

---

### 🧩 2. 類型對應 Match  
#### 題型歸類  
- 經典的「一次買賣」問題  
- 使用**貪心算法**（Greedy）：每次都記錄歷史最低價，並計算當天賣出能否創造更大利潤

---

### 🗺️ 3. 解題計畫 Plan  
1. 初始化 `min_price = 無限大`，`max_profit = 0`
2. 遍歷每個價格：
   - 若當天價格比目前最低價小，更新最低價
   - 否則計算賣出利潤 → 若比目前最大利潤高就更新
3. 回傳 `max_profit`

#### 邊界條件  
- 空陣列或僅一個元素 → 回傳 0（無法交易）

---

### 🐍 4. Python 程式碼實作（含註解）

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices or len(prices) < 2:
            return 0  # 邊界檢查：沒有足夠的天數

        min_price = float('inf')  # 最低價格初始為無限大
        max_profit = 0  # 初始最大利潤為 0

        for price in prices:
            if price < min_price:
                min_price = price  # 更新最低價
            else:
                profit = price - min_price  # 今天賣出的利潤
                max_profit = max(max_profit, profit)  # 更新最大利潤

        return max_profit
````

---

### 🌐 4. JavaScript 程式碼實作（含註解）

```javascript
var maxProfit = function(prices) {
    if (!prices || prices.length < 2) return 0; // 邊界檢查

    let minPrice = Infinity; // 最低價格
    let maxProfit = 0; // 最大利潤

    for (let price of prices) {
        if (price < minPrice) {
            minPrice = price; // 更新最低價格
        } else {
            let profit = price - minPrice; // 今天賣出的利潤
            maxProfit = Math.max(maxProfit, profit); // 更新最大利潤
        }
    }

    return maxProfit;
};
```

---

### 🔍 5. 檢查測試 Review

#### 測資範例

* `[7,1,5,3,6,4]` → 買1賣6 → 回傳 5
* `[7,6,4,3,1]` → 無法獲利 → 回傳 0
* `[1,2]` → 買1賣2 → 回傳 1

---

### 🎯 6. 效能評估 Evaluate

#### 時間複雜度：O(n)

#### 空間複雜度：O(1)

* 單次遍歷即可完成
* 僅用常數額外空間
* 為此題最佳解

#
#
#

# 📈 LeetCode 121 - Best Time to Buy and Sell Stock: Coding Interview Speaking Guide

### 🧠 1. Clarify the Problem  
#### 💬 English  
"Let me first make sure I fully understand the problem."

We are given an array of integers, where each element represents the stock price on a given day.  
Our goal is to determine the **maximum profit** we can make by choosing **one day to buy** and **a later day to sell**.  
We can only complete **one transaction** — buy once, sell once.

"Just to confirm, we must buy before we sell, and return 0 if no profit is possible. Is that correct?"

#### 💬 中文  
「讓我先確認我是否完全理解這個問題。」

我們給定一個整數陣列，每個元素代表某天的股價。  
目標是找出從買入到賣出的最大利潤，而且只能進行一次交易（先買後賣）。  
如果無法賺錢，就應該回傳 0。

「我想確認的是，我們必須在買入之後才能賣出，若無法獲利則回傳 0，對嗎？」

---

### 🧊 2. Discuss Edge Cases  
#### 💬 English  
"Let me quickly consider a few edge cases."

- An empty array or array with one element → return 0 (no transaction possible)
- All prices decreasing → return 0 (no profitable transaction)
- All prices increasing → buy on day 0, sell on the last day

#### 💬 中文  
「我來快速考慮一些邊界情況。」

- 空陣列或只有一個價格 → 回傳 0（無法進行交易）  
- 價格持續下降 → 回傳 0（找不到可以賺錢的買賣）  
- 價格持續上升 → 第一天買，最後一天賣，獲得最大利潤

---

### 🐌 3. Consider Brute-Force and Optimal Approach  
#### 💬 English  
"A brute-force approach would be trying all pairs of days, calculating profit for each, and choosing the maximum.  
But that’s O(n²), which is inefficient."

"Instead, we can use a greedy strategy:  
As we scan the prices, keep track of the lowest price seen so far.  
At each day, compute the potential profit if we sold today.  
Update the maximum profit whenever it increases."

#### 💬 中文  
「暴力解法是檢查所有可能的買入和賣出日組合，計算每對的利潤，再選最大值。  
但這樣的時間複雜度是 O(n²)，效率太低。」

「所以我們改用貪心策略：  
一邊掃描股價，一邊記錄目前為止看到的最低價格，  
每天都試著計算如果今天賣出的話能賺多少，  
如果這個利潤比之前更大，就更新最大利潤。」

---

### ✍️ 4. Explain and Implement Optimal Code  
#### 💬 English  
"Let me write the code and explain step by step."

##### 🐍 Python

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices or len(prices) < 2:
            return 0  # Cannot make a transaction

        min_price = float('inf')  # Track the lowest price seen so far
        max_profit = 0  # Track the highest profit

        for price in prices:
            if price < min_price:
                min_price = price  # Found a new minimum price
            else:
                profit = price - min_price  # Profit if we sell today
                max_profit = max(max_profit, profit)  # Update if it's the best so far

        return max_profit
````

##### 🌐 JavaScript

```javascript
var maxProfit = function(prices) {
    if (!prices || prices.length < 2) return 0; // Edge case

    let minPrice = Infinity;
    let maxProfit = 0;

    for (let price of prices) {
        if (price < minPrice) {
            minPrice = price; // New lowest price found
        } else {
            const profit = price - minPrice;
            maxProfit = Math.max(maxProfit, profit); // Update profit
        }
    }

    return maxProfit;
};
```

#### 💬 中文

「我來寫下最佳解法的程式碼，並逐步解釋。」

* Python 中使用 `min_price` 來追蹤歷史最低價。
* 每天都嘗試用當天價格減去最低價來看能否賺錢。
* 如果能賺的錢比之前更多，就更新 `max_profit`。
* JavaScript 版本邏輯相同，只是語法差異。

---

### ⏱️ 5. Time and Space Complexity

#### 💬 English

"This solution runs in O(n) time since we only pass through the array once.
It uses O(1) space because we’re only storing two variables: min price and max profit."

#### 💬 中文

「這個解法的時間複雜度是 O(n)，因為我們只走訪一次陣列。
空間複雜度是 O(1)，因為只用了兩個變數來儲存最低價格與最大利潤。」

---

### 💡 6. Follow-Up Questions

#### 💬 English

Some follow-up questions the interviewer might ask:

* What if we can make **two** transactions? (→ LeetCode 123)
* What if we can make **unlimited** transactions? (→ LeetCode 122)
* What if we get prices **in a stream**, one per second?
* Can you return the exact **days to buy and sell**?

#### 💬 中文

面試官可能會延伸提問：

* 如果可以進行兩次交易呢？（對應 LeetCode 123）
* 如果可以不限次數地買賣呢？（對應 LeetCode 122）
* 如果股價是以即時串流的方式傳入呢？
* 你能不能回傳應該在哪幾天買入和賣出呢？

