# 🧩 LeetCode 36 — Valid Sudoku｜UMPIRE (English)

### 🧠 Understand

* **Problem**: Given a 9×9 Sudoku board with digits `'1'`–`'9'` and `'.'` for empty, determine whether the board is **currently valid** (no rule violations). You **do not** need to solve it.
* **Rules**:

  1. Each **row** must not contain duplicate digits.
  2. Each **column** must not contain duplicate digits.
  3. Each **3×3 sub‑box** (9 total) must not contain duplicate digits.
* **Input/Output**: `board: List[List[str]]` (or `string[][]`), return `True/False`.
* **Constraints**: Always 9×9; characters are only `'1'`–`'9'` or `'.'`.

### 🔗 Match

* **Pattern**: Duplicate detection using **hash sets**.
* **Data structures**:

  * `rows[9]`: set for each row
  * `cols[9]`: set for each column
  * `boxes[9]`: set for each 3×3 box
* **Box index**: `box_id = (r // 3) * 3 + (c // 3)` maps `(r,c)` to an index `0..8`.

### 🗺️ Plan

1. Initialize three arrays of 9 sets: `rows`, `cols`, `boxes`.
2. For each cell `(r,c)`:

   * Skip if `board[r][c] == '.'`.
   * Let `v = board[r][c]` and compute `box_id`.
   * If `v` already in `rows[r]` or `cols[c]` or `boxes[box_id]` → **invalid** (return `False`).
   * Otherwise add `v` to all three sets.
3. If loop completes with no conflict → **valid** (return `True`).

### 🛠️ Implement

#### 🐍 Python

```python
from typing import List

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Create 9 independent sets for rows, columns, and 3x3 boxes
        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        # Traverse each cell of the 9x9 board
        for r in range(9):
            for c in range(9):
                val = board[r][c]

                # Ignore empty cells
                if val == '.':
                    continue

                # Compute which 3x3 box this (r,c) belongs to
                # Row-block index: r // 3 is 0,1,2 ; Col-block index: c // 3 is 0,1,2
                # Combine to a unique id in [0..8]
                box_id = (r // 3) * 3 + (c // 3)

                # If 'val' already appeared in this row/col/box, it's invalid
                if (val in rows[r]) or (val in cols[c]) or (val in boxes[box_id]):
                    return False

                # Record this digit as seen for the row, column, and box
                rows[r].add(val)
                cols[c].add(val)
                boxes[box_id].add(val)

        # No duplicates found across rows, cols, or boxes
        return True
```

#### 💻 JavaScript

```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
  // Prepare 9 independent sets for rows, columns, and boxes
  const rows = Array.from({ length: 9 }, () => new Set());
  const cols = Array.from({ length: 9 }, () => new Set());
  const boxes = Array.from({ length: 9 }, () => new Set());

  // Scan every cell
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      const val = board[r][c];

      // Skip empty cells
      if (val === '.') continue;

      // Compute 3x3 box id: 0..8
      const boxId = Math.floor(r / 3) * 3 + Math.floor(c / 3);

      // If duplicate in row/col/box -> invalid
      if (rows[r].has(val) || cols[c].has(val) || boxes[boxId].has(val)) {
        return false;
      }

      // Mark seen
      rows[r].add(val);
      cols[c].add(val);
      boxes[boxId].add(val);
    }
  }

  // No violation detected
  return true;
};
```

### 🔍 Review / Test

* **Dry run** a simple valid row: adding `'5'` then `'3'` to `rows[0]` works; encountering `'5'` again in the same row immediately triggers `False`.
* **Edge cases**:

  * Many `'.'` → still valid if no duplicates.
  * Duplicates inside a 3×3 box but not in same row/col → `boxes` set catches them.
  * Inputs outside `'1'`–`'9'` don’t occur per constraints.

### ⏳ Evaluate

