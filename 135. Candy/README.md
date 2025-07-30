
# 🍭 LeetCode 135. Candy – UMPIRE Notes (English)

### ❓ U — Understand the Problem

We are given a list of integers `ratings`, where each integer represents the rating of a child. Our job is to distribute candies to these children such that:

1. Each child gets at least **1 candy**.
2. A child with a **higher rating than their neighbor** gets **more candies** than that neighbor.

We need to return the **minimum total number of candies** required to satisfy these rules.

---

### 🧩 M — Match to Known Patterns

This is a classic **Greedy Algorithm** problem. It involves:

* Making **local optimal decisions** (compare with neighbors)
* Doing **two passes** (left-to-right and right-to-left)

---

### 🛠️ P — Plan the Approach

#### Strategy:

1. Initialize a `candies` array with all 1s.
2. Traverse **left to right**:

   * If `ratings[i] > ratings[i-1]`, then `candies[i] = candies[i-1] + 1`
3. Traverse **right to left**:

   * If `ratings[i] > ratings[i+1]`, then `candies[i] = max(candies[i], candies[i+1] + 1)`
4. Return the **sum of candies**.

#### Time and Space Complexity:

* **Time:** O(n)
* **Space:** O(n)

---

### 🧑‍💻 I — Implement the Solution

#### 🐍 Python

```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n  # Step 1: Give each child 1 candy initially

    # Step 2: Left to Right pass
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            # If current child has higher rating than the left one,
            # give one more candy than the left child
            candies[i] = candies[i - 1] + 1

    # Step 3: Right to Left pass
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            # If current child has higher rating than the right one,
            # take the max to avoid overwriting larger left values
            candies[i] = max(candies[i], candies[i + 1] + 1)

    # Step 4: Return the total candies
    return sum(candies)
```

#### 🌐 JavaScript

```javascript
var candy = function(ratings) {
    const n = ratings.length;
    const candies = new Array(n).fill(1); // Step 1: Initialize each child with 1 candy

    // Step 2: Left to Right pass
    for (let i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1; // Give more candy than the left child
        }
    }

    // Step 3: Right to Left pass
    for (let i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            // Take the max to avoid reducing already assigned candy
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }

    // Step 4: Return total candies
    return candies.reduce((total, c) => total + c, 0);
};
```

---

### 🔍 R — Review with Test Cases

#### Example 1

```
Input:  [1, 0, 2]
Output: 5
Reason: [2, 1, 2]
```

#### Example 2

```
Input:  [1, 2, 2]
Output: 4
Reason: [1, 2, 1]
```

---

### 🧠 E — Evaluate and Optimize

* This two-pass greedy approach is optimal for this problem.
* Alternative approaches using O(1) space are **possible** but harder to implement and less readable.
* Practical applications include **ranking-based reward systems**, **performance bonuses**, and **merit-based resource allocation**.

#
#
#

# 🍬 LeetCode 135. 分糖果 – UMPIRE 筆記（中文）

### ❓ U — 理解題目（Understand）

給你一個整數陣列 `ratings`，每個數字代表一位小朋友的評分。我們要依照以下兩個規則來發糖果：

1. 每位小朋友至少要拿到 1 顆糖果。
2. 如果某個小朋友的評分比鄰居高，他就要拿得比鄰居多。

請回傳「最少」需要發出的糖果總數。

---

### 🧩 M — 類型對應（Match）

這是一題典型的 **貪心演算法（Greedy Algorithm）** 題目。核心觀念是：

* 根據鄰居的狀況，做出「當下最好的選擇」
* 需要從 **左到右一次**、**右到左再一次**

---

### 🛠️ P — 解題計畫（Plan）

#### 解法步驟：

1. 建立一個 `candies` 陣列，初始都填入 1（每人至少一顆）。
2. 從左到右掃描：

   * 如果 `ratings[i] > ratings[i-1]`，那 `candies[i] = candies[i-1] + 1`
3. 再從右到左掃描：

   * 如果 `ratings[i] > ratings[i+1]`，那 `candies[i] = max(candies[i], candies[i+1] + 1)`
4. 把 `candies` 陣列加總回傳。

#### 時間與空間複雜度：

* **時間：** O(n)
* **空間：** O(n)

---

### 🧑‍💻 I — 程式實作（Implement）

