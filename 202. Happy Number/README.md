# 😆 LeetCode 202 – Happy Number (UMPIRE, English)

### 🧠 Understand

#### Problem

Given a positive integer `n`, repeatedly replace `n` by the **sum of the squares of its digits**. If this process eventually reaches `1`, `n` is **happy**; if it falls into a cycle that never hits `1`, `n` is **unhappy**. Return `true/false`.

#### Key intuition

* The digit–square transform shrinks numbers quickly. Any value will drop into a small finite set (≤ 243) and then either reach `1` or loop—so this is a **cycle detection** problem.

---

### 🔍 Match

* Model the transform as a function `next(n) = sum_of_squares_of_digits(n)`.
* Detect cycles via:

  1. **HashSet** of seen states (simple & clear).
  2. **Floyd’s tortoise–hare** (O(1) extra space).

---

### 🗺️ Plan

#### 🧺 Plan A — HashSet

* Loop while `n != 1` and `n` not in `seen`.
* Insert `n` into `seen`, update `n = next(n)`.
* Stop: return `true` if `n == 1`, else `false` when repeated.

#### 🐇 Plan B — Floyd’s Cycle Detection

* `slow = n`, `fast = next(n)`.
* While `fast != 1` and `slow != fast`:

  * `slow = next(slow)`
  * `fast = next(next(fast))`
* If `fast == 1` → happy; else cycle → unhappy.

---

### 🧩 Implement

#### 🐍 Python — HashSet (step-by-step comments)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            """Return sum of squares of digits of x (no divmod; clearer steps)."""
            total = 0
            while x > 0:
                d = x % 10          # take last digit
                total += d * d      # add its square
                x = x // 10         # remove last digit
            return total

        seen = set()                 # states we've already visited
        # Iterate until we either reach 1 (happy) or repeat a state (cycle)
        while n != 1 and n not in seen:
            seen.add(n)              # mark current state
            n = next_value(n)        # move to next state
        return n == 1                # true if ended at 1, else false
```

#### 🦄 JavaScript — HashSet (step-by-step comments)

```javascript
var isHappy = function (n) {
  const nextValue = (x) => {
    // Return sum of squares of digits of x
    let total = 0;
    while (x > 0) {
      const d = x % 10;          // take last digit
      total += d * d;            // add its square
      x = Math.floor(x / 10);    // remove last digit
    }
    return total;
  };

  const seen = new Set();        // visited states
  while (n !== 1 && !seen.has(n)) {
    seen.add(n);                 // remember we've seen n
    n = nextValue(n);            // advance to next state
  }
  return n === 1;                // happy iff ended at 1
};
```

#### 🐢 Python — Floyd (O(1) extra space)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            total = 0
            while x > 0:
                d = x % 10
                total += d * d
                x = x // 10
            return total

        slow = n                   # moves 1 step each loop
        fast = next_value(n)       # moves 2 steps each loop

        # Loop until fast hits 1 (happy) or slow meets fast (cycle)
        while fast != 1 and slow != fast:
            slow = next_value(slow)
            fast = next_value(next_value(fast))

        return fast == 1
```

#### 🦊 JavaScript — Floyd (O(1) extra space)

```javascript
var isHappyFloyd = function (n) {
  const nextValue = (x) => {
    let total = 0;
    while (x > 0) {
      const d = x % 10;
      total += d * d;
      x = Math.floor(x / 10);
    }
    return total;
  };

  let slow = n;                 // 1-step pointer
  let fast = nextValue(n);      // 2-step pointer

  // If fast reaches 1 -> happy; if slow meets fast -> cycle -> unhappy
  while (fast !== 1 && slow !== fast) {
    slow = nextValue(slow);
    fast = nextValue(nextValue(fast));
  }
  return fast === 1;
};
```

---

### 🔎 Review

* **Happy example**: `19 → 82 → 68 → 100 → 1` → `true`.
* **Unhappy example**: `2 → 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 ...` (cycle) → `false`.

---

### ⚖️ Evaluate

* **Time**: Each `next` takes `O(#digits)`. Total steps are bounded (values collapse to ≤ 243 quickly), so practical runtime is tiny.
* **Space**: HashSet `O(k)` for small number of states; Floyd `O(1)`.

---

### 🧪 Test Cases

* `1 → true`
* `7 → true` (single-digit happy)
* `19 → true` (classic)
* `2 → false` (classic cycle)
* `100000 → true` (quickly reaches 1)

---

### 🚀 Follow-ups

* Generalize to other bases or use sum of **k-th powers** instead of squares.
* Batch query: list all happy numbers in `[1..N]` (use memo or reuse checks).
* Explain why single-digit happy numbers in base-10 are `{1, 7}` (optional trick).

---

### 📝 Additional Notes (EN)