* **Time**: Visit each of the 81 cells once → **O(9²)**, effectively **O(1)** constant for fixed 9×9.
* **Space**: At most 27 sets with ≤9 digits each → **O(1)**.

### 📦 Variants & Follow‑ups

* **Bitmasking**: Replace sets with 9 integers per rows/cols/boxes. For digit `d`, `mask = 1 << (d - 1)`. Check `(rows[r] & mask)`, etc. Same complexity, often faster in low‑level languages.
* **General N×N Sudoku**: For size `N` with sub‑boxes of size `√N × √N`, resize arrays and adjust the box‑index formula.

### ⚠️ Notes & Gotchas

* Always **skip `'.'`**.
* Ensure **independent sets**: use `[set() for _ in range(9)]` (Python) or `Array.from(..., () => new Set())` (JS); **do not** use `[set()] * 9`.
* **Box index** must be `(r // 3) * 3 + (c // 3)` (Python) or `Math.floor(r/3) * 3 + Math.floor(c/3)` (JS).
* Don’t mix row/column indices when indexing arrays.

<br>

# 🀄️ 數獨有效性（LeetCode 36）｜UMPIRE（中文）

### 📘 理解（Understand）

* **題意**：給定 9×9 數獨盤面，格子包含 `'1'`～`'9'` 或 `'.'`（空），判斷此盤面**目前是否有效**（是否違規），**不需要**求解整盤。
* **規則**：

  1. 每一\*\*列（row）\*\*不可重複相同數字。
  2. 每一\*\*行（column）\*\*不可重複相同數字。
  3. 每一個 **3×3 子方塊** 不可重複相同數字。
* **輸入/輸出**：`board: List[List[str]]`（或 `string[][]`），回傳 `True/False`。
* **限制**：尺寸固定 9×9；只會出現 `'1'`～`'9'` 與 `'.'`。

### 🧷 類比（Match）

* **解題型態**：使用 **集合（set）** 做「是否重複」檢查。
* **資料結構**：

  * `rows[9]`：每列一個集合，存該列已出現的數字
  * `cols[9]`：每行一個集合
  * `boxes[9]`：每個 3×3 子方塊一個集合
* **子方塊索引**：`box_id = (r // 3) * 3 + (c // 3)`，將 `(r,c)` 對應到 `0..8`。

### 🧭 計畫（Plan）

1. 建立三組各 9 個集合：`rows`、`cols`、`boxes`。
2. 對每個格子 `(r,c)`：

   * 若 `board[r][c] == '.'` 則跳過。
   * 令 `v = board[r][c]`，計算 `box_id`。
   * 若 `v` 已存在於 `rows[r]` 或 `cols[c]` 或 `boxes[box_id]` → 違規（回傳 `False`）。
   * 否則將 `v` 分別加入三個集合。
3. 走訪結束未偵測到衝突 → 合法（回傳 `True`）。

### 🧪 實作（Implement）

#### 🐼 Python

```python
from typing import List

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # 建立 9 個彼此獨立的集合（列 / 行 / 子方塊）
        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        # 逐格檢查
        for r in range(9):
            for c in range(9):
                val = board[r][c]

                # 空格不檢查
                if val == '.':
                    continue

                # 計算 3x3 子方塊編號（0..8）
                box_id = (r // 3) * 3 + (c // 3)

                # 若在同列 / 同行 / 同子方塊中已出現過，則違規
                if (val in rows[r]) or (val in cols[c]) or (val in boxes[box_id]):
                    return False

                # 登錄此數字已出現
                rows[r].add(val)
                cols[c].add(val)
                boxes[box_id].add(val)

        # 沒有任何衝突，盤面有效
        return True
```

#### 🧰 JavaScript