#### 🐍 Python 實作

```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n  # 每個孩子先分 1 顆糖

    # 左到右：右邊比左邊高 → 糖果數也要多
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            candies[i] = candies[i - 1] + 1

    # 右到左：左邊比右邊高 → 要檢查是否需要更新糖果數
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            candies[i] = max(candies[i], candies[i + 1] + 1)

    return sum(candies)
```

#### 🌐 JavaScript 實作

```javascript
var candy = function(ratings) {
    const n = ratings.length;
    const candies = new Array(n).fill(1); // 每個孩子先發 1 顆糖果

    // 左到右：右邊評分高 → 多發一顆
    for (let i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }

    // 右到左：左邊評分高 → 多發一顆，但不能比原本更少
    for (let i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }

    return candies.reduce((total, c) => total + c, 0);
};
```

---

### 🔍 R — 測試與驗證（Review）

#### 範例一：

```text
輸入：ratings = [1, 0, 2]
步驟：→ [2, 1, 2]
總數：5
```

#### 範例二：

```text
輸入：ratings = [1, 2, 2]
步驟：→ [1, 2, 1]
總數：4
```

---

### 🧠 E — 評估與優化（Evaluate）

* 此為最有效且最直覺的方式，讀性高。
* 也能思考實務應用，例如：

  * 獎金分配機制
  * 根據表現自動分級給資源
  * 競賽排名獎項制度
 
#
#
#

# 🎤 Full Spoken-Style Interview Answer


### 1️⃣ Clarify the Problem

Sure! So for this problem, we're given an array called `ratings`, where each element represents the rating of a child.
We are supposed to distribute candies to each child based on the following two rules:

* Each child must receive **at least one candy**.
* Any child who has a **higher rating than their neighbor** should get **more candies than that neighbor**.

Our goal is to calculate the **minimum number of candies** we need to distribute while satisfying these two conditions.

Is that correct?
Also, should I assume the ratings array is non-empty? And can I assume the ratings are integers?

---

### 2️⃣ Discuss Edge Cases

Some edge cases that come to mind:

* If the ratings array has only one child, like `[5]`, then we should return `1`.
* If all children have the same rating, like `[3, 3, 3]`, each child just gets 1 candy → total = 3.
* If ratings are strictly increasing like `[1, 2, 3]`, we expect to give out `[1, 2, 3]` candies → total = 6.
* If ratings are strictly decreasing like `[3, 2, 1]`, we expect `[3, 2, 1]` → also total = 6.
* For wave-like patterns such as `[1, 2, 2, 1]`, we need to check carefully both left and right constraints.

---

### 3️⃣ Consider Brute-Force and Optimal Approach

**Brute-force idea:**
One naive way might be to repeatedly loop through the ratings and adjust the candy count until no violations remain. But this can be inefficient, especially for long inputs, and may lead to a time complexity worse than O(n).

**Optimal idea (Greedy):**
We can solve this in **two passes** using a greedy strategy.

* First, scan from **left to right**, and if a child has a higher rating than their left neighbor, give them more candies.
* Then scan from **right to left**, and if a child has a higher rating than their right neighbor, make sure they also have more candies.

This way, we guarantee that **both sides of the condition are satisfied**, and we can keep the total candies minimal.

---

### 4️⃣ Explain and Implement Optimal Code

Let me walk through the Python implementation first:

```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n  # Step 1: Give each child 1 candy initially

    # Step 2: Left to Right pass
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            candies[i] = candies[i - 1] + 1

    # Step 3: Right to Left pass
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            candies[i] = max(candies[i], candies[i + 1] + 1)

    return sum(candies)
```

Now let me explain this step-by-step:

1. First, we initialize a `candies` array with 1 candy for each child.
2. In the **first loop**, we go from left to right. If the current child has a higher rating than the one before them, we give them one more candy than the previous child.
3. In the **second loop**, we go from right to left. We do the same logic in reverse — if the current child has a higher rating than the one after them, we make sure they have more candies. But we also take the `max()` to make sure we don't reduce a candy count that was already set correctly in the first loop.
4. Finally, we just sum up the `candies` array to get the minimum total required.

This ensures both rules are satisfied for every child.

Would you like me to implement the same logic in JavaScript as well?

