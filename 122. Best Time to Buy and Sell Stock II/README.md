
# 💹 LeetCode 122: Best Time to Buy and Sell Stock II

### 🧠 Understand  
#### Problem  
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.  
You can complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).  
However, you may not engage in multiple transactions simultaneously (you must sell the stock before you buy again).  
Return the maximum profit you can achieve.

#### Input
- `prices`: List of integers representing daily stock prices.

#### Output
- An integer representing the maximum profit.

#### Constraints
- `1 <= prices.length <= 3 * 10^4`
- `0 <= prices[i] <= 10^4`

---

### 🧩 Match  
#### Problem Type  
- Greedy  
- Array traversal  

#### Pattern  
- Whenever `prices[i] > prices[i - 1]`, we simulate buying at `i-1` and selling at `i`.

---

### 📝 Plan  
1. Initialize a variable `total_profit = 0`.
2. Loop from day 1 to the end:
   - If `prices[i] > prices[i - 1]`, add the difference to `total_profit`.
3. Return `total_profit`.

---

### 🧑‍💻 Implement

#### 🐍 Python (with detailed comments):

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Initialize total profit to 0
        total_profit = 0

        # Iterate over the array starting from the second day
        for i in range(1, len(prices)):
            # If today's price is higher than yesterday's, we can make a profit
            if prices[i] > prices[i - 1]:
                # Add the difference (today's gain) to the total profit
                total_profit += prices[i] - prices[i - 1]

        return total_profit
````

---

#### 🌐 JavaScript (with detailed comments):

```javascript
var maxProfit = function(prices) {
    let totalProfit = 0;

    // Loop from the second element to the end
    for (let i = 1; i < prices.length; i++) {
        // If the current price is greater than the previous day's price
        if (prices[i] > prices[i - 1]) {
            // Add the difference to the total profit
            totalProfit += prices[i] - prices[i - 1];
        }
    }

    return totalProfit;
};
```

---

### 🧪 Review

#### Test Case 1

Input: `[7,1,5,3,6,4]`
Output: `7`
Explanation: Buy at 1, sell at 5 (profit = 4). Buy at 3, sell at 6 (profit = 3). Total = 7.

#### Test Case 2

Input: `[1,2,3,4,5]`
Output: `4`
Explanation: Multiple consecutive gains. Buy at 1 and sell at 5, or simulate each daily trade.

---

### 📊 Evaluate

#### Time Complexity

* O(n) — We traverse the list once.

#### Space Complexity

* O(1) — No additional memory used except the `total_profit` variable.

#
#
#
# 📈 LeetCode 122：買賣股票的最佳時機 II

### 🧠 理解問題  
#### 題目  
給你一個整數陣列 `prices`，其中 `prices[i]` 是第 `i` 天的股票價格。  
你可以進行無限次買賣（但**不能同時持有多張股票**，也就是說賣了才能買）。  
請回傳你所能得到的**最大利潤**。

#### 輸入
- `prices`: 表示每天股價的整數陣列

#### 輸出
- 一個整數，代表最大可獲利金額

#### 限制條件
- `1 <= prices.length <= 3 * 10^4`
- `0 <= prices[i] <= 10^4`

---

### 🧩 類型匹配  
#### 題型  
- 貪心算法  
- 陣列遍歷問題

#### 模式  
- 只要今天比昨天貴，就假裝昨天買今天賣，把利潤加起來

---

### 📝 解題計畫  
1. 初始化變數 `total_profit = 0`  
2. 從第 1 天開始到最後一天遍歷：  
   - 如果今天價格大於昨天，就把價差加入 `total_profit`  
3. 回傳 `total_profit`

---

### 👨‍💻 實作代碼

#### 🐍 Python（含詳細註解）

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 初始化總利潤為 0
        total_profit = 0

        # 從第二天開始檢查是否可以賺錢
        for i in range(1, len(prices)):
            # 如果今天價格比昨天高，模擬昨天買、今天賣
            if prices[i] > prices[i - 1]:
                # 把利潤加到總利潤中
                total_profit += prices[i] - prices[i - 1]

        return total_profit
````

---

#### 🌐 JavaScript（含詳細註解）

```javascript
var maxProfit = function(prices) {
    let totalProfit = 0;

    // 從第 1 天開始遍歷
    for (let i = 1; i < prices.length; i++) {
        // 如果今天的價格高於昨天，代表可以賺錢
        if (prices[i] > prices[i - 1]) {
            // 將這段利潤加入總利潤
            totalProfit += prices[i] - prices[i - 1];
        }
    }

    return totalProfit;
};
```

---

### 🧪 測試範例

#### 範例 1

輸入：`[7,1,5,3,6,4]`
輸出：`7`
說明：1 買 5 賣賺 4，3 買 6 賣賺 3，共 7。

#### 範例 2

輸入：`[1,2,3,4,5]`
輸出：`4`
說明：每天都漲，就每天賺一點，共賺 4。

---

### 📊 效能評估

#### 時間複雜度

* O(n)：只遍歷一次整個陣列

#### 空間複雜度

* O(1)：只使用一個變數儲存總利潤

#
#
#

# 🧠 LeetCode Interview Speaking Template – 英文面試口語筆記（英文＋中文對照）

### 💡 1. Clarify the Problem | 釐清問題

#### ✅ English:
Let me make sure I understand the problem correctly.  
We're given an array of prices where `prices[i]` is the stock price on day `i`.  
We can buy and sell any number of times, but **we must sell the stock before we can buy again** — we can’t hold more than one stock at a time.  
Our goal is to return the **maximum profit** possible.

🗣️ Interview line you can say:  
> "I just want to confirm I understand the problem correctly. Is my understanding accurate?"