* **Order of operations**: When extracting digits, do `d = x % 10` **before** `x = x // 10` in Python (or `Math.floor(x/10)` in JS).
* **No infinite loops**: In HashSet, the loop guard `n != 1 && !seen.has(n)`/`n not in seen` is essential.
* **Floyd init**: A common pattern is `slow = n`, `fast = next(n)`. Don’t start both equal to `n`, or you may exit too early.
* **Integer division in JS**: Use `Math.floor(x / 10)`. Avoid `parseInt(x/10)` (works but semantically odd) and avoid `x = x / 10` (not integer).
* **Micro-opt**: Precompute squares `[0,1,4,9,16,25,36,49,64,81]` to remove multiplications—tiny win only.

<br>

# 😆 快樂數（LeetCode 202）— UMPIRE（中文）

### 🐣 理解（Understand）

#### 題意

給定正整數 `n`，重複把 `n` 轉成「各位數字平方和」。若最終變成 `1`，則 `n` 為**快樂數**；若陷入不包含 `1` 的循環，則不是快樂數。回傳布林值。

#### 關鍵直覺

* 位數平方和會很快把數字壓縮到一個很小的有限集合（≤ 243），之後不是到 `1` 就是進入循環 → **循環偵測**問題。

---

### 🧷 匹配（Match）

* 把轉換寫成函式 `next(n) = 位數平方和`。
* 循環偵測方法：

  1. **HashSet**：記錄看過的狀態，重複代表有循環。
  2. **Floyd 快慢指標**：額外空間 `O(1)`。

---

### 🧮 計畫（Plan）

#### 🍯 計畫 A — HashSet

* 當 `n != 1` 且 `n` 未出現過：加入 `seen`，更新 `n = next(n)`。
* 結束時：若 `n == 1` → 快樂；重複 → 不快樂。

#### 🦥 計畫 B — Floyd（快慢指標）

* 設 `slow = n`, `fast = next(n)`。
* 當 `fast != 1` 且 `slow != fast`：

  * `slow = next(slow)`
  * `fast = next(next(fast))`
* 若 `fast == 1` → 快樂；若相遇 → 循環 → 不快樂。

---

### 🛠️ 實作（Implement）

#### 🐼 Python — HashSet（含詳細註解）

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            """回傳 x 的各位數平方和（不使用 divmod，步驟更直觀）。"""
            total = 0
            while x > 0:
                d = x % 10          # 取最後一位數字
                total += d * d      # 平方後累加
                x = x // 10         # 去掉最後一位
            return total

        seen = set()                 # 已出現過的狀態
        # 直到到達 1（快樂）或遇到重複（循環）
        while n != 1 and n not in seen:
            seen.add(n)              # 紀錄目前狀態
            n = next_value(n)        # 前往下一個狀態
        return n == 1                # 結果是否為 1
```

#### 🐙 JavaScript — HashSet（含詳細註解）

```javascript
var isHappy = function (n) {
  const nextValue = (x) => {
    let total = 0;
    while (x > 0) {
      const d = x % 10;             // 取最後一位
      total += d * d;               // 平方後累加
      x = Math.floor(x / 10);       // 去掉最後一位（整數除法）
    }
    return total;
  };

  const seen = new Set();           // 記錄出現過的狀態
  while (n !== 1 && !seen.has(n)) {
    seen.add(n);
    n = nextValue(n);
  }
  return n === 1;
};
```

#### 🐨 Python — Floyd（O(1) 空間）

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            total = 0
            while x > 0:
                d = x % 10
                total += d * d
                x = x // 10
            return total

        slow = n                    # 每回合走一步
        fast = next_value(n)        # 每回合走兩步

        # fast 先到 1 => 快樂；兩者相遇 => 進入循環
        while fast != 1 and slow != fast:
            slow = next_value(slow)
            fast = next_value(next_value(fast))

        return fast == 1
```

#### 🐝 JavaScript — Floyd（O(1) 空間）

```javascript
var isHappyFloyd = function (n) {
  const nextValue = (x) => {
    let total = 0;
    while (x > 0) {
      const d = x % 10;
      total += d * d;
      x = Math.floor(x / 10);
    }
    return total;
  };

  let slow = n;                    // 慢指標（一步）
  let fast = nextValue(n);         // 快指標（兩步）

  // fast 到 1 => 快樂； slow 與 fast 相遇 => 循環
  while (fast !== 1 && slow !== fast) {
    slow = nextValue(slow);
    fast = nextValue(nextValue(fast));
  }
  return fast === 1;
};
```

---

### 🔍 檢查與推演（Review）

* **快樂案例**：`19 → 82 → 68 → 100 → 1` → `true`
* **不快樂案例**：`2 → 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 → ...`（循環）→ `false`

---

### 📊 複雜度與評估（Evaluate）