```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
  // 建立 9 個彼此獨立的 Set（列/行/子方塊）
  const rows = Array.from({ length: 9 }, () => new Set());
  const cols = Array.from({ length: 9 }, () => new Set());
  const boxes = Array.from({ length: 9 }, () => new Set());

  // 逐格檢查
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      const val = board[r][c];

      // 空格不檢查
      if (val === '.') continue;

      // 計算 3x3 子方塊索引（0..8）
      const boxId = Math.floor(r / 3) * 3 + Math.floor(c / 3);

      // 若同列/同行/同子方塊已出現過，則違規
      if (rows[r].has(val) || cols[c].has(val) || boxes[boxId].has(val)) {
        return false;
      }

      // 登記已出現
      rows[r].add(val);
      cols[c].add(val);
      boxes[boxId].add(val);
    }
  }

  // 無違規 → 有效
  return true;
};
```

### 🔎 檢查與測試（Review / Test）

* **心智演練**：如第 0 列遇到 `'5'` 與 `'3'`，加入 `rows[0]`；若稍後又在第 0 列遇到 `'5'`，立刻被 `rows[0]` 偵測到重複。
* **邊界情況**：

  * 很多 `'.'` 的稀疏盤，只要無重複仍為有效。
  * 若僅在同一子方塊重複、但列/行不同，`boxes` 也能偵測到。
  * 題目保證輸入只會是 `'.'` 或 `'1'`～`'9'`。

### 🧮 複雜度評估（Evaluate）

* **時間**：逐格一次 → **O(9²)**（固定大小可視為 **O(1)**）。
* **空間**：27 個集合，每個最多 9 個元素 → **O(1)**。

### 🧱 變體與延伸（Variants）

* **位元遮罩**：以整數位元表示是否出現過某數字（`mask = 1 << (d-1)`），以與/或運算檢查與標記。複雜度不變，常見於低階語言做優化。
* **一般化尺寸**：對於 N×N（√N×√N 子方塊）同理，只需調整陣列大小與子方塊索引計算。

### 📝 注意事項（Gotchas）

* 記得**跳過 `'.'`**。
* Python 請用 **`[set() for _ in range(9)]`**，避免 `[set()] * 9` 造成「多個元素指向同一集合」的陷阱。
* 子方塊索引要使用 **`(r // 3) * 3 + (c // 3)`**（或 JS 的 `Math.floor(r/3) * 3 + Math.floor(c/3)`）。
* 小心不要把 **row/col** 索引對調。
* 若你改成位元遮罩版本，確認**位移**與\*\*索引（`d-1`）\*\*無誤，避免 off‑by‑one。

<br>


# 🎤 Full Spoken-Style Interview Answer


### 1) Clarify the problem and read the provided examples and constraints

“Let me first restate the problem to ensure I understand it correctly.

We’re given a 9×9 Sudoku board. Each cell is either a digit `'1'` through `'9'` or a `'.'` meaning empty. I’m not required to solve the Sudoku—just to check whether the **current** board is valid so far according to Sudoku rules:

* No duplicate digits in any **row**.
* No duplicate digits in any **column**.
* No duplicate digits in any **3×3 sub-box**.

A few clarifications:

* `'.'` should be ignored; it doesn’t violate anything by itself.
* Validity is local to the existing numbers; even if the puzzle is unsolvable, that’s not our concern. We only care if a rule is already violated.

Constraints:

* The board is always 9×9.
* Characters are guaranteed to be `'1'`–`'9'` or `'.'`.

Quick mental example: if the top-left 3×3 box has two `'5'`s, even if they’re in different rows and columns globally, the board is invalid. Similarly, two `'7'`s in the same row or same column is invalid.”

---

### 2) Discuss edge cases

“Edge cases I’m thinking about:

* **Many empty cells**: A mostly empty board should still be valid unless a conflict exists.
* **Box-only conflicts**: Two same digits inside the same 3×3 box, but in different rows and columns—must still be caught.
* **Indexing mistakes**: Computing the 3×3 box index is a common source of bugs. I’ll use `boxId = (r // 3) * 3 + (c // 3)` to map any `(r, c)` to one of the 9 boxes (0..8).
* **Shared references**: In Python, I’ll avoid `[set()] * 9` (which would create 9 references to the same set) and instead use a list comprehension to create **independent** sets.
* **Ignoring '.'**: I’ll make sure I skip `'.'` entirely.”

