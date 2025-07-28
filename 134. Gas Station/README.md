
# ⛽ LeetCode 134 — Gas Station｜加油站問題

#### 🧠 UMPIRE (English Version)

### 🟣 Understand

We are given two integer arrays, `gas` and `cost`, of the same length. Each `gas[i]` represents the amount of fuel we get at station `i`, and `cost[i]` represents the fuel needed to go from station `i` to station `i+1`.

Goal: Return the starting gas station index that allows you to travel the entire circuit **once**, or return -1 if it's impossible.

Key insights:
- If total gas < total cost → impossible → return -1
- If total gas ≥ total cost → there’s a unique valid solution

---

### 🔎 Match

- This is a **Greedy algorithm** problem.
- It's about choosing a valid starting point.
- Related to cumulative sum, subarray reasoning.

---

### 🧩 Plan

1. Compute the total gas and total cost.
2. If total gas < total cost, return -1.
3. Iterate through each gas station:
   - Track remaining tank after each stop.
   - If the tank goes negative, that means this route from previous start is invalid. Reset starting station to `i+1`.

---

### 💻 Implement

#### 🐍 Python

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        total_gas = 0     # Total net gas across all stations
        tank = 0          # Current gas in the tank
        start = 0         # Candidate starting station index

        for i in range(len(gas)):
            diff = gas[i] - cost[i]
            total_gas += diff      # Accumulate net gas globally
            tank += diff           # Accumulate local tank gas

            if tank < 0:
                # Can't reach next station, reset start
                start = i + 1
                tank = 0

        return start if total_gas >= 0 else -1
````

#### 🌐 JavaScript

```javascript
var canCompleteCircuit = function(gas, cost) {
    let totalGas = 0;  // Total net gas across all stations
    let tank = 0;      // Gas currently in tank
    let start = 0;     // Candidate start station index

    for (let i = 0; i < gas.length; i++) {
        const diff = gas[i] - cost[i];
        totalGas += diff;  // Add to total gas
        tank += diff;      // Update current tank

        if (tank < 0) {
            // Cannot reach the next station, reset start
            start = i + 1;
            tank = 0;
        }
    }

    return totalGas >= 0 ? start : -1;
};
```

---

### 🧠 Why compute `total_gas` separately?

Even though we track the tank while simulating the route, we still need to compute the total net gas separately. Here's why:

* `tank` keeps track of the **local gas balance** while simulating a specific start station.
* But `tank` might give a **false sense** of validity — we may find a candidate start that seems to work temporarily, but the **overall circuit** is still impossible.

### ✅ `total_gas` ensures feasibility

By calculating `total_gas += gas[i] - cost[i]` for all stations:

* We **verify whether the problem has any valid solution**.
* If `total_gas < 0`, it means the car doesn’t have enough fuel to complete the entire loop — no matter where you start.

```python
return start if total_gas >= 0 else -1
```

### 🔁 Summary:

| Variable    | Purpose                                    |
| ----------- | ------------------------------------------ |
| `tank`      | Tracks local tank status for current route |
| `total_gas` | Confirms if the entire route is possible   |

> You can think of `tank` as simulating, and `total_gas` as verifying.

---

### 📈 Review

* Run example test cases to verify.
* Think through edge cases: total gas exactly equal to cost, only one station, etc.

---

### ⏱️ Evaluate

* **Time complexity**: O(n) — one pass through the array
* **Space complexity**: O(1) — constant variables

---

#### 🧠 UMPIRE（中文版本）

### 🟣 U — 理解問題

你有兩個等長陣列 `gas` 和 `cost`：

* `gas[i]` 表示在第 `i` 個加油站能加到的油量。
* `cost[i]` 表示從第 `i` 站開到下一站需要的油量。

目標：找出一個起始站點，讓你可以繞行整圈回到原地。如果無法完成，回傳 -1。

🔑 關鍵點：

* 如果總油量 < 總花費 → 一定不可能 → 回傳 -1
* 如果總油量 ≥ 總花費 → 保證有唯一可行解

---

### 🔎 M — 類型匹配

* 這是一題 **貪心算法**（Greedy）。
* 類似處理「最大子陣列和」或「連續累加」的問題。
* 要動態判斷從哪一站出發最合理。

---

### 🧩 P — 解題計畫