* **時間**：每次 `next` 是 `O(位數)`；總步數有小上界，實務上極快。
* **空間**：HashSet `O(k)`（k 很小）；Floyd `O(1)`。

---

### 🧫 測試案例（Test Cases）

* `1 → true`
* `7 → true`
* `19 → true`
* `2 → false`
* `100000 → true`

---

### 🌈 延伸討論（Follow-ups）

* 改變進位或把平方換成 `k` 次方；性質如何改變？
* 範圍查詢：找出 `[1..N]` 所有快樂數（可重用單點檢查或記憶化）。

---

### 📌 附加筆記（注意事項）

* **取位順序**：先 `d = x % 10`，再 `x = x // 10`（Python）；JS 用 `Math.floor(x / 10)`。
* **避免死循環**：HashSet 的判斷式要寫成「`n != 1` 且 `n` 未出現」，否則可能無限迴圈。
* **Floyd 初始化**：常見正確寫法是 `slow = n`，`fast = next(n)`；不要都從 `n` 開始，否則可能過早判定相遇。
* **JS 整數除法**：必用 `Math.floor`，別用普通 `/`（那是浮點數）。
* **微優化**：預先建立平方表 `[0,1,4,9,16,25,36,49,64,81]`，雖然提升很小但可讀性也不錯。

<br>

# 🎤 Full Spoken-Style Interview Answer

### 1) Clarify the problem, examples, and constraints — *what I’d say*

“Let me restate the problem to be sure I understand it. I’m given a positive integer `n`. I repeatedly transform it by replacing `n` with the **sum of the squares of its digits**. If the process eventually becomes `1`, we call `n` a *happy* number and return `true`; if instead it falls into a loop that never includes `1`, we return `false`.

Quick example: for `n = 19`, I get `1²+9² = 82`, then `8²+2² = 68`, then `6²+8² = 100`, then `1²+0²+0² = 1`, so `19` is happy. For `n = 2`, the sequence cycles `2→4→16→37→58→89→145→42→20→4→…`, so it’s not happy.

Constraints-wise, `n` is positive. There’s no need for big-integer libraries. One useful intuition is that this digit-square transform quickly shrinks large numbers into a small range—so we either hit `1` or repeat a previous value, which means cycle detection is the core problem.”

---

### 2) Edge cases — *what I’d say*

“I’ll watch out for:

* `n = 1` ➜ trivially happy.
* Numbers like `10` or any `10^k` ➜ immediately go to `1`.
* Large `n` ➜ the transform collapses fast; still fine.
* If the interviewer asks about `n <= 0`, I’d clarify the contract; usually inputs are `n >= 1`.”

---

### 3) Brute-force vs. optimal approach — *what I’d say*

“A straightforward approach is to *iterate the transform* and remember states in a **set**. If I see a state twice before reaching `1`, there’s a cycle, so return `false`. That’s super clear and easy to code.

To reduce space to O(1), I can use **Floyd’s tortoise–hare** cycle detection: apply the transform once for `slow` and twice for `fast`. If `fast` ever hits `1`, we’re happy; if `slow == fast`, there’s a cycle. In interviews, I usually start with the set-based approach for clarity, then mention Floyd for the space optimization.”

---

### 4) Explain and implement the optimal code — *what I’d say while coding*

“I’ll implement two versions. First, the HashSet version because it’s very readable. I’ll write a helper `next_value(x)` that returns the sum of squares of digits by peeling digits with `% 10`(x mod ten) and `// 10`(x floor-divided by ten). Then I’ll loop until I either reach `1` or revisit a number. After that, I’ll show the Floyd version that uses constant extra space.

I’ll code in Python first, then mirror it in JavaScript.”

