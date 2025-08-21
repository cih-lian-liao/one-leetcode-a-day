# 🎯 LeetCode 289 — Game of Life (Marker Encoding)｜UMPIRE Notes (English)

### 🧠 Understand

#### Problem

* Input: `m x n` grid `board` where each cell is `0` (dead) or `1` (alive).
* Output: Modify `board` **in place** to its **next generation**, applying Conway’s rules simultaneously:

  1. Live cell with **< 2** live neighbors → dies (underpopulation).
  2. Live cell with **2 or 3** live neighbors → lives.
  3. Live cell with **> 3** live neighbors → dies (overpopulation).
  4. Dead cell with **exactly 3** live neighbors → becomes live (reproduction).
* Neighbor set: the 8 surrounding cells. Counting must use the **original** state.

#### Constraints

* Must be **in‑place** with **O(1)** extra space (no full copy).
* Edges/corners have fewer neighbors—guard array bounds carefully.

---

### 🧩 Match

#### Pattern Recognition

* Grid simulation + local neighbor counting.
* **In‑place trick used here: Marker Encoding.**

  * We write temporary markers so we can:

    * Keep reading the **original** state during counting, and
    * Remember the **next** state for each cell, all in the same grid.

#### Marker Scheme

* `1 → -1`: was live, will die.
* `0 → 2` : was dead, will live.
* Reading rule while counting neighbors:

  * **Originally live** ⇒ values `1` **or** `-1`
  * **Originally dead** ⇒ values `0` **or** `2`

---

### 🗺️ Plan

#### Two Passes

1. **Pass 1 (write markers):**

   * For every cell, count **original** live neighbors using the reading rule above.
   * Apply Conway’s rules:

     * If a live cell will die, set it to `-1`.
     * If a dead cell will live, set it to `2`.
     * Otherwise, leave it as is (`0` or `1`).
2. **Pass 2 (normalize):**

   * Convert markers to final 0/1:

     * Values `> 0` → `1` (live)
     * Values `<= 0` → `0` (dead)

#### Neighbor Directions

`[(-1,-1), (-1,0), (-1,1), (0,-1), (0,1), (1,-1), (1,0), (1,1)]`

---

### 🛠️ Implement

#### Python — Marker Encoding (in‑place, O(1) extra space)

```python
# 289. Game of Life — Marker Encoding (Python)
# Strategy:
#   Pass 1: write markers using original-state reading rule
#           - live -> dead :  1 -> -1
#           - dead -> live :  0 ->  2
#           (when counting neighbors, treat 1 and -1 as originally live)
#   Pass 2: normalize
#           - value > 0  -> 1
#           - else       -> 0
from typing import List

class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])

        # 8 neighbor offsets
        dirs = [(-1, -1), (-1,  0), (-1,  1),
                ( 0, -1),           ( 0,  1),
                ( 1, -1), ( 1,  0), ( 1,  1)]

        # Helper: whether a cell was originally live (before marking)
        def was_live(v: int) -> bool:
            # Originally live means v was 1, or has been marked -1 (1 -> -1)
            return v == 1 or v == -1

        # ----- Pass 1: decide next state and write markers -----
        for r in range(rows):
            for c in range(cols):
                # Count original live neighbors
                live_neighbors = 0
                for dr, dc in dirs:
                    rr, cc = r + dr, c + dc
                    if 0 <= rr < rows and 0 <= cc < cols and was_live(board[rr][cc]):
                        live_neighbors += 1

                cell = board[r][c]
                if cell == 1:
                    # Live cell: survives with 2 or 3 live neighbors; otherwise dies
                    if live_neighbors < 2 or live_neighbors > 3:
                        board[r][c] = -1  # mark live -> dead
                else:  # cell == 0
                    # Dead cell: becomes live with exactly 3 live neighbors
                    if live_neighbors == 3:
                        board[r][c] = 2   # mark dead -> live

        # ----- Pass 2: normalize markers to final 0/1 -----
        for r in range(rows):
            for c in range(cols):
                board[r][c] = 1 if board[r][c] > 0 else 0
```

#### JavaScript — Marker Encoding (in‑place, O(1) extra space)

```javascript
// 289. Game of Life — Marker Encoding (JavaScript)
// Plan:
//   Pass 1: write markers
//     - 1 -> -1  (live -> dead)
//     - 0 ->  2  (dead -> live)
//     When counting, treat 1 and -1 as "originally live".
//   Pass 2: normalize
//     - value > 0 => 1
//     - else      => 0
var gameOfLife = function(board) {
  if (!board || !board.length || !board[0].length) return;

  const rows = board.length;
  const cols = board[0].length;

  const dirs = [
    [-1,-1], [-1, 0], [-1, 1],
    [ 0,-1],          [ 0, 1],
    [ 1,-1], [ 1, 0], [ 1, 1],
  ];

  const wasLive = (v) => v === 1 || v === -1; // originally live

  // ----- Pass 1: write markers -----
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      // Count original live neighbors
      let liveNeighbors = 0;
      for (const [dr, dc] of dirs) {
        const rr = r + dr, cc = c + dc;
        if (rr >= 0 && rr < rows && cc >= 0 && cc < cols && wasLive(board[rr][cc])) {
          liveNeighbors++;
        }
      }

      const cell = board[r][c];
      if (cell === 1) {
        // Live -> survives only with 2 or 3 neighbors
        if (liveNeighbors < 2 || liveNeighbors > 3) {
          board[r][c] = -1; // mark live -> dead
        }
      } else { // 0
        // Dead -> becomes live with exactly 3 neighbors
        if (liveNeighbors === 3) {
          board[r][c] = 2;  // mark dead -> live
        }
      }
    }
  }

  // ----- Pass 2: normalize to final 0/1 -----
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      board[r][c] = board[r][c] > 0 ? 1 : 0;
    }
  }
};
```

