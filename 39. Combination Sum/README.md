

# 🧩 LeetCode 39 - Combination Sum (UMPIRE Method)

### 🧠 Understand

We are given an array of **unique positive integers** `candidates` and a target value `target`.
Our goal is to **return all unique combinations** of numbers from `candidates` that sum up to `target`.

* Each number in `candidates` can be used **unlimited times** in a combination.
* The solution set must **not contain duplicate combinations** (even if the order is different).

#### Example:

```python
Input: candidates = [2,3,6,7], target = 7  
Output: [[2,2,3], [7]]
```

---

### 🧩 Match

This is a **backtracking** problem, where we recursively try all possible combinations of the candidate numbers that could add up to the target.

We choose **Depth-First Search (DFS)** with backtracking.

---

### 📝 Plan

* Create a recursive helper function `backtrack(start, path, total)`:

  * `start`: Index to avoid repeating previous combinations.
  * `path`: Current combination list.
  * `total`: Current sum of elements in `path`.

* **Base Cases**:

  * If `total == target`, add `path` to result.
  * If `total > target`, stop searching further.

* In the recursive loop:

  * Traverse the `candidates` from index `start`.
  * Choose current number → add to path.
  * Recursively call `backtrack()` with same index (can reuse number).
  * Backtrack by removing the last number from path.

---

### 💻 Implement

#### 🐍 Python

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []

        def backtrack(start, path, total):
            if total == target:
                # Found a valid combination
                res.append(path[:])
                return
            if total > target:
                # Exceeded the sum, prune the path
                return

            for i in range(start, len(candidates)):
                # Choose candidates[i]
                path.append(candidates[i])
                # Recurse with the same index since reuse is allowed
                backtrack(i, path, total + candidates[i])
                # Backtrack: remove the last element and try the next candidate
                path.pop()

        backtrack(0, [], 0)
        return res
```

#### 🌐 JavaScript

```javascript
var combinationSum = function(candidates, target) {
    const res = [];

    function backtrack(start, path, total) {
        if (total === target) {
            // Found a valid combination
            res.push([...path]);
            return;
        }
        if (total > target) {
            // Prune the path
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            // Choose candidates[i]
            path.push(candidates[i]);
            // Recurse with the same index to allow reuse
            backtrack(i, path, total + candidates[i]);
            // Backtrack
            path.pop();
        }
    }

    backtrack(0, [], 0);
    return res;
};
```

---

### 🔍 Review

* ❓ Are we correctly pruning when total exceeds the target?
* ✅ Do we only include combinations that sum exactly to target?
* ✅ Do we avoid duplicates by controlling the `start` index?

---

### 📊 Evaluate

#### Time Complexity:

* Worst case: Exponential, **O(2^t)** where t = target

#### Space Complexity:

* Stack depth: **O(target)**
* Result size depends on how many valid combinations exist

#
#
#

# 🌸 中文版本 - UMPIRE 面試筆記

### 🧠 Understand（理解問題）

我們有一個不重複的正整數陣列 `candidates`，以及一個整數 `target`。
我們要找出所有可以組合加總為 target 的數字組合。

* 每個數字**可以重複使用**很多次。
* 結果中**不可以有重複的組合**（順序不同也視為重複）。

#### 範例：

```python
Input: candidates = [2,3,6,7], target = 7  
Output: [[2,2,3], [7]]
```

---

### 🧩 Match（匹配問題類型）

這是一道典型的 **回溯法（Backtracking）** 問題。

我們要用 **深度優先搜尋 DFS** 搭配「回溯」來試出所有可能的組合。

---

### 📝 Plan（規劃策略）

* 定義遞迴函數 `backtrack(start, path, total)`：

  * `start`: 控制從哪個 index 開始遍歷，避免重複組合。
  * `path`: 當前的組合。
  * `total`: 當前的總和。

* **終止條件**：

  * 若 total == target：將 path 加入答案。
  * 若 total > target：剪枝，直接 return。

* 迴圈中：

  * 從 `start` 開始往後試。
  * 每次選一個數字，加入 path。
  * 遞迴繼續探索。
  * 回溯：移除最後一個加入的數字。

---

### 💻 Implement（實作）

#### 🐍 Python 版本

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []

        def backtrack(start, path, total):
            if total == target:
                # 找到符合條件的組合
                res.append(path[:])
                return
            if total > target:
                # 加總超過目標，停止繼續
                return

            for i in range(start, len(candidates)):
                # 選擇 candidates[i]
                path.append(candidates[i])
                # 這裡傳入 i，不是 i+1，因為數字可以重複使用
                backtrack(i, path, total + candidates[i])
                # 回溯：移除最後加入的元素
                path.pop()

        backtrack(0, [], 0)
        return res
```