1. 計算 `total_gas` 和 `total_cost`。
2. 如果總油量不足，直接回傳 -1。
3. 遍歷每個站：

   * 用 `tank` 模擬目前油箱狀態。
   * 如果途中油量變負，代表這段無法完成，起點改為下一站。

---

### 💻 I — 實作代碼

#### 🐍 Python（含註解）

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        total_gas = 0     # 全部油量總和（評估整體是否可行）
        tank = 0          # 當前油箱剩餘油量
        start = 0         # 起始加油站索引

        for i in range(len(gas)):
            diff = gas[i] - cost[i]  # 本站盈餘油量
            total_gas += diff        # 累加整體油量差
            tank += diff             # 更新油箱狀態

            if tank < 0:
                # 若油量不足，代表起點不成立
                start = i + 1
                tank = 0             # 油箱清空，重新開始計算

        return start if total_gas >= 0 else -1
```

#### 🌐 JavaScript（含註解）

```javascript
var canCompleteCircuit = function(gas, cost) {
    let totalGas = 0;  // 評估是否總體可完成
    let tank = 0;      // 模擬當前油箱
    let start = 0;     // 嘗試的起點

    for (let i = 0; i < gas.length; i++) {
        const diff = gas[i] - cost[i]; // 計算盈餘
        totalGas += diff;
        tank += diff;

        if (tank < 0) {
            // 無法前進，重設起點
            start = i + 1;
            tank = 0;
        }
    }

    return totalGas >= 0 ? start : -1;
};
```
---

### 🧠 為什麼還要計算 `total_gas`？

雖然我們在模擬過程中用 `tank` 追蹤油量，但還是需要額外使用 `total_gas` 的原因是：

* `tank` 是用來**模擬局部路徑**中油箱是否足夠。
* 但 `tank` 有可能會給出**錯誤的信號** —— 你可能找到一個起點好像可以走，但其實從全局來看根本無法完成一圈。

### ✅ `total_gas` 是用來驗證可行性

透過每一站累加 `gas[i] - cost[i]`：

* 我們可以判斷這一圈的總油量是否夠。
* 如果 `total_gas < 0`，就代表整體油是不夠的，**從哪一站開始都沒用**，直接回傳 -1。

```python
return start if total_gas >= 0 else -1
```

### 🔁 小結：

| 變數名稱        | 用途說明            |
| ----------- | --------------- |
| `tank`      | 模擬從目前起點的油箱狀態    |
| `total_gas` | 驗證整圈是否可完成的總體可行性 |

> 可以把 `tank` 當成是「局部模擬」，`total_gas` 則是「整體驗證」。
---

### 📈 R — 複習檢查

* 測試範例：

  * gas = \[1,2,3,4,5], cost = \[3,4,5,1,2] → 回傳 3
  * gas = \[2,3,4], cost = \[3,4,3] → 回傳 -1
* 留意邊界：只有一個站、一站 gas 恰好等於 cost 等情況。

---

### ⏱️ E — 效能分析

* **時間複雜度**：O(n)
* **空間複雜度**：O(1)

#
#
#

# 🎤 Full Spoken-Style Interview Answer



### 1️⃣ Clarify the problem

"Let me make sure I understand the problem correctly.

We are given two integer arrays: `gas` and `cost`, both of length `n`. Each element `gas[i]` represents the amount of fuel we can fill at station `i`, and `cost[i]` is the fuel needed to travel from station `i` to station `i + 1` — with wrap-around at the end, since the stations form a circular route.

We need to return the index of the starting station that allows us to complete a full circuit. If it's not possible, we should return -1. The problem guarantees that if a solution exists, it will be unique."

---

### 2️⃣ Discuss edge cases

"Some edge cases I’m considering:

* The total amount of gas is less than the total cost — in that case, the trip is impossible, so we return -1.
* The input arrays might have only one station — we need to make sure the code still works for that.
* The solution must account for circular behavior, meaning we wrap around from the last station to the first.
* Negative or zero differences in gas-cost — we should handle that carefully."

---

### 3️⃣ Consider brute-force and optimal approach

"My first thought is to simulate the trip starting from every station, and check whether we can complete the circuit. That would be a brute-force solution with O(n²) time complexity — not very efficient.

But since the problem guarantees that if there is a solution, it is unique, we can do better.

Here’s the key insight:

* If the total gas is greater than or equal to the total cost, **we know a solution exists**.
* While iterating through each station, if at any point our tank goes negative, we know that we can’t start from any of the stations between the previous start and the current index. So we reset the starting index to the next station."

---

### 4️⃣ Explain and implement optimal code

"I'll use a greedy one-pass solution.

We’ll keep two variables:

* `total_gas` to check whether the trip is possible at all.
* `tank` to track the current fuel while simulating the trip.

Here’s how the Python code looks:"

#### 🐍 Python

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        total_gas = 0  # total net fuel across all stations
        tank = 0       # current fuel tank while simulating
        start = 0      # candidate starting station

        for i in range(len(gas)):
            diff = gas[i] - cost[i]
            total_gas += diff
            tank += diff

            # If tank goes negative, cannot start from current start
            if tank < 0:
                start = i + 1  # try next station
                tank = 0       # reset tank

        return start if total_gas >= 0 else -1
```