---

### 🔍 Review (Dry‑run)

#### Example

Input

```
0 1 0
0 0 1
1 1 1
0 0 0
```

Expected next

```
0 0 0
1 0 1
0 1 1
0 1 0
```

* During Pass 1, you’ll mark transitions (e.g., `0 -> 2` for births and `1 -> -1` for deaths) while still counting neighbors using the **original** state rule (`1` and `-1` are both originally live).
* Pass 2 collapses markers into `0/1` and matches the expected output.

---

### ✅ Evaluate

* **Time complexity:** `O(mn)` (each cell checks ≤ 8 neighbors).
* **Space complexity:** `O(1)` extra (pure in‑place).
* **Correctness:** Guaranteed by reading original states during counting and normalizing in the second pass.

---

### 📝 Extra Notes & Pitfalls (for Marker Encoding)

* **Original-state reading rule is critical.** When counting, treat `1` and `-1` as live; `0` and `2` as dead.
* **Don’t normalize early.** Only convert markers to `0/1` after the entire first pass is done.
* **Bounds checks** for neighbors must be exact; off‑by‑one bugs are common.
* **Testing tips:** try all‑zeros, all‑ones, single row/column, oscillators (blinker), still lifes (block), and gliders.
* **When to prefer marker encoding vs bit‑packing:** marker encoding is conceptually simpler if you’re new to bitwise ops; bit‑packing is also O(1) but uses shifting and masking.

<br>

# 🧬 生命遊戲（標記編碼法）｜UMPIRE 筆記（中文）

### 🧐 理解（U）

#### 題目重點

* 輸入：`m x n` 棋盤 `board`，每格為 `0`（死）或 `1`（活）。
* 輸出：**原地** 更新為 **下一代**，同時套用規則：

  1. 活細胞活鄰居 **< 2** → 死（稀疏）。
  2. 活細胞活鄰居 **= 2 或 3** → 活。
  3. 活細胞活鄰居 **> 3** → 死（擁擠）。
  4. 死細胞活鄰居 **= 3** → 變活（繁殖）。
* 鄰居：周圍八格；計數必須以 **原始狀態** 為準。

#### 限制

* 額外空間 **O(1)**（不能建立同尺寸副本）。
* 邊界與角落要小心避免越界。

---

### 🧷 對應（M）

#### 類型與技巧

* 問題類型：網格模擬＋鄰居計數。
* **原地技巧：標記編碼（Marker Encoding）**
  我們用暫時的「標記值」記錄「即將變化」，同時仍能讀到「原始狀態」。

#### 標記規則

* `1 → -1`：原本活 → 下一代死。
* `0 → 2` ：原本死 → 下一代活。
* 計數時的原始狀態判定：

  * **原本活** ⇒ `1`、`-1`
  * **原本死** ⇒ `0`、`2`

---

### 🧭 規劃（P）

#### 兩輪流程

1. **第一輪（寫入標記）**

   * 逐格計數「原本活」的鄰居數（把 `1`、`-1` 視為活）。
   * 套用規則：

     * 活且將死 → 設 `-1`
     * 死且將活 → 設 `2`
     * 其他維持原值（`0` 或 `1`）
2. **第二輪（正規化）**

   * 統一轉回 `0/1`：`> 0 → 1`，`<= 0 → 0`

#### 八方向

`[(-1,-1), (-1,0), (-1,1), (0,-1), (0,1), (1,-1), (1,0), (1,1)]`

---

### 🧰 實作（I）

#### Python — 標記編碼（原地，額外空間 O(1)）

```python
# 289. Game of Life — 標記編碼（Python）
# 作法：
#   第一輪：寫入標記
#     - 1 -> -1（原活將死）
#     - 0 ->  2（原死將活）
#     計數時把 1 與 -1 都視為「原本活」。
#   第二輪：正規化
#     - 值 > 0  -> 1
#     - 其餘    -> 0
from typing import List

class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])
        dirs = [(-1,-1), (-1,0), (-1,1),
                ( 0,-1),          ( 0, 1),
                ( 1,-1), ( 1, 0), ( 1, 1)]

        def was_live(v: int) -> bool:
            # 原本活：1 或 -1（-1 表示從活變死的標記）
            return v == 1 or v == -1

        # 第一輪：寫入標記
        for r in range(rows):
            for c in range(cols):
                live_neighbors = 0
                for dr, dc in dirs:
                    rr, cc = r + dr, c + dc
                    if 0 <= rr < rows and 0 <= cc < cols and was_live(board[rr][cc]):
                        live_neighbors += 1

                val = board[r][c]
                if val == 1:
                    # 活：只有 2 或 3 個活鄰居才存活
                    if live_neighbors < 2 or live_neighbors > 3:
                        board[r][c] = -1  # 活 -> 死
                else:  # 0
                    # 死：剛好 3 個活鄰居才復活
                    if live_neighbors == 3:
                        board[r][c] = 2   # 死 -> 活

        # 第二輪：正規化為 0/1
        for r in range(rows):
            for c in range(cols):
                board[r][c] = 1 if board[r][c] > 0 else 0
```

#### JavaScript — 標記編碼（原地，額外空間 O(1)）