#### 🌐 JavaScript 版本

```javascript
var combinationSum = function(candidates, target) {
    const res = [];

    function backtrack(start, path, total) {
        if (total === target) {
            // 符合條件的組合
            res.push([...path]);
            return;
        }
        if (total > target) {
            // 超過目標，加以剪枝
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            // 選擇當前數字
            path.push(candidates[i]);
            // 遞迴時仍從 i 開始，允許重複使用
            backtrack(i, path, total + candidates[i]);
            // 回溯
            path.pop();
        }
    }

    backtrack(0, [], 0);
    return res;
};
```

---

### 🔍 Review（檢查）

* ✅ 是否有剪枝條件 total > target？
* ✅ 是否正確加入符合條件的 path？
* ✅ 是否透過 start 控制不重複？

---

### 📊 Evaluate（時間與空間複雜度）

#### ⏱️ 時間複雜度：

* 最差為 **指數級**：O(2^t)，t 為 target。

#### 🧠 空間複雜度：

* 遞迴深度最多為 target：O(target)
* 結果集合大小視答案數量而定。

#
#
#


### 🎯 Backtracking 直觀比喻：走迷宮找出口的你

想像你在走迷宮找出口，每走一步都記錄下路線（例如：先往左 → 再往上 → 再往右）

* 如果你走到死路，你會怎麼做？你會：

  1. **退一步**
  2. 換另一條路再走走看

`path.pop()` 就是在程式裡模擬這個「退一步」的動作：

```python
# 嘗試加入一個數字
path.append(candidates[i])
# 遞迴：試著往這個方向繼續走
backtrack(...)
# 回溯：這條路走完了（或是走錯了），我們退回來
path.pop()
```

---

### 🧠 如果沒有 `pop()` 會發生什麼？

你會一直把東西堆在 `path` 裡，沒有退回上一個狀態，導致：

1. 所有組合都混在一起
2. 你在一個組合裡加了太多東西，甚至無法準確加總
3. 沒辦法試「其他可能路徑」

---

### 🔍 用真實流程來跑一個例子

### Input: `candidates = [2, 3]`, target = 5

### 開始：

```python
res = []
path = []
```

#

### 第一層：嘗試 2

```python
path = [2]
backtrack(start=0, path=[2], total=2)
```

#

### 第二層：再嘗試 2

```python
path = [2, 2]
backtrack(start=0, path=[2, 2], total=4)
```

#

### 第三層：再嘗試 2

```python
path = [2, 2, 2]
total = 6 > 5 → return
```

現在要怎麼辦？我們**退一步**（不然永遠卡在這條死路）：

```python
path.pop()   # path = [2, 2]
```

換下一個選擇：改選 3！

#

### 回到第二層、嘗試 3：

```python
path = [2, 2, 3]
total = 7 > 5 → return → 再 pop()
```

一直這樣退、再試其他可能。

#
### 如果沒有 pop 的話：

你會在 `path = [2, 2, 2]` 的基礎上再加 `3`，變成 `[2, 2, 2, 3]`，整個混掉！

#

### 🧠 核心邏輯簡表

| 動作                 | 代表              |
| ------------------ | --------------- |
| `path.append(...)` | 做一個選擇（走進去）      |
| `backtrack(...)`   | 往這條路深入遞迴        |
| `path.pop()`       | 撤銷這個選擇（回退來走其他路） |

#

### ✅ 一句話總結：

> `pop()` 是為了「回復狀態」，讓我們可以試不同的組合，不 pop 就無法回退、也無法產生正確的所有組合。

#
#