```javascript
var candy = function(ratings) {
    const n = ratings.length;
    const candies = new Array(n).fill(1);

    // Left to right pass
    for (let i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }

    // Right to left pass
    for (let i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }

    return candies.reduce((sum, count) => sum + count, 0);
};
```

---

### 5️⃣ Discuss Time and Space Complexity

* The **time complexity** is **O(n)** because we do two separate linear passes over the array.
* The **space complexity** is also **O(n)** because we use an extra array to store the candy distribution.

If needed, this could be optimized further for space, but the logic becomes harder to manage.

---

### 6️⃣ Mention Follow-up Questions

Some possible follow-up questions an interviewer might ask:

* Can you reduce the space complexity to O(1) while still satisfying the rules?
* What would happen if there were more complex neighbor rules — say, you have to compare a child to both neighbors simultaneously?
* What if children can sit in a circle instead of a line?
* Could you design a version that tracks **how many different valid candy distributions** are possible?

#
#
#

### ❓WHY｜為什麼要練習這道題

**EN:**
This problem teaches how to use the greedy algorithm effectively when you need to satisfy local constraints between neighbors. It helps you think about multi-pass scans and how to merge two conditions (left and right) safely without conflict. It’s a fundamental pattern used in reward allocation, trend evaluation, and state propagation.

**ZH｜中文：**
這道題訓練我們如何使用「貪心演算法」來解決基於「鄰近條件」的分配問題。它讓我們學會在多次掃描中處理「雙邊條件」並避免覆蓋錯誤，是一種在獎勵分配、趨勢分析、狀態傳遞中經常出現的基本解法模式。

---

### 🎯 Real-World Applications｜實際應用場景

#### 💼 1. Performance-Based Bonus Distribution｜依據績效的獎金分配

**EN:**
In companies, employees are often evaluated based on performance scores. When distributing bonuses, those with better performance than their peers should get higher rewards. This mirrors the candy problem with ratings and neighbor comparison.

**ZH｜中文：**
在公司中，員工常根據績效評分分配獎金。當一位員工的績效比鄰近同事高時，理應獲得更多獎金，這與本題中依評分和鄰居比較分糖果的邏輯非常相似。

---

#### 🏫 2. Classroom Participation Rewards｜課堂參與獎勵制度

**EN:**
Teachers may want to reward students based on participation scores while maintaining fairness across seating arrangements. Students with higher engagement than neighbors (e.g., by row) should receive more tokens.

**ZH｜中文：**
老師可能根據學生的課堂參與度發放獎勵，並希望在相鄰座位之間保持公平。如果某位學生參與程度高於左右鄰座，就應獲得更多獎勵，這就是 candy 題的延伸應用。

---

#### 📈 3. Data Trend Scoring｜資料趨勢評分系統

**EN:**
In analytics systems, you might need to assign scores to time-series or spatial data where trends matter. If a data point shows a better trend than its neighbor, it should be rewarded with a higher weight.

**ZH｜中文：**
在資料分析系統中，若需根據時間序列或空間分布趨勢給予權重，當某點的趨勢比鄰近點更顯著時，它就應該有更高的分數。這和 ratings 陣列的比較邏輯非常接近。

---

#### 🧭 4. Scheduling with Priority Constraints｜排程中的優先權處理

**EN:**
When assigning tasks with soft priority constraints—like scheduling jobs on a conveyor where adjacent jobs cannot differ too much in priority—you may need to ensure adjacent elements follow an increasing or decreasing pattern like in the candy problem.

**ZH｜中文：**
在進行排程時，若任務有「優先順序但不能差太多」的軟性限制（例如輸送帶上的作業），就必須讓相鄰任務的資源或權重調整，這與 candy 題中的「根據相鄰元素調整分配」的邏輯一致。

---

#### 🚦 5. UI/UX Priority Animation Timing｜前端動畫或提示排序

**EN:**
When designing animation delays or tooltip display priority in UI/UX, you may want to adjust delays based on visual importance or content rank. Items with higher priority than adjacent ones should be triggered faster or more noticeably.

**ZH｜中文：**
在前端介面設計中，若要根據使用者重要性或內容優先度調整動畫延遲或提示顯示順序，較高優先權的項目應比鄰近項目更快被觸發，這也與 candy 題「優先→多資源」的概念呼應。