```javascript
// 289. Game of Life — 標記編碼（JavaScript）
// 流程：
//   第一輪：寫入標記
//     - 1 -> -1（原活將死）
//     - 0 ->  2（原死將活）
//     計數時把 1 與 -1 視為「原本活」。
//   第二輪：正規化
//     - 值 > 0 -> 1
//     - 其餘   -> 0
var gameOfLife = function(board) {
  if (!board || !board.length || !board[0].length) return;

  const rows = board.length;
  const cols = board[0].length;
  const dirs = [
    [-1,-1], [-1, 0], [-1, 1],
    [ 0,-1],          [ 0, 1],
    [ 1,-1], [ 1, 0], [ 1, 1],
  ];

  const wasLive = (v) => v === 1 || v === -1; // 原本活

  // 第一輪：寫入標記
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      let liveNeighbors = 0;

      for (const [dr, dc] of dirs) {
        const rr = r + dr, cc = c + dc;
        if (rr >= 0 && rr < rows && cc >= 0 && cc < cols && wasLive(board[rr][cc])) {
          liveNeighbors++;
        }
      }

      const v = board[r][c];
      if (v === 1) {
        if (liveNeighbors < 2 || liveNeighbors > 3) {
          board[r][c] = -1; // 活 -> 死
        }
      } else { // 0
        if (liveNeighbors === 3) {
          board[r][c] = 2;  // 死 -> 活
        }
      }
    }
  }

  // 第二輪：正規化
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      board[r][c] = board[r][c] > 0 ? 1 : 0;
    }
  }
};
```

---

### 🔬 檢查（R）

#### 範例

輸入

```
0 1 0
0 0 1
1 1 1
0 0 0
```

期望

```
0 0 0
1 0 1
0 1 1
0 1 0
```

* 第一輪會把「將死」標 `-1`、「將活」標 `2`，計數一律把 `1`、`-1` 視為「原本活」。
* 第二輪正規化後即可得到期望結果。

---

### ✅ 評估（E）

* **時間**：`O(mn)`（每格最多檢查 8 個鄰居）。
* **空間**：`O(1)`（原地操作）。
* **正確性**：靠「原始狀態判定規則」與「第二輪正規化」確保同步更新。

---

### 📝 附加筆記（注意事項）

* **關鍵判定**：計數時，`1` 與 `-1` 都視為「原本活」；`0` 與 `2` 視為「原本死」。
* **分兩輪**：第一輪只負責「決定並標記」；第二輪才「正規化」。不要在第一輪中途把標記立刻轉成 0/1。
* **邊界檢查**：`0 <= rr < rows`、`0 <= cc < cols` 兩個條件都要同時成立，避免越界。
* **測資建議**：

  * 全 0、全 1、單列/單行、非方形。
  * 經典圖樣（block、beehive、blinker、toad、glider）。
* **可讀性**：

  * 把八方向抽成常數陣列。
  * 命名清楚如 `liveNeighbors`、`wasLive`、`normalize`（若拆函式）。
* **效能**：

  * 兩層迴圈 + 8 鄰居已是理論下限；避免在內圈建立新物件或做不必要運算。
* **與位元法比較**：

  * 標記法不需要位元運算概念，易懂；
  * 位元法也為 O(1) 空間，適合熟悉位元操作時使用。

<br>

# 🎮 LeetCode 289 — Game of Life｜UMPIRE Notes (Bit Packing Method)

### 🧠 Understand

#### Problem

* Input: `m x n` grid `board` with cells `0` (dead) or `1` (alive).
* Output: Modify `board` **in place** to its **next generation** using Conway’s rules:

  1. Live cell with **< 2** live neighbors → dies.
  2. Live cell with **2 or 3** live neighbors → lives.
  3. Live cell with **> 3** live neighbors → dies.
  4. Dead cell with **exactly 3** live neighbors → becomes live.
* All cells update **simultaneously** (neighbor counts must use the **original** state).

#### Constraints & Implications

* Must be **O(1)** extra space (no same‑size copy).
* Neighbor set is the 8 surrounding cells; be careful at edges and corners.

---

### 🧩 Match

* Pattern: **grid simulation** with **local neighbor counting**.
* In‑place tricks:

  * **Bit packing (recommended)**: store current in bit0, next in bit1.
  * **Marker encoding**: 1→-1 (was live, will die), 0→2 (was dead, will live).

---

### 🗺️ Plan

**Bit packing (2 passes)**

1. First pass:

   * For each cell, count live neighbors via `(board[r][c] & 1)` to read **original** state only.
   * Decide next state; if next is live, set bit1 using `board[r][c] |= 2`.
2. Second pass:

   * Finalize by right‑shifting every cell once: `board[r][c] >>= 1`.

**Neighbor directions**
`[(-1,-1), (-1,0), (-1,1), (0,-1), (0,1), (1,-1), (1,0), (1,1)]`

---

### 🛠️ Implement

#### Python (Bit packing, in‑place, O(1) extra space)

```python
# LeetCode 289 - Game of Life (Python, Bit Packing)
# Idea:
# - Keep ORIGINAL state in bit0 (LSB): x & 1
# - Write NEXT state into bit1: x |= 2 (binary 10)
# - After first pass, shift every cell right by 1: x >>= 1 (to finalize)
from typing import List

class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        # Guard for empty inputs
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])

        # All 8 neighbor coordinate deltas
        dirs = [(-1,-1), (-1,0), (-1,1),
                ( 0,-1),          ( 0, 1),
                ( 1,-1), ( 1, 0), ( 1, 1)]

        # Pass 1: compute the NEXT state and store it in bit1
        for r in range(rows):
            for c in range(cols):
                # Count live neighbors from the ORIGINAL state (bit0)
                live_neighbors = 0
                for dr, dc in dirs:
                    rr, cc = r + dr, c + dc
                    # Bound check
                    if 0 <= rr < rows and 0 <= cc < cols:
                        # Only read bit0, ignore bit1 (which may already be written elsewhere)
                        live_neighbors += (board[rr][cc] & 1)

                cur = board[r][c] & 1  # ORIGINAL state of this cell

                # Apply Conway's rules to compute NEXT state (1 = live, 0 = dead)
                next_live = 0
                if cur == 1:
                    # Live cell stays alive if it has 2 or 3 live neighbors
                    if live_neighbors == 2 or live_neighbors == 3:
                        next_live = 1
                else:
                    # Dead cell becomes alive if it has exactly 3 live neighbors
                    if live_neighbors == 3:
                        next_live = 1

                # If next state is live, set bit1 (binary 10)
                if next_live == 1:
                    board[r][c] |= 2

        # Pass 2: finalize by shifting right to move NEXT state (bit1) into bit0
        for r in range(rows):
            for c in range(cols):
                board[r][c] >>= 1
```