具體地理解 backtracking 的流，使用這個例子：

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3], [7]]
```

來畫一個**遞迴樹狀圖（decision tree）**，搭配每一步對應的 `path` 與 `total`，並且說明什麼時候會 `append()`、什麼時候會 `pop()`、什麼時候 return 或加入結果。


### 🌲 遞迴樹狀流程圖（用文字模擬）

我們從 `start = 0` 開始，表示從 candidates\[0] = 2 開始試。

```
                              []
                            / |  \  \
                         [2][3][6][7] ← 第一層: 選擇一個數開始
                          |
                         [2,2]
                          |
                         [2,2,2]
                          |
                       [2,2,2,2] ❌ total=8 > 7 → return → pop
                          |
                       [2,2,2,3] ❌ total=9 > 7 → return → pop
                          |
                       [2,2,2,6] ❌ total=10 → return
                          |
                       [2,2,2,7] ❌ total=11 → return
                    ↑ pop to [2,2]
                    └── try 3 → [2,2,3] ✅ total=7 → 加入答案 → pop
                         → try 6 → [2,2,6] ❌ total=10 → return
                         → try 7 → [2,2,7] ❌ total=11 → return
                    ↑ pop to [2]
                    └── try 3 → [2,3] → total=5
                              └── try 3 again → [2,3,3] → total=8 ❌ → pop
                              └── try 6 → [2,3,6] → total=11 ❌
                              └── try 7 → total=12 ❌
                    ↑ pop to [2]
                    └── try 6 → [2,6] → total=8 ❌
                    └── try 7 → [2,7] → total=9 ❌
               ↑ pop to []
               └── try 3 → [3]
                       └── try 3 → [3,3] → total=6
                              └── try 3 → [3,3,3] → total=9 ❌
                              └── try 6 → total=12 ❌
                              └── try 7 → total=13 ❌
                       └── try 6 → total=9 ❌
                       └── try 7 → total=10 ❌
               └── try 6 → [6]
                       └── try 6 → total=12 ❌
                       └── try 7 → total=13 ❌
               └── try 7 → [7] ✅ total=7 → 加入答案
```


### 🧠 流程逐步分析

### Step 1️⃣：初始狀態

* `path=[]`, `total=0`, `start=0`
* 可選的數字：\[2, 3, 6, 7]

#

### Step 2️⃣：選擇 2，繼續探索

* `path=[2]`, `total=2`, `start=0`
* 可以繼續選 2（因為可重複）

#

### Step 3️⃣：再選 2

* `path=[2,2]`, `total=4`
* 繼續選 2 → `path=[2,2,2]`, `total=6`
* 再選 2 → `path=[2,2,2,2]`, `total=8` ❌ 超過 → `return` → **pop**（回退到 \[2,2,2]）

#

### Step 4️⃣：試試看其他數

* 在 \[2,2,2] 狀態下：

  * 選 3 → `total=9` ❌ → return
  * 選 6 → total=12 ❌ → return
  * 選 7 → total=13 ❌ → return

所以 \[2,2,2] 結束，**pop 到 \[2,2]**

#

### Step 5️⃣：在 \[2,2] 狀態下試 3：

* `path=[2,2,3]`, `total=7` ✅ → **加入答案！**
* 然後 `pop()` 回來，嘗試 6/7，也都超過 → return

#

### Step 6️⃣：回退到 \[2]，試 3：

* `path=[2,3]`, `total=5`
* 選 3 again → `total=8` ❌ → return
* 選 6 → total=11 ❌
* 選 7 → total=12 ❌
  → 回退到 \[]

#

### Step 7️⃣：試 3 為起點

* `path=[3]`, total=3
* 選 3 → \[3,3], total=6
* 選 3 → \[3,3,3] → total=9 ❌ → return
* 選 6, 7 → 都超過 → 回退

#

### Step 8️⃣：選 7 為起點

* `path=[7]`, total=7 ✅ 加入答案！

#

### ✅ 最後答案：

```python
[[2,2,3], [7]]
```


### 🔁 pop 的角色總結：

| 動作                         | 原因                          |
| -------------------------- | --------------------------- |
| `path.pop()`               | 把上一層嘗試的數字拿掉，讓我們能試下一個數字      |
| 例子：從 \[2,2,2] pop 到 \[2,2] | 我們才能試 \[2,2,3] 而不是卡死在 2 的路上 |
| 如果不 pop                    | 組合會包含錯誤數字，甚至重複出現不該出現的東西     |

#
#

### 🌳 Combination Sum 遞迴流程圖（強化詳解版）

> Input: `candidates = [2, 3, 6, 7]`
> Target = 7
> Output: `[[2,2,3], [7]]`



```
Level 0:                    []
                             ├────────────┬────────────┬────────────┐