"And here’s the JavaScript version:"

#### 🌐 JavaScript

```javascript
var canCompleteCircuit = function(gas, cost) {
    let totalGas = 0;
    let tank = 0;
    let start = 0;

    for (let i = 0; i < gas.length; i++) {
        const diff = gas[i] - cost[i];
        totalGas += diff;
        tank += diff;

        if (tank < 0) {
            start = i + 1;
            tank = 0;
        }
    }

    return totalGas >= 0 ? start : -1;
};
```

---

### 5️⃣ Discuss time/space complexity

"The time complexity is O(n), since we loop through the array only once.
The space complexity is O(1), because we only use a few variables to store state."

---

### 6️⃣ Mention follow-up questions

"A few possible follow-up questions or extensions might be:

* What if we needed to return **all possible starting indices** instead of just one?
* How would you solve it if the problem **didn't guarantee a unique solution**?
* What changes if the car’s tank has a maximum capacity and can’t hold all the gas from a station?
* Could we make the solution parallelizable?"
#
#
#

# 🎤 Full Spoken Interview Script｜完整英文面試口語講稿 — LeetCode 134: Gas Station

### ❓ Clarify the Problem｜釐清問題

#### 💬 English  
Let me make sure I understand the problem correctly.

We're given two arrays, `gas` and `cost`, both of the same length.  
Each `gas[i]` means the amount of fuel at station `i`, and `cost[i]` means the fuel required to travel to the next station `(i + 1)` in a circular path.

We need to find a **starting station index** such that we can **complete the entire circuit once**, returning to the start.  
If it’s not possible, we return `-1`.

The input guarantees that if there is a solution, it is **unique**.

#### 📖 中文  
我先確認一下我是否正確理解這題。

我們有兩個長度相同的陣列 `gas` 和 `cost`。  
其中 `gas[i]` 表示第 `i` 個加油站有多少油可加，`cost[i]` 表示從第 `i` 站開到第 `i+1` 站所需的油量（而加油站是環狀的）。

我們要找出一個「起始加油站」的 index，使我們能夠順利**繞完整圈並回到起點**。  
如果無法完成，就回傳 `-1`。

題目保證如果存在解，則答案是**唯一**的。

---

### 🔍 Edge Cases｜邊界情況

#### 💬 English  
Some edge cases to consider:

- If total gas is less than total cost → definitely impossible.
- Only one station? Make sure it works.
- Must respect the circular route — we wrap from last station back to index `0`.
- Negative or zero differences in gas-cost? Be cautious.

#### 📖 中文  
我們要考慮以下邊界情況：

- 如果總油量小於總花費 → 絕對無法完成 → 回傳 -1。
- 如果只有一個加油站，也要能正確處理。
- 這是一個環狀路線，最後一站應該能接回第 0 站。
- gas 與 cost 的差值若為 0 或負數也要小心處理。

---

### 💭 Brute Force & Optimal Plan｜暴力與最佳解法

#### 💬 English  
A brute-force idea is to try every station as a starting point and simulate the full circuit. That would be O(n²), which is inefficient.

Instead, we notice something:
If the **total gas is greater than or equal to total cost**, a solution is guaranteed (from problem constraints).