#### Python — HashSet (clear & friendly)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            """
            Compute the sum of the squares of the digits of x.
            Example: 82 -> 8^2 + 2^2 = 68
            """
            total = 0
            while x > 0:
                d = x % 10        # take last digit
                total += d * d    # add its square
                x = x // 10       # drop last digit
            return total

        seen = set()               # remembers states we've visited
        # Iterate until we reach 1 (happy) or repeat a state (cycle)
        while n != 1 and n not in seen:
            seen.add(n)            # mark current state
            n = next_value(n)      # move to next state
        return n == 1              # true if ended at 1
```

#### JavaScript — HashSet

```javascript
var isHappy = function (n) {
  const nextValue = (x) => {
    // Sum of squares of digits for x
    let total = 0;
    while (x > 0) {
      const d = x % 10;           // last digit
      total += d * d;             // add its square
      x = Math.floor(x / 10);     // drop last digit (integer division)
    }
    return total;
  };

  const seen = new Set();         // visited states
  while (n !== 1 && !seen.has(n)) {
    seen.add(n);
    n = nextValue(n);
  }
  return n === 1;
};
```

#### Python — Floyd’s tortoise–hare (O(1) extra space)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_value(x: int) -> int:
            total = 0
            while x > 0:
                d = x % 10
                total += d * d
                x = x // 10
            return total

        slow = n                  # moves one transform per loop
        fast = next_value(n)      # moves two transforms per loop

        # If fast reaches 1 -> happy; if slow meets fast -> cycle (unhappy)
        while fast != 1 and slow != fast:
            slow = next_value(slow)
            fast = next_value(next_value(fast))

        return fast == 1
```

#### JavaScript — Floyd’s tortoise–hare (O(1) extra space)

```javascript
var isHappyFloyd = function (n) {
  const nextValue = (x) => {
    let total = 0;
    while (x > 0) {
      const d = x % 10;
      total += d * d;
      x = Math.floor(x / 10);
    }
    return total;
  };

  let slow = n;                 // 1-step pointer
  let fast = nextValue(n);      // 2-step pointer

  while (fast !== 1 && slow !== fast) {
    slow = nextValue(slow);
    fast = nextValue(nextValue(fast));
  }
  return fast === 1;
};
```

---

### 5) Time & space complexity — *what I’d say*

“Each `next_value` runs in `O(d)` where `d` is the number of digits. The sequence quickly collapses into a small finite set (≤ 243 in base 10), so total iterations are bounded by a small constant in practice. I’d summarize runtime as effectively constant for typical inputs. Space is `O(k)` for the set approach—again, tiny—or `O(1)` with Floyd.”

---

### 6) Follow-up questions — *what I’d ask or mention*

* “Would you like me to switch to Floyd’s method for `O(1)` extra space? I already have that version.”
* “Should we generalize to other bases or to the sum of **k-th powers** instead of squares?”
* “If we needed all happy numbers in `[1..N]`, I could reuse the single-number check and add memoization.”
* “I can also discuss why single-digit happy numbers in base 10 are `{1, 7}` and how that can be used as a tiny optimization.”

<br>

# 🎯 Real-World Applications｜實際應用場景

### 🔄 Cycle Detection in Systems｜系統中的循環偵測

**EN:** The happy number problem is essentially about detecting whether a process terminates (reaches `1`) or falls into a loop. In real-world systems, cycle detection is critical—for example, in **deadlock detection** in operating systems or databases. You need to know if a process will keep waiting forever in a cycle of dependencies.
**ZH:** 快樂數問題的核心是判斷一個過程是會結束（到達 `1`）還是會陷入循環。在真實系統中，循環偵測非常重要，例如在**作業系統或資料庫的死結偵測**中，需要確認一個程序是否會永遠卡在相互等待的循環中。

---

### 🔍 Detecting Infinite Loops in Algorithms｜演算法中的無窮迴圈偵測

**EN:** In algorithm design, we often need to detect infinite loops. The approach used here—HashSet for seen states, or Floyd’s tortoise-hare pointer method—is directly applicable. For example, in **graph traversal** (like linked lists or functional transformations), we use similar techniques to detect cycles.
**ZH:** 在演算法設計中，我們經常需要判斷是否出現無窮迴圈。這題用到的方法──HashSet 紀錄狀態，或 Floyd 快慢指標──可以直接應用。例如在**圖的遍歷**（像是鏈結串列或函數轉換）中，就會使用類似技巧來檢測循環。

---

### 📱 Input Validation & Number Transformations｜輸入驗證與數字轉換

**EN:** The repeated digit-square transformation resembles real-world tasks like **checksum calculations** (e.g., credit card Luhn algorithm) or **hashing functions**, where you repeatedly manipulate digits and check properties.
**ZH:** 這題裡「數字平方和」的轉換過程，和真實場景中的一些任務很像，例如**檢查碼計算**（像信用卡的 Luhn 演算法）或**雜湊函式**，都會反覆處理數字並檢驗某些性質。

---

### 🕹️ Game State Validation｜遊戲狀態驗證

**EN:** In game development or simulations, you often need to determine if a state will eventually stabilize or loop forever. Detecting whether a game configuration reaches a winning state (`1`) or cycles endlessly is similar to checking happy numbers.
**ZH:** 在遊戲開發或模擬中，常常要判斷一個狀態會不會最終穩定下來或是無限循環。判斷一個遊戲局面是否最終達到勝利狀態（對應 `1`），還是會一直重複，和快樂數問題非常類似。

---

### 📊 Data Quality Checks｜資料品質檢查

**EN:** In data engineering pipelines, sometimes transformations need to be validated to ensure they don’t enter non-terminating loops. The same cycle-detection strategies can be used to confirm whether repeated transformations converge or cycle.
**ZH:** 在資料工程流程中，某些資料轉換需要驗證，確保它們不會陷入無窮迴圈。像這題使用的循環檢測策略，就可以用來判斷反覆轉換是否會收斂到結果，還是陷入循環。