#### JavaScript (Bit packing, in‑place, O(1) extra space)

```javascript
// LeetCode 289 - Game of Life (JavaScript, Bit Packing)
// Strategy:
// - bit0 holds the ORIGINAL state (x & 1)
// - bit1 will hold the NEXT state       (x |= 2)
// - After pass1, shift right once to finalize (x >>= 1)
var gameOfLife = function(board) {
  if (!board || !board.length || !board[0].length) return;

  const rows = board.length;
  const cols = board[0].length;

  // 8 neighbor directions
  const dirs = [
    [-1,-1], [-1, 0], [-1, 1],
    [ 0,-1],          [ 0, 1],
    [ 1,-1], [ 1, 0], [ 1, 1],
  ];

  // Pass 1: compute NEXT into bit1 using ORIGINAL bit0 for counts
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      let liveNeighbors = 0;

      // Count ORIGINAL live neighbors
      for (const [dr, dc] of dirs) {
        const rr = r + dr, cc = c + dc;
        if (rr >= 0 && rr < rows && cc >= 0 && cc < cols) {
          liveNeighbors += (board[rr][cc] & 1); // read only bit0
        }
      }

      const cur = board[r][c] & 1; // ORIGINAL

      // Conway's rules -> NEXT state
      let nextLive = 0;
      if (cur === 1) {
        // Stay alive with 2 or 3 neighbors
        if (liveNeighbors === 2 || liveNeighbors === 3) nextLive = 1;
      } else {
        // Dead becomes alive with exactly 3 neighbors
        if (liveNeighbors === 3) nextLive = 1;
      }

      // If NEXT is live, set bit1
      if (nextLive === 1) {
        board[r][c] |= 2;
      }
    }
  }

  // Pass 2: finalize by shifting right 1 (bit1 -> bit0)
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      board[r][c] >>= 1;
    }
  }
};
```

---

### 🔎 Review (Dry‑run)

**Example**

Input

```
0 1 0
0 0 1
1 1 1
0 0 0
```

Expected next

```
0 0 0
1 0 1
0 1 1
0 1 0
```

* (1,1)=0 has exactly 3 live neighbors → becomes 1.
* (0,1)=1 has 2 live neighbors → stays 1.
* (1,2)=1 has 2 live neighbors → stays 1.
* Others follow under/over‑population rules.

---

### ✅ Evaluate

* **Time**: `O(mn)` (each cell checks up to 8 neighbors).
* **Space**: `O(1)` extra (in‑place).
* Correctness hinges on always reading **original** states for counting.

---

### ✍️ Extra Notes (Implementation Pitfalls)

* Use `(cell & 1)` when counting neighbors; do **not** read the tentative next bit.
* Remember the **second pass** (`>>= 1`) to finalize.
* Carefully handle bounds at edges/corners—no negative or out‑of‑range indices.
* If you prefer **marker encoding**, count “originally live” as `v == 1 or v == -1` and “originally dead” as `v == 0 or v == 2`, then normalize.
* For multiple generations `k`, you can repeat the two‑pass process `k` times; discuss cycle detection only for small boards if asked.
* Tests to try: all zeros, all ones, single row/column, checkerboard patterns, oscillators (e.g., blinker), still lifes (block).

<br>

# 🧬 生命遊戲｜UMPIRE 筆記（中文）

### 🧐 U — 理解

* 輸入：`m x n` 棋盤 `board`，元素為 `0`（死）或 `1`（活）。
* 輸出：**原地** 更新到 **下一代**，規則：

  1. 活細胞活鄰居 **< 2** → 死（稀疏）。
  2. 活細胞活鄰居 **為 2 或 3** → 活。
  3. 活細胞活鄰居 **> 3** → 死（擁擠）。
  4. 死細胞活鄰居 **= 3** → 變活。
* **同時更新**：鄰居計數一律以 **原始狀態** 為準。

---

### 🧷 M — 對應

* 類型：**網格模擬**＋**鄰居計數**。
* 原地技巧：

  * **位元打包（推薦）**：bit0 放原始、bit1 放下一代。
  * **標記編碼**：1→-1（原活將死）、0→2（原死將活）。

---

### 🧭 P — 規劃

**位元法（兩輪）**

1. 第一次掃描：

   * 用 `(board[r][c] & 1)` 讀原始狀態並計數。
   * 若下一代為活，對該格 `|= 2` 設定 bit1。
2. 第二次掃描：

   * 全板右移一位 `>>= 1`，把下一代狀態落位到 bit0。

**鄰居方向**
`[(-1,-1), (-1,0), (-1,1), (0,-1), (0,1), (1,-1), (1,0), (1,1)]`

---

### 🧰 I — 實作

#### Python（位元法，原地，額外空間 O(1)）