We can use a **greedy strategy**:  
- Use `tank` to simulate current fuel.  
- If `tank < 0` at station `i`, that means we can't start from `start ~ i`.  
→ So, we reset the starting station to `i + 1` and reset `tank = 0`.

#### 📖 中文  
暴力解法是：從每個加油站作為起點開始模擬走完整圈，檢查能否回到起點。但時間複雜度是 O(n²)，效率很差。

我們可以發現：
如果「總加油量 ≥ 總花費」，根據題目保證，一定存在解。

這時可以使用 **貪心策略**：
- 用 `tank` 來模擬當前油量。
- 如果某一站後 `tank < 0`，說明無法從 `start ~ i` 中的任何一站完成旅程。
→ 就把起點設為 `i + 1`，`tank` 歸零，重新計算。

---

### 🧑‍💻 Code Implementation｜程式實作

#### 🐍 Python

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        total_gas = 0  # 全局油量總差，驗證整體是否可行
        tank = 0       # 當前油箱剩餘
        start = 0      # 候選起始站點

        for i in range(len(gas)):
            diff = gas[i] - cost[i]
            total_gas += diff
            tank += diff

            if tank < 0:
                # 無法到下一站，重設起點
                start = i + 1
                tank = 0

        return start if total_gas >= 0 else -1
````

#### 💻 JavaScript

```javascript
var canCompleteCircuit = function(gas, cost) {
    let totalGas = 0;
    let tank = 0;
    let start = 0;

    for (let i = 0; i < gas.length; i++) {
        const diff = gas[i] - cost[i];
        totalGas += diff;
        tank += diff;

        if (tank < 0) {
            start = i + 1;
            tank = 0;
        }
    }

    return totalGas >= 0 ? start : -1;
};
```

---

### ⏱️ Complexity Analysis｜時間與空間複雜度分析

#### 💬 English

* Time complexity: **O(n)** — single pass
* Space complexity: **O(1)** — constant space

#### 📖 中文

* 時間複雜度：**O(n)**，只需一次迴圈
* 空間複雜度：**O(1)**，只用常數空間儲存狀態變數

---

### 🧩 Follow-Up Questions｜延伸討論

#### 💬 English

* What if we had to return all possible valid starting points?
* What if the tank had a maximum capacity?
* What if the problem didn’t guarantee uniqueness?
* Could the check be parallelized?

#### 📖 中文

* 如果題目要我們回傳所有可行的起始站呢？
* 如果車子的油箱有最大容量限制怎麼辦？
* 如果題目沒有保證唯一解呢？
* 我們能否平行處理檢查每一個站點？

---

### 🧠 Final Takeaway｜總結提醒

#### 💬 English

This is a classic greedy problem where the key lies in recognizing that local failure leads us to a better global start point.
The global feasibility is verified by `total_gas`, and the local feasibility is tracked with `tank`.

#### 📖 中文

這是一題典型的貪心題。關鍵在於：一旦局部油箱不足，就可以直接跳過這一段，尋找更好的起點。
整體可行性由 `total_gas` 驗證，局部模擬由 `tank` 控制。

#
#
#

### ❓WHY｜為什麼要練習這道題

#### 💬 English  
This problem is an excellent exercise for understanding greedy algorithms, prefix sum logic, and how to design logic that simulates constraints over a circular structure.

It strengthens your:
- Greedy thinking and local/global feasibility reasoning
- Ability to simulate real-world resource systems efficiently
- Interview communication skills when justifying greedy choices

It also introduces the important idea of “failing early” — eliminating invalid candidates quickly, which is very relevant in system optimization problems.

#### 📖 中文  
這題是學習貪心演算法、前綴和邏輯，以及如何模擬環狀結構限制的絕佳練習。

它能強化你的：
- 貪心思維與區分局部/全域可行性的邏輯推理
- 模擬資源流動與消耗的能力
- 面試中說明為何選擇 greedy 策略的溝通技巧

同時也教會我們一個關鍵技巧：「提早失敗」→ 能快速排除不可行的候選方案，這在系統效能設計中非常實用。

---

### 🎯 Real-World Applications｜實際應用場景

#### 💬 English  
Here are some real-world use cases where this pattern shows up:

1. **Battery/Power Management in IoT or Robotics**  
   - You need to determine whether a robot can complete a circular route of tasks given charging points and energy consumption per step.