#### 🈶 中文：
讓我確認我是否正確理解了這個問題。  
我們有一個陣列 `prices`，其中 `prices[i]` 表示第 `i` 天的股票價格。  
我們可以買賣不限次數，但一次只能持有一張股票，也就是說，**必須先賣掉才可以再次買入**。  
目標是回傳能夠取得的**最大利潤**。

🗣️ 你可以在面試中這樣說：  
> 「我想先確認我是否正確理解了題目，請問這樣的理解正確嗎？」

---

### ✨ 2. Discuss Edge Cases | 討論邊界情況

#### ✅ English:
Before we dive into the solution, I'd like to consider some edge cases:
- If the input array is empty or has only one element, no transaction is possible.
- If prices are always decreasing, like `[5, 4, 3]`, the best profit is 0.
- If prices are always increasing, like `[1, 2, 3]`, we would buy early and sell each day to maximize profit.

🗣️ Interview line you can say:  
> "Would you like me to handle these edge cases explicitly in the code?"

#### 🈶 中文：
在開始解題前，我想先討論幾個邊界情況：
- 如果陣列為空或只有一個數字，就無法進行交易。
- 如果價格總是遞減，例如 `[5, 4, 3]`，則最佳做法是完全不交易，利潤為 0。
- 如果價格總是遞增，例如 `[1, 2, 3]`，則每天賣出都會有利潤。

🗣️ 面試中你可以這樣問：  
> 「您希望我在程式碼中明確處理這些邊界情況嗎？」

---

### 🌱 3. Brute-force and Optimal Approach | 暴力解與最佳解法

#### ✅ English:
A brute-force solution would try all possible buy and sell pairs, but that would be inefficient — O(n²) time complexity.  
Instead, we can use a **greedy** approach.  
Whenever today’s price is higher than yesterday’s, we can treat it as a buy-sell opportunity and add that profit to our result.  
We sum all such gains to get the total maximum profit.

🗣️ Interview line you can say:  
> "So instead of checking every pair, I’d like to use a greedy approach where I only sum up the profits from every ascending pair."

#### 🈶 中文：
暴力解會檢查所有可能的買賣日對組合，這樣會有 O(n²) 的時間複雜度，非常沒效率。  
更好的方法是採用**貪心演算法**：  
只要今天的價格比昨天高，我們就假設昨天買入、今天賣出，賺取這筆利潤。  
將所有這樣的正利潤加起來就是最大總利潤。

🗣️ 面試中你可以說：  
> 「我打算用貪心策略，看到今天價格比昨天高就當作一次交易，這樣可以加總所有可得利潤。」

---

### 🐣 4. Code with Explanation | 實作與講解

#### ✅ English (Python):

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        total_profit = 0
        # Loop from day 1 to the end
        for i in range(1, len(prices)):
            # If today's price is higher than yesterday's, sell for a profit
            if prices[i] > prices[i - 1]:
                total_profit += prices[i] - prices[i - 1]
        return total_profit
````

🗣️ What to say while coding:

> "Here I’m looping from the second day (index 1) to the end. Every time I see a price that's greater than the previous day's price, I consider it a profitable transaction and add that difference to total profit."

#### 🈶 中文（Python）：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        total_profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i - 1]:
                total_profit += prices[i] - prices[i - 1]
        return total_profit
```

🗣️ 你可以邊寫邊解釋：

> 「我從第 2 天（index 1）開始走訪，並與前一天比較，只要今天比昨天貴，就視為可以賺錢的機會，把這段利潤加進 total。」

---

#### ✅ English (JavaScript):

```javascript
var maxProfit = function(prices) {
    let totalProfit = 0;
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            totalProfit += prices[i] - prices[i - 1];
        }
    }
    return totalProfit;
};
```

🗣️ What to say:

> "This JavaScript version follows the same idea. We loop through the prices and add the profit from each increasing pair."

#### 🈶 中文（JavaScript）：

```javascript
var maxProfit = function(prices) {
    let totalProfit = 0;
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            totalProfit += prices[i] - prices[i - 1];
        }
    }
    return totalProfit;
};
```

🗣️ 你可以這樣說：

> 「JavaScript 寫法和 Python 一樣，我們每次遇到今天比昨天高的情況，就加進總利潤。」

---

### 🌟 5. Time and Space Complexity | 時間與空間複雜度

#### ✅ English:

* **Time Complexity**: O(n), we loop through the array once.
* **Space Complexity**: O(1), we only use a few variables, no extra memory is used.

🗣️ You can say:

> "This solution is efficient in both time and space, running in linear time with constant space."

#### 🈶 中文：

* **時間複雜度**：O(n)，我們只走訪一次。
* **空間複雜度**：O(1)，只用了幾個變數，沒有額外的記憶體。

🗣️ 你可以說：

> 「這個解法時間與空間都非常有效率，是線性時間、常數空間。」

---

### 🎁 6. Follow-up Questions | 延伸問題與考點

#### ✅ English:

* Add a **transaction fee** (LeetCode 714).
* Add a **cooldown** period after each sell (LeetCode 309).
* Limit the number of transactions to 2 (LeetCode 123).
* Track actual **buy/sell days** instead of just profit.

🗣️ You can say:

> "If time allows, I'd be happy to explore variations such as transaction fees or cooldown periods."

#### 🈶 中文：

* 加入手續費（LeetCode 714）。
* 加入賣出後冷卻期（LeetCode 309）。
* 限制最多兩次交易（LeetCode 123）。
* 不只回傳利潤，還要標示每次買入賣出的日期。

🗣️ 你可以說：

> 「如果有時間，我也可以試著解釋有手續費或冷卻期的延伸版本。」