```python
# 289. Game of Life（Python，位元打包）
# 作法：
# - bit0 = 原始（用 & 1 讀）
# - bit1 = 下一代（用 |= 2 寫）
# - 第二輪每格 >> 1 完成更新
from typing import List

class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])
        dirs = [(-1,-1), (-1,0), (-1,1),
                ( 0,-1),          ( 0, 1),
                ( 1,-1), ( 1, 0), ( 1, 1)]

        # 第一次：計算下一代，寫入 bit1
        for r in range(rows):
            for c in range(cols):
                live_neighbors = 0
                for dr, dc in dirs:
                    rr, cc = r + dr, c + dc
                    if 0 <= rr < rows and 0 <= cc < cols:
                        live_neighbors += (board[rr][cc] & 1)  # 只讀原始 bit0

                cur = board[r][c] & 1  # 此格的原始狀態
                next_live = 0
                if cur == 1:
                    if live_neighbors == 2 or live_neighbors == 3:
                        next_live = 1
                else:
                    if live_neighbors == 3:
                        next_live = 1

                if next_live:
                    board[r][c] |= 2  # 設定 bit1 為 1

        # 第二次：右移一位，讓下一代進入 bit0
        for r in range(rows):
            for c in range(cols):
                board[r][c] >>= 1
```

#### JavaScript（位元法，原地，額外空間 O(1)）

```javascript
// 289. Game of Life（JavaScript，位元打包）
// 重點：計數時只看 bit0（& 1），下一代寫到 bit1，最後統一 >> 1。
var gameOfLife = function(board) {
  if (!board || !board.length || !board[0].length) return;

  const rows = board.length;
  const cols = board[0].length;
  const dirs = [
    [-1,-1], [-1, 0], [-1, 1],
    [ 0,-1],          [ 0, 1],
    [ 1,-1], [ 1, 0], [ 1, 1],
  ];

  // 第一次：計算下一代，寫入 bit1
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      let liveNeighbors = 0;

      for (const [dr, dc] of dirs) {
        const rr = r + dr, cc = c + dc;
        if (rr >= 0 && rr < rows && cc >= 0 && cc < cols) {
          liveNeighbors += (board[rr][cc] & 1); // 只讀原始狀態
        }
      }

      const cur = (board[r][c] & 1);
      let nextLive = 0;

      if (cur === 1) {
        if (liveNeighbors === 2 || liveNeighbors === 3) nextLive = 1;
      } else {
        if (liveNeighbors === 3) nextLive = 1;
      }

      if (nextLive) board[r][c] |= 2; // 寫入下一代到 bit1
    }
  }

  // 第二次：右移一位完成更新
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      board[r][c] >>= 1;
    }
  }
};
```

---

### 🔬 R — 檢查（手算）

範例輸入

```
0 1 0
0 0 1
1 1 1
0 0 0
```

期望下一代

```
0 0 0
1 0 1
0 1 1
0 1 0
```

重點格：

* (1,1)=0 → 活鄰居恰 3 → 變活。
* (0,1)=1 → 活鄰居為 2 → 繼續活。
* (1,2)=1 → 活鄰居為 2 → 繼續活。

---

### 📏 E — 評估

* **時間**：`O(mn)`（每格最多看 8 鄰居）。
* **空間**：`O(1)` 額外空間（原地）。
* **正確性**：鄰居計數一律以 **原始狀態** 為準（`& 1`）。

---

### 📝 附加筆記（寫程式注意事項）

* **同時更新**的核心：計數時一定只看 **bit0**，避免被已寫入的 bit1 汙染。
* **別忘第二輪**：右移 `>>= 1`（或標記法的正規化）。
* **邊界檢查**：`0 <= rr < rows` 與 `0 <= cc < cols` 缺一不可。
* **測資覆蓋**：

  * 全 0、全 1、單列/單行、非方形。
  * 經典圖樣：靜態（block、beehive）、振盪器（blinker、toad）、滑翔機（glider）。
* **可替代作法（標記法）**：

  * 計數時把 `1`、`-1` 視為「原本活」，`0`、`2` 視為「原本死」。
  * 第一輪完成後，把 `> 0` 轉成 `1`，其餘轉 `0`。
  * 位元法較直觀且常見於面試。
* **多代演進**：小的 `k` 直接重複；大的 `k` 可討論狀態哈希與循環偵測（小板子時才實用）。
* **可讀性**：把 8 個方向獨立成常數陣列，讓主迴圈更乾淨；變數命名如 `liveNeighbors`、`nextLive` 清楚易懂。
* **效能細節**：巢狀兩層迴圈已是必要代價；避免在內圈分配新陣列或做多餘運算。


<br>

# 🧭 標記編碼 vs 位元法：對照表

### 🧱 概念速覽