2. **Delivery Route Optimization**  
   - In logistics, you may need to decide whether a vehicle can complete all deliveries with the available fuel across stations.

3. **Resource Allocation in Game Design**  
   - In game development, players may need to complete a circular challenge using limited stamina or tools — this logic applies for path feasibility.

4. **Token Ring Network Simulation (OS/Networking)**  
   - In token ring networks or circular scheduling, validating whether a message or token can complete a round-trip successfully is conceptually similar.

5. **CPU Scheduling and Circular Buffers**  
   - When handling tasks in round-robin fashion, you may simulate resource exhaustion and reset points, similar to the `tank < 0` logic.

#### 📖 中文  
以下是幾個實際場景中會使用到這題邏輯的例子：

1. **物聯網或機器人的電池/電力管理**  
   - 需要判斷機器人在有限充電點與耗電的情況下，是否能完成一整圈任務。

2. **物流配送路線優化**  
   - 在物流系統中，需要決定一輛車是否能靠每站補充的油完成所有配送。

3. **遊戲設計中的資源配置與體力挑戰**  
   - 玩家需要在有限資源（體力、工具）下完成一個「環狀」任務，這題的邏輯剛好適用於可行性驗證。

4. **作業系統或網路中的 Token Ring 網路模擬**  
   - 在環狀網路或輪詢排程中，驗證 token 是否能繞一圈傳遞成功，本質與這題邏輯類似。

5. **CPU 輪詢排程與環狀快取（circular buffer）處理**  
   - 當系統以 round-robin 處理任務時，也需要類似「耗盡資源 → 重設起點」的機制，這與 `tank < 0` 的貪心邏輯相似。

#
#
#


### 🧾 FAQ｜常見疑問與深入補充

### ❓ Why don't we use `i % n` to simulate the circular route?  
### ❓為什麼我們的解法中不需要用 `%` 來處理環狀結構？

#### 💬 English  
Even though the stations form a circle, we don’t need to simulate wrapping around using `i % n`. That’s because the greedy approach lets us scan from left to right once and find the only possible start index. Once we complete this linear pass, we’ve essentially "covered" the circular structure.

We only need to reset the starting index (`start = i + 1`) when we run out of gas (`tank < 0`).  
So we never actually need to loop around in code — we let the one-pass logic take care of it.

#### 📖 中文  
雖然加油站是環狀的，我們的程式中**不需要用 `i % n`** 來模擬繞圈。  
這是因為我們使用貪心策略，一次從左到右的遍歷就能找出唯一可能的起點。

只要在油箱變成負值時（`tank < 0`），把起點重設為下一站（`i + 1`）即可。  
這種做法會自然地處理「哪裡不能當起點」，所以程式不需要真的繞圈。

---

### ❓ Why do we still need `total_gas` even after finding a candidate `start`?  
### ❓就算找到了起點，為什麼還需要 `total_gas` 檢查？

#### 💬 English  
Great question! Even if our greedy logic finds a candidate start index, it doesn't guarantee the car can finish the full circuit unless the total gas is **at least equal** to the total cost.

`total_gas` is used to verify global feasibility.  
Without it, our `start` might be a false positive if the entire trip is impossible due to insufficient fuel.

#### 📖 中文  
很好的問題！就算我們的 `start` 是透過貪心邏輯找出來的，但這不代表就一定能完成一圈，**除非總油量 ≥ 總花費**。

`total_gas` 是用來檢查**全域可行性**的。  
如果沒有這個檢查，`start` 有可能是個「看起來可以但實際不行」的錯誤起點，因為整體油量根本不足。

---

### ❓ Is the answer always unique?  
### ❓這題的解一定是唯一的嗎？

#### 💬 English  
Yes — the problem statement says:  
> "The input is generated such that the answer is unique."

That means:
- If a solution exists, it will be exactly one.
- We don’t need to worry about returning multiple indices or checking ambiguity.

This makes greedy logic even more powerful and safe to apply.

#### 📖 中文  
是的，題目明確說明：  
>「輸入資料會設計成只有唯一解。」

這代表：
- 如果存在解，它只會有一個。
- 我們不需要考慮多個 index 的情況，也不用擔心不確定性。

這樣的條件讓貪心解法更加穩定、可靠。