Level 1:                 [2]            [3]           [6]           [7]
                         ↑append         ↑append       ↑append       ↑append
                         ↓start=0        ↓start=1      ↓start=2      ↓start=3
                         ↓total=2        ↓total=3      ↓total=6      ↓total=7 ✅➡加入答案
                         ↓

Level 2:             [2,2]              
                     ↑append  
                     ↓total=4 
                     ↓start=0

Level 3:         [2,2,2]           
                 ↑append
                 ↓total=6
                 ↓start=0

Level 4:     [2,2,2,2] ❌ total=8 > 7 → return
                 ↑append
                 ↓pop → 回到 [2,2,2]

             [2,2,2,3] ❌ total=9 > 7 → return  
                 ↑append
                 ↓pop → 回到 [2,2,2]

             [2,2,2,6] ❌ total=12 → return  
             [2,2,2,7] ❌ total=13 → return  

🔙 pop → 回到 Level 3: [2,2]

             [2,2,3] ✅ total=7 → 加入答案  
                 ↑append  
                 ↓pop → 回到 [2,2]  

             [2,2,6] ❌ total=10 → return  
             [2,2,7] ❌ total=11 → return  

🔙 pop → 回到 Level 2: [2]

             [2,3] → total=5  
                 ↑append  

Level 3:     [2,3,3] ❌ total=8 → return  
                 ↑append  
                 ↓pop → 回到 [2,3]

             [2,3,6] ❌ total=11 → return  
             [2,3,7] ❌ total=12 → return  

🔙 pop → 回到 [2]

             [2,6] ❌ total=8 → return  
             [2,7] ❌ total=9 → return  

🔙 pop → 回到 []

---

Level 1:                [3] → total=3
                         ↑append

Level 2:            [3,3] → total=6
                     ↑append

Level 3:        [3,3,3] ❌ total=9 → return  
                [3,3,6] ❌ total=12 → return  
                [3,3,7] ❌ total=13 → return  

🔙 pop → 回到 [3,3] → pop → 回到 [3]

             [3,6] ❌ total=9 → return  
             [3,7] ❌ total=10 → return  

🔙 pop → 回到 []

Level 1:                [6] → total=6
                         ↑append  

Level 2:            [6,6] ❌ total=12 → return  
                     [6,7] ❌ total=13 → return  

🔙 pop → 回到 []

Level 1:                [7] ✅ total=7 → 加入答案  
```



### 🧠 重點說明（搭配圖使用）

| 名稱        | 解釋                       |
| --------- | ------------------------ |
| `Level X` | 代表在第幾層遞迴中，例如 path 有幾層元素  |
| `↑append` | 這一層是如何加入一個新數字            |
| `↓pop`    | 這一層走完後怎麼回退上去（回溯）         |
| ✅ total=7 | 達到目標，加進 `res`            |
| ❌ total>x | 超過 target，不能繼續，立即 return |
| `start=i` | 控制下次從第幾個數字開始選，避免重複組合順序   |

---

### ✨ 最終結果：

```python
[[2,2,3], [7]]
```

這兩組組合都是由不重複順序產生（2,2,3 vs 3,2,2 只留一種），並且每組加總都等於 7。

---

### ✅ 總結關鍵理解：

| 概念                | 說明                                      |
| ----------------- | --------------------------------------- |
| `append()`        | 表示往下一層探索，試著選一個數字加入 path                 |
| `pop()`           | 表示這條路走完了、要退回上一層換個選擇                     |
| `total > target`  | 剪枝：超過就不必再繼續了                            |
| `total == target` | 成功找到一組解法，加到結果裡                          |
| `start`           | 控制重複使用 or 不重複的關鍵（這題可以重複使用，所以是 i，不是 i+1） |