| 面向       | 標記編碼（Marker Encoding）                      | 位元法（Bit Packing）                              |               |
| -------- | ------------------------------------------ | --------------------------------------------- | ------------- |
| 核心想法     | 用**臨時標記**在原地記錄「將變化」：`1→-1`（活→死）、`0→2`（死→活） | 在同一格的**不同 bit**同時存「原始」與「下一代」：bit0=原始、bit1=下一代 |               |
| 讀取原始狀態   | `1` 與 `-1` 視為「原本活」；`0` 與 `2` 視為「原本死」       | `cell & 1` 取得原始（只看 bit0）                      |               |
| 寫入下一代    | 活→死：設 `-1`；死→活：設 `2`；其他維持原值                | 若下一代為活：\`cell                                 | = 2\`（設 bit1） |
| 最終收斂     | 第二輪正規化：`>0 → 1`、`<=0 → 0`                  | 第二輪右移：`cell >>= 1`（bit1 下移到 bit0）             |               |
| 值域 & 可讀性 | 值可能為 `-1, 0, 1, 2`；語意直觀（會看到「將死/將活」標記）      | 值為 `0..3`（二進位 `00~11`）；需理解位元運算                |               |
| 記憶體      | `O(1)`                                     | `O(1)`                                        |               |
| 時間       | `O(mn)`                                    | `O(mn)`                                       |               |
| 同步更新安全性  | 以「原本活/死」判定規則避免互相汙染                         | 以 `& 1` 只讀 bit0，避免互相汙染                        |               |
| 邊界易錯點    | 忘了把 `-1` 當「原本活」、`2` 當「原本死」                 | 忘了 `& 1` 只讀原始、或忘了最後 `>>=1`                    |               |
| 何時選用     | **沒學位元也能上手**，語意清楚、教學友善                     | **熟悉位元時最佳**；程式碼緊湊、少分支                         |               |
| 面試敘述亮點   | 強調「中間標記」的語意與第二輪正規化                         | 強調「雙軌存儲」（bit0/bit1）與兩輪流程                      |               |

<br>


# 🔎 常見 Bug 清單（含快速自查）

### 🧨 通用（兩種方法都會踩）

* 邊界檢查寫錯：

  * ✅ `0 <= rr < rows` 且 `0 <= cc < cols` 必須同時成立。
  * ❌ 用 `<=` 導致越界（應該是 `<`）。
* 把**自己**算進鄰居：

  * ✅ 只遍歷 8 個方向，不包含 `(0,0)`。
* 方向陣列打錯或少方向：

  * ✅ 固定使用 8 向：`[(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]`。
* 規則判斷合併錯誤：

  * ✅ 活存活條件僅 `2 或 3`；死復活條件僅 `== 3`。
  * ❌ 寫成 `>= 3` 或把兩條件混在一起。
* 同步更新概念忘記了：

  * ✅ 第一輪**只計數與標記/設位**，**不要**立刻把值轉成 0/1。

---

### 🧯 標記編碼專屬

* **忘了原始狀態讀取規則**：

  * ✅ 計數時把 `1`、`-1` 視為活；`0`、`2` 視為死。
* **第一輪中途正規化**：

  * ❌ 一邊標記一邊把 `-1/2` 轉 `0/1`。
  * ✅ 必須全部標記完成後，**第二輪**一次性正規化。
* **正規化判斷寫錯**：

  * ✅ `> 0 → 1`、`<= 0 → 0`。
  * ❌ 只判斷 `== 2` 或 `== -1`，容易漏況。
* **Python 判斷陷阱**：

  * 使用布林語境時，`-1` 在 Python 屬於 Truthy，別用 `if cell:` 來判定「是否原本活」。

---

### 🧲 位元法專屬

* **計數沒用 `& 1`**：

  * ❌ 直接加 `board[rr][cc]`，會把 bit1（下一代）也算進去。
  * ✅ 一律 `live += (board[rr][cc] & 1)`。
* **忘了最後右移**：

  * ❌ 少了 `cell >>= 1`，導致棋盤仍是「雙位混合」。
  * ✅ 第二輪**必做**右移收斂。
* **把設位寫成 `|= 1`**：

  * ❌ 這是在設 bit0，不是寫下一代。
  * ✅ 應該是 `|= 2`（二進位 `10`，設 bit1）。
* **使用錯誤運算子**（JS）：

  * ❌ 用邏輯或 `||=` 取代位元或 `|=`。
  * ✅ 必須使用位元或 `|=`。
* **先右移再計數**：

  * ❌ 破壞原始狀態。
  * ✅ 兩輪順序：**先**全盤計數＋寫 bit1，**再**右移收斂。

---

### 🧪 最小再現測資（快速抓 bug）

* **全 1 小盤**（易出現過度擁擠規則錯）：

  ```
  1 1
  1 1
  ```
* **全 0**（不應有誕生）：

  ```
  0 0
  0 0
  ```
* **單列/單行**（邊界與方向）：

  ```
  0 1 0 1 0
  ```
* **經典振盪器 blinker**（檢查同步更新）：

  ```
  0 1 0
  0 1 0
  0 1 0
  ```

  下一代應為：

  ```
  0 0 0
  1 1 1
  0 0 0
  ```

---

### 🧰 語言小提醒

* **Python**

  * 別用 `if cell:` 來代表「活」，標記階段會把 `-1` 誤判為 True。

    * ✅ 用 `if cell == 1:` 或 `if (cell & 1):`（位元法）。
  * 巢狀迴圈中避免建立新 list（如方向陣列）以免不必要的配置。
* **JavaScript**

  * 位元運算會把數字轉為 32 位整數，這裡的值域很小（0\~3 或 -1/2），**安全**。
  * 小心把 `|`（位元或）誤寫成 `||`（邏輯或）。

---

### 🧠 決策小抄（面試時怎麼說）

* **如果你還不熟位元運算**：先選 **標記編碼**，語意直觀、好講解。
* **如果你熟悉位元**：選 **位元法**，強調「bit0=原始、bit1=下一代，兩輪右移收斂」的同步更新思路。
* 都要提到：**兩輪流程**、**同步更新**、**O(1) 空間**、**O(mn) 時間**、**邊界檢查**。


<br>

# 🎤 Full Spoken-Style Interview Answer

> I’ll walk through my understanding, call out edge cases, weigh approaches, then live‑narrate an in‑place implementation (marker encoding), explain complexity, and end with thoughtful follow‑ups.



### 1) Clarify the problem (out loud) — set the stage

“Here’s how I understand the task: I’m given an `m x n` grid `board` where each cell is `0` (dead) or `1` (alive). I need to update the board **in place** to the **next generation** of Conway’s Game of Life. Each cell has up to 8 neighbors. The simultaneous update rules are:

* Live with **< 2** live neighbors → dies (underpopulation)
* Live with **2 or 3** live neighbors → lives
* Live with **> 3** live neighbors → dies (overpopulation)
* Dead with **exactly 3** live neighbors → becomes live (reproduction)

Two details I’ll keep front‑of‑mind: (1) updates are **simultaneous**, so neighbor counts must use the **original** state; and (2) I should mutate the input grid, ideally with `O(1)` extra space. Are there any additional constraints I should assume, such as max dimensions or non‑binary inputs? If not, I’ll proceed with the standard constraints.”

**Reference example I’ll sanity‑check against:**

```
Input:
0 1 0
0 0 1
1 1 1
0 0 0

Expected next:
0 0 0
1 0 1
0 1 1
0 1 0
```

“This example highlights why simultaneous updates matter—if I overwrite values too early, later neighbor counts become wrong.”

---

### 2) Edge cases — make failure modes explicit

* Empty grid or degenerate sizes: `[]`, `[[1]]`, single row/column.
* All zeros (stays zeros).
* All ones (many should die from overcrowding).
* Non‑square boards.
* Boundaries & corners (must guard indices).
* Ensure I **never** count the cell itself as a neighbor.

---

### 3) Approaches — trade‑off discussion

**Brute force (extra board):**
“Copy the board, compute next states from the original, write to the copy, then copy back. Time `O(mn)`, but space `O(mn)`; trivial to implement, not in‑place.”

**In‑place options (both `O(1)` extra space):**

1. **Marker encoding** *(I’ll implement this; it’s very readable)*

   * Write temporary markers to remember the future without losing the past:

     * `1 -> -1` means “was live, will die”
     * `0 ->  2` means “was dead, will live”
   * When counting neighbors, treat `1` and `-1` as **originally live**; treat `0` and `2` as **originally dead**.
   * After the first pass, **normalize**: `> 0 → 1`, `<= 0 → 0`.

2. **Bit packing** *(equally good, more “bit‑y”)*

   * Keep **original** in bit0, write **next** to bit1: read with `& 1`, set next with `|= 2`, finalize with `>>= 1`.

“I’ll code the **marker encoding** version because it’s easy to narrate and proof against common bitwise mistakes in interviews.”

---

### 4) Implement (live narration) — marker encoding

“I’ll define the 8 directions once. Then I’ll do **two passes**:

* Pass 1: count original‑live neighbors; write markers `-1` or `2` when a transition happens.
* Pass 2: normalize markers to final `0/1`.”

#### Python (with step‑by‑step comments)

```python
from typing import List

class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        # Guard: empty inputs
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])

        # 8 neighbor directions (rowDelta, colDelta)
        dirs = [(-1, -1), (-1,  0), (-1,  1),
                ( 0, -1),           ( 0,  1),
                ( 1, -1), ( 1,  0), ( 1,  1)]

        # Helper: interpret ORIGINAL state during Pass 1
        # Originally-live means value is 1 (still live) or -1 (marked live->dead)
        def was_live(v: int) -> bool:
            return v == 1 or v == -1

        # ---------- Pass 1: decide next state and write markers ----------
        for r in range(rows):
            for c in range(cols):
                # Count ORIGINAL live neighbors
                live_neighbors = 0
                for dr, dc in dirs:
                    rr, cc = r + dr, c + dc
                    # Bounds check; only count cells that were originally live
                    if 0 <= rr < rows and 0 <= cc < cols and was_live(board[rr][cc]):
                        live_neighbors += 1

                cell = board[r][c]
                if cell == 1:
                    # Live survives ONLY with 2 or 3 neighbors; otherwise dies
                    if live_neighbors < 2 or live_neighbors > 3:
                        board[r][c] = -1  # mark live -> dead (so we still read it as originally live)
                else:  # cell == 0
                    # Dead becomes live ONLY with exactly 3 neighbors
                    if live_neighbors == 3:
                        board[r][c] = 2   # mark dead -> live (still read as originally dead)

        # ---------- Pass 2: normalize markers to final 0/1 ----------
        for r in range(rows):
            for c in range(cols):
                # Any positive value (1 or 2) becomes 1; non-positive (0 or -1) becomes 0
                board[r][c] = 1 if board[r][c] > 0 else 0