---

### 3) Consider brute-force and optimal approach

“**Brute force #1 (validate after reading):**
I could check each row by scanning its 9 cells and ensuring no duplicates (using a temporary set). Then do the same for each column, and then for each of the nine 3×3 boxes. That’s perfectly fine for a fixed 9×9 board. Time is effectively constant (81 cells), space is small.

**Brute force #2 (cell-centric without sets):**
For each filled cell, scan its entire row/column/box linearly to see if the same number appears again. This also works but is a bit more repetitive and less clean.

**Optimal one-pass approach:**
I prefer a **single pass** with three arrays of sets:

* `rows[9]`, `cols[9]`, `boxes[9]`
  As I iterate through each cell `(r, c)`, if it’s a digit `v`, I compute `boxId`. If `v` is already in `rows[r]`, `cols[c]`, or `boxes[boxId]`, I can immediately return `False`. Otherwise, I insert `v` into all three. If I finish the loop without finding duplicates, return `True`.

This is clean, easy to reason about, and runs in O(1) time for a fixed 9×9 board with O(1) extra space.”

---

### 4) Explain and implement optimal code

#### Python (spoken walk-through + code)

“I’ll code the one-pass approach in Python. I’ll create three lists of sets for rows, columns, and boxes. I’ll iterate `r` and `c` from 0 to 8, skip `'.'`, compute `box_id`, check the three sets, and update them.”

```python
from typing import List

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Create 9 independent sets for rows, columns, and boxes.
        # Using a list comprehension guarantees each element is a distinct set.
        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        # Traverse every cell in the 9x9 board.
        for r in range(9):
            for c in range(9):
                val = board[r][c]

                # Ignore empty cells.
                if val == '.':
                    continue

                # Compute the 3x3 box ID.
                # r//3 yields 0,1,2 for row-blocks; c//3 yields 0,1,2 for column-blocks.
                # Multiplying row-block by 3 then adding column-block yields a unique ID in [0..8].
                box_id = (r // 3) * 3 + (c // 3)

                # If val already exists in the corresponding row, column, or box, it's invalid.
                if (val in rows[r]) or (val in cols[c]) or (val in boxes[box_id]):
                    return False

                # Otherwise, record the digit as seen in this row, column, and box.
                rows[r].add(val)
                cols[c].add(val)
                boxes[box_id].add(val)

        # If we finish scanning without conflicts, the board is valid so far.
        return True
```

#### JavaScript (spoken walk-through + code)

“Now I’ll mirror the same logic in JavaScript. I’ll use `Array.from(..., () => new Set())` to create independent sets.”

```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
  // Prepare 9 distinct sets for rows, columns, and 3x3 boxes.
  const rows = Array.from({ length: 9 }, () => new Set());
  const cols = Array.from({ length: 9 }, () => new Set());
  const boxes = Array.from({ length: 9 }, () => new Set());

  // Walk through every cell.
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      const val = board[r][c];

      // Skip empty cells.
      if (val === '.') continue;

      // Compute the box ID in [0..8].
      const boxId = Math.floor(r / 3) * 3 + Math.floor(c / 3);

      // If value already seen in this row/column/box => invalid board.
      if (rows[r].has(val) || cols[c].has(val) || boxes[boxId].has(val)) {
        return false;
      }

      // Record as seen.
      rows[r].add(val);
      cols[c].add(val);
      boxes[boxId].add(val);
    }
  }

  // No conflicts encountered: board is valid so far.
  return true;
};
```

---

### 5) Discuss time/space complexity

“Complexity-wise, we examine each of the 81 cells once—so **time is O(81)**, which is effectively **O(1)** for this fixed-size board. Space-wise, we maintain at most 27 sets (9 for rows, 9 for columns, 9 for boxes), each holding up to 9 digits, so **space is also O(1)**.”

---

### 6) Mention follow-up questions

“If there’s time, I’d ask or be ready for follow-ups like:

* **Bitmask optimization**: Replace sets with integers and use bit operations (`mask = 1 << (digit - 1)`) for constant-time checks and updates.
* **Generalization**: If the board were `N×N` with `√N×√N` sub-boxes, we can scale the same idea, just adjusting sizes and the box-index formula.
* **Diagnostics**: Modify the function to return which cell or which rule was violated (row/column/box and the digit).
* **Streaming updates**: If we receive a stream of cell updates, maintain the same three structures incrementally and validate each update in O(1).
* **Memory constraints**: Discuss trade-offs if we had to avoid sets (e.g., boolean arrays or bitmasks).”

---

### Notes / Gotchas I’d call out while coding

* Always **skip `'.'`**.
* In Python, use `[set() for _ in range(9)]` to avoid shared references from `[set()] * 9`.
* Double-check **box index**: `boxId = (r // 3) * 3 + (c // 3)`.
* Be careful not to mix up **row vs column** when indexing your structures.


<br>


## 🎯 Real-World Applications｜實際應用場景

### 🧾 Data Validation in Spreadsheets｜試算表中的資料驗證

**EN:** Similar to how we validate Sudoku rows, columns, and boxes, in enterprise spreadsheets (like Excel or Google Sheets), we often need to ensure that certain rows or columns contain unique values—such as unique IDs, product codes, or order numbers. The logic is basically: for each cell, check if the value has already appeared in the relevant group.
**中文：** 和檢查數獨的列、行、方塊相似，在企業的試算表（像 Excel 或 Google Sheets）中，常需要確保某些列或欄的值是唯一的，例如員工編號、產品代碼或訂單編號。邏輯就是：對每個儲存格檢查該值是否已在相應的群組中出現過。

---

### 🛂 User ID or Email Uniqueness Check｜使用者 ID 或電子郵件唯一性檢查

**EN:** In user registration systems, we must ensure that no two users share the same username or email. This is conceptually identical to ensuring no duplicates in a Sudoku row/column: we maintain a set of seen values and reject duplicates.
**中文：** 在使用者註冊系統中，我們必須確保沒有兩個使用者使用相同的帳號或電子郵件。這與檢查數獨列或行不重複的邏輯相同：維護一個已出現值的集合，並拒絕重複項。

---

### 🗄️ Database Integrity Constraints｜資料庫完整性約束

**EN:** SQL databases often enforce constraints like `UNIQUE` or composite unique keys. The Sudoku check is similar: each row/col/box represents a constraint group, and any duplicate violates integrity rules.
**中文：** SQL 資料庫經常會設定 `UNIQUE` 或複合唯一鍵的約束。數獨檢查的邏輯類似：每列/行/方塊就是一個約束群組，任何重複值都違反完整性規則。

---

### 🧪 Quality Control in Manufacturing｜製造業的品質檢查

**EN:** In manufacturing, each batch of products may require a unique serial number within a certain group or time slot. This is just like ensuring uniqueness in Sudoku sub-boxes.
**中文：** 在製造業中，每批產品可能需要在特定群組或時間段內的序號唯一。這就像確保數獨的子方塊中數字不重複一樣。

---

### 🎮 Game State Validation｜遊戲狀態驗證

**EN:** In game development, you might need to check that certain rules are not violated in the current game state—for example, no two chess pieces occupying the same square, or in a puzzle game, no duplicates in a row/column.
**中文：** 在遊戲開發中，可能需要檢查當前遊戲狀態是否違反規則，例如國際象棋中不能有兩個棋子佔據同一格，或益智遊戲中，列或行不能有重複元素。