```

**What I’m watching for while coding:**

* “In Python, `-1` is truthy, so I never use `if board[rr][cc]:` to mean ‘live’; I always call `was_live`.
* I never normalize in the middle of Pass 1; I do it once, after the entire pass.”

#### JavaScript (same structure, clear intent)

```javascript
var gameOfLife = function(board) {
  if (!board || !board.length || !board[0].length) return;

  const rows = board.length;
  const cols = board[0].length;

  // 8 neighbor directions
  const dirs = [
    [-1,-1], [-1, 0], [-1, 1],
    [ 0,-1],          [ 0, 1],
    [ 1,-1], [ 1, 0], [ 1, 1],
  ];

  // Helper: originally-live check during Pass 1
  const wasLive = (v) => v === 1 || v === -1;

  // ---------- Pass 1: write markers ----------
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      let liveNeighbors = 0;

      // Count ORIGINAL live neighbors
      for (const [dr, dc] of dirs) {
        const rr = r + dr, cc = c + dc;
        if (rr >= 0 && rr < rows && cc >= 0 && cc < cols && wasLive(board[rr][cc])) {
          liveNeighbors++;
        }
      }

      const cell = board[r][c];
      if (cell === 1) {
        // Live survives only with 2 or 3 neighbors
        if (liveNeighbors < 2 || liveNeighbors > 3) {
          board[r][c] = -1; // mark live -> dead
        }
      } else { // 0
        // Dead becomes live with exactly 3 neighbors
        if (liveNeighbors === 3) {
          board[r][c] = 2;  // mark dead -> live
        }
      }
    }
  }

  // ---------- Pass 2: normalize ----------
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      board[r][c] = board[r][c] > 0 ? 1 : 0;
    }
  }
};
```

**Quick, spoken dry‑run on the sample:**
“Take cell `(1,1)` in the example; it’s `0`, but it has neighbors `(0,1)=1`, `(1,2)=1`, `(2,1)=1` → exactly 3 → mark `2` (dead→live). A live cell with 2 or 3 neighbors stays live; with fewer than 2 or more than 3, mark `-1`. After Pass 2, markers collapse to the expected matrix.”

---

### 5) Time and space complexity — crisp summary

* **Time:** `O(mn)` — each cell inspects ≤ 8 neighbors (constant factor).
* **Space:** `O(1)` extra — in‑place markers only.

---

### 6) Follow‑ups — show depth and adaptability

* **k generations:** “For moderate `k`, repeat the two‑pass procedure `k` times. For small boards and large `k`, cache states (hashing) to detect cycles. For infinite/sparse worlds, store only live cells in a set and use neighbor frequency maps.”
* **Bit‑packing alternative:** “Original in **bit0**, next in **bit1**; count neighbors with `x & 1`; set next with `x |= 2`; finalize with `x >>= 1`. Same asymptotics; avoids negative markers if that’s preferred.”
* **Testing strategy:** “I’d test all‑zeros, all‑ones, single row/col, oscillators like **blinker**, still lifes like **block**, a **glider**, and random boards to smoke‑test boundary logic.”
* **Why not copy?** “Copy is simplest but uses `O(mn)` space; both in‑place methods keep `O(1)`.”

---

### Bonus: interviewer‑friendly “pitfall checklist” (I say this out loud)

* “While counting, **1 and −1 are originally live**; **0 and 2 are originally dead**.”
* “Do **not** normalize during Pass 1; normalize once at the end.”
* “Bounds checks on every neighbor; never count the cell itself.”
* “In Python, don’t rely on `if cell:` — `-1` is truthy. Use explicit checks or the helper.”
* “For bit‑packing, always read with `& 1` and don’t forget the final `>>= 1`.”

---

### Optional 15‑second pitch if they prefer bitwise (I’m ready)

* “Identical control flow: count neighbors using `(board[r][c] & 1)`, compute next, set `board[r][c] |= 2` if next is live, then finalize with a single pass `>>= 1`. That preserves original state in bit0 while staging the next state in bit1.”

---

**Close‑out**
“That’s my solution path: confirm rules and simultaneity, call out edge cases, choose an in‑place technique, implement with two clean passes, and validate on known patterns. Happy to switch to the bitwise version or extend to `k` generations if you’d like.”

<br>

# 🎯 Real-World Applications｜實際應用場景



### 🧬 Biological Simulations｜生物模擬

* **EN:** The Game of Life is a simplified cellular automaton. In practice, similar rules are used in **biological and ecological simulations** to study population growth, extinction patterns, and spread of species.
* **ZH:** 生命遊戲是一種簡化的細胞自動機。在實際應用中，類似的規則會被用在 **生物與生態模擬**，例如研究族群增長、滅絕模式、或物種擴散的過程。

---

### 🎨 Computer Graphics & Image Processing｜電腦圖形與影像處理

* **EN:** Cellular automata are often used in **procedural texture generation, pattern creation, and image effects**. For example, simulating natural textures like fire, smoke, or terrain involves neighbor-based update rules.
* **ZH:** 細胞自動機常用於 **程序化材質生成、圖案設計與影像特效**。例如火焰、煙霧、地形等自然效果的模擬，就會用到鄰居規則來更新像素。

---

### 🕹️ Game Development｜遊戲開發

* **EN:** In games, similar grid-based simulations are applied to **pathfinding, map evolution, and procedural world generation**. For example, cave or dungeon generation often uses rules like “if a cell has too few neighbors, it becomes empty.”
* **ZH:** 在遊戲中，類似的網格模擬被應用於 **路徑尋找、地圖演化、程序化世界生成**。例如地牢或洞穴的生成規則，常常會用到「若某格鄰居過少就清空」這樣的方式。

---

### 🌍 Epidemiology & Spread Models｜流行病學與傳播模型

* **EN:** Neighbor-based update systems are used in **epidemic spread models** — for example, whether an infected person spreads disease to nearby individuals depends on local rules, similar to Game of Life.
* **ZH:** 以鄰居為基礎的更新系統可用於 **疫情傳播模型** —— 比如一個感染者是否會傳染給周圍的人，取決於局部規則，和生命遊戲的原理相似。

---

### 🧑‍💻 Parallel & Distributed Systems｜平行與分散式系統

* **EN:** Updating all cells simultaneously without interfering with each other mirrors **synchronization problems in distributed computing**. Engineers face similar issues when multiple processors update shared state simultaneously.
* **ZH:** 在 **分散式運算** 中，同步更新所有狀態而不互相干擾，和生命遊戲同樣的概念。工程師在多處理器同時更新共享資源時，也會遇到相似問題。

---

### 🧩 Algorithm Interview Insight｜演算法面試的意義

* **EN:** The key interview learning point is **“in-place state transitions with simultaneous update”**, which directly maps to real-world constraints like memory efficiency, cache friendliness, and safe concurrent updates.
* **ZH:** 這題在面試中的考點是 **「如何原地轉換狀態並同時更新」**，這對應到實際場景中的記憶體效率、快取友善設計，以及並發更新的正確性。

