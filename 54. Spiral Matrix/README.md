# 🌀 LeetCode 54 — Spiral Matrix | UMPIRE Notes


### 🏁 English UMPIRE

#### 🧭 Understand

* **Problem**: Given an `m × n` integer matrix, return **all elements in clockwise spiral order**.
* **Input**: 2D array `matrix` with `m` rows and `n` columns (possibly `m=0` or `n=0`).
* **Output**: 1D array of length `m × n`.
* **Spiral rule**: top row → right column → bottom row (right→left) → left column (bottom→top), then **shrink boundaries** and repeat.
* **Edge cases**: empty matrix, one row, one column, very rectangular matrices (e.g., `1×N`, `M×1`).

#### 🧩 Match

* Classic **boundary-shrinking** pattern with four pointers: `top`, `bottom`, `left`, `right`.
* Alternative is direction-simulation with visited set, but it uses extra `O(mn)` space. We’ll use the **boundary method** for clarity and optimal space.

#### 📝 Plan

Maintain:

* `top` = first unconsumed row, `bottom` = last unconsumed row
* `left` = first unconsumed col, `right` = last unconsumed col

Loop while `top ≤ bottom` **and** `left ≤ right`:

1. Traverse **top row** `left → right`; then `top += 1`.
2. Traverse **right col** `top → bottom`; then `right -= 1`.
3. If `top ≤ bottom`: traverse **bottom row** `right → left`; then `bottom -= 1`.
4. If `left ≤ right`: traverse **left col** `bottom → top`; then `left += 1`.

> The two **if** checks prevent duplicates when only one row/column remains.

#### 💻 Implement

##### 🐍 Python

```python
from typing import List

def spiralOrder(matrix: List[List[int]]) -> List[int]:
    """
    Return the elements of a matrix in clockwise spiral order.
    Boundary-shrinking approach with O(1) extra space (excluding output).
    """
    # ---- Guard: empty matrix or empty rows ----
    if not matrix or not matrix[0]:
        return []

    res: List[int] = []

    # ---- Initialize boundaries ----
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    # ---- Iterate until boundaries cross ----
    while top <= bottom and left <= right:
        # 1) Top row: left -> right
        for col in range(left, right + 1):
            res.append(matrix[top][col])
        top += 1  # shrink the top boundary (that row is consumed)

        # 2) Right column: top -> bottom
        for row in range(top, bottom + 1):
            res.append(matrix[row][right])
        right -= 1  # shrink the right boundary (that column is consumed)

        # 3) Bottom row: right -> left (only if rows remain)
        #    Use <= to include the last remaining row exactly once.
        if top <= bottom:
            for col in range(right, left - 1, -1):
                res.append(matrix[bottom][col])
            bottom -= 1  # shrink bottom after consuming it

        # 4) Left column: bottom -> top (only if cols remain)
        if left <= right:
            for row in range(bottom, top - 1, -1):
                res.append(matrix[row][left])
            left += 1  # shrink left after consuming it

    return res
```

##### 🧑‍💻 JavaScript

```javascript
/**
 * Return elements of matrix in clockwise spiral order.
 * Boundary-shrinking approach with O(1) extra space (excluding output).
 * @param {number[][]} matrix
 * @return {number[]}
 */
function spiralOrder(matrix) {
  // ---- Guard: empty matrix or empty rows ----
  if (!matrix || matrix.length === 0 || matrix[0].length === 0) return [];

  const res = [];

  // ---- Initialize boundaries ----
  let top = 0, bottom = matrix.length - 1;
  let left = 0, right = matrix[0].length - 1;

  // ---- Iterate until boundaries cross ----
  while (top <= bottom && left <= right) {
    // 1) Top row: left -> right
    for (let col = left; col <= right; col++) {
      res.push(matrix[top][col]);
    }
    top += 1; // shrink top boundary

    // 2) Right column: top -> bottom
    for (let row = top; row <= bottom; row++) {
      res.push(matrix[row][right]);
    }
    right -= 1; // shrink right boundary

    // 3) Bottom row: right -> left (only if rows remain)
    if (top <= bottom) {
      for (let col = right; col >= left; col--) {
        res.push(matrix[bottom][col]);
      }
      bottom -= 1; // shrink bottom boundary
    }

    // 4) Left column: bottom -> top (only if cols remain)
    if (left <= right) {
      for (let row = bottom; row >= top; row--) {
        res.push(matrix[row][left]);
      }
      left += 1; // shrink left boundary
    }
  }

  return res;
}
```

#### 🔍 Review (Dry Run)

Example:

```
[
  [ 1,  2,  3,  4],
  [ 5,  6,  7,  8],
  [ 9, 10, 11, 12]
]
→ [1,2,3,4,8,12,11,10,9,5,6,7]
```

Walk the outer ring, shrink, then walk the inner ring. Checks on steps (3) and (4) avoid duplication when only one row/column remains.

#### ⚖️ Evaluate (Complexity)

* **Time**: `O(mn)` — each element is visited exactly once.
* **Space**: `O(1)` auxiliary (not counting the `O(mn)` output array).

#### 🧪 Edge Cases & Tests

* `[]` → `[]`
* `[[1,2,3]]` → `[1,2,3]`
* `[[1],[2],[3]]` → `[1,2,3]`
* `[[1,2],[3,4]]` → `[1,2,4,3]`
* Highly rectangular `m×n` where `m ≫ n` or `n ≫ m`.

#### 🔁 Variants & Follow-ups

* **LC 59**: Generate spiral matrix `n×n` with values `1..n²` (same boundaries, but **write** instead of read).
* **Counterclockwise**: reorder the four traversals.
* **Start from arbitrary cell** or return the **k-th** visited element.

#### 🛠️ Extra Notes (English)

* **Why `≤` not `<`?** Using `top ≤ bottom` and `left ≤ right` ensures you **still process** the last remaining row/column. The extra `if` checks before bottom/left prevent double visiting after boundaries move.
* **Order matters**: top row → right col → bottom row → left col. Changing order breaks correctness.
* **Update after use**: increment/decrement boundaries **only after** consuming that edge.
* **Off-by-one**: inclusive ranges are `left..right` and `top..bottom`. Use `right + 1` and `left - 1` correctly in loops.
* **Empty inputs**: guard early.
* **Read-only**: do not mutate the matrix.

<br>

# 🐉 中文 UMPIRE

#### 🎯 理解題目

* **任務**：把 `m × n` 矩陣中的元素，依**順時針螺旋**順序輸出成一維陣列。
* **輸入**：二維陣列 `matrix`，可能為空或有空列。
* **輸出**：長度 `m × n` 的一維陣列。
* **螺旋規則**：上列 → 右欄 → 下列（由右到左）→ 左欄（由下到上），再**收縮邊界**重複。
* **邊界情況**：空矩陣、單列、單欄、極端長條形。

#### 🧠 模式匹配

* 採用**四邊界收縮法**：`top`、`bottom`、`left`、`right` 四個指標逐圈內縮。
* 方向模擬法（含 visited 標記）也可，但會多 `O(mn)` 空間；本題以邊界法更精煉。

#### 🗺️ 解題計畫

維護：

* `top`（尚未取用的第一列）、`bottom`（尚未取用的最後一列）
* `left`（尚未取用的第一欄）、`right`（尚未取用的最後一欄）

當 `top ≤ bottom` 且 `left ≤ right`：

1. 走 **上列** `left → right`，之後 `top += 1`
2. 走 **右欄** `top → bottom`，之後 `right -= 1`
3. 若 `top ≤ bottom`：走 **下列** `right → left`，之後 `bottom -= 1`
4. 若 `left ≤ right`：走 **左欄** `bottom → top`，之後 `left += 1`

> 第 (3)(4) 的條件判斷可避免單列/單欄時的**重複走訪**。

#### 🧰 程式實作

##### 🐼 Python 版本

```python
from typing import List

def spiralOrder(matrix: List[List[int]]) -> List[int]:
    """
    以順時針螺旋順序回傳矩陣元素。
    使用四邊界收縮；除輸出外只需 O(1) 額外空間。
    """
    # ---- 特判：空矩陣 / 空列 ----
    if not matrix or not matrix[0]:
        return []

    res: List[int] = []

    # ---- 初始化四個邊界 ----
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    # ---- 當邊界尚未交錯，持續繞圈 ----
    while top <= bottom and left <= right:
        # 1) 上列：left -> right
        for col in range(left, right + 1):
            res.append(matrix[top][col])
        top += 1  # 該列已取用，往內縮

        # 2) 右欄：top -> bottom
        for row in range(top, bottom + 1):
            res.append(matrix[row][right])
        right -= 1  # 該欄已取用，往內縮

        # 3) 下列：right -> left（仍有列時才走）
        if top <= bottom:
            for col in range(right, left - 1, -1):
                res.append(matrix[bottom][col])
            bottom -= 1  # 取用後再內縮

        # 4) 左欄：bottom -> top（仍有欄時才走）
        if left <= right:
            for row in range(bottom, top - 1, -1):
                res.append(matrix[row][left])
            left += 1  # 取用後再內縮

    return res
```

##### 🦊 JavaScript 版本

```javascript
/**
 * 以順時針螺旋順序輸出矩陣元素。
 * 邊界收縮，除輸出外為 O(1) 額外空間。
 * @param {number[][]} matrix
 * @return {number[]}
 */
function spiralOrder(matrix) {
  // ---- 特判：空矩陣 / 空列 ----
  if (!matrix || matrix.length === 0 || matrix[0].length === 0) return [];

  const res = [];

  // ---- 初始化四個邊界 ----
  let top = 0, bottom = matrix.length - 1;
  let left = 0, right = matrix[0].length - 1;

  // ---- 邊界未交錯，持續繞圈 ----
  while (top <= bottom && left <= right) {
    // 1) 上列：left -> right
    for (let col = left; col <= right; col++) {
      res.push(matrix[top][col]);
    }
    top += 1;

    // 2) 右欄：top -> bottom
    for (let row = top; row <= bottom; row++) {
      res.push(matrix[row][right]);
    }
    right -= 1;

    // 3) 下列：right -> left（仍有列時）
    if (top <= bottom) {
      for (let col = right; col >= left; col--) {
        res.push(matrix[bottom][col]);
      }
      bottom -= 1;
    }

    // 4) 左欄：bottom -> top（仍有欄時）
    if (left <= right) {
      for (let row = bottom; row >= top; row--) {
        res.push(matrix[row][left]);
      }
      left += 1;
    }
  }

  return res;
}
```

#### 🔎 乾跑檢查

以

```
[
  [ 1,  2,  3,  4],
  [ 5,  6,  7,  8],
  [ 9, 10, 11, 12]
]
```

為例 → `1,2,3,4,8,12,11,10,9,5,6,7`。
外圈走完，內縮再走內圈；在只剩單列/單欄時，(3)(4) 的條件避免重複。

#### 🧮 複雜度與易錯點

* **時間**：`O(mn)`（每格恰好一次）
* **空間**：`O(1)` 輔助（不含輸出）
* **易錯點**：

  * 忘了在 (3)(4) 前加 `if top <= bottom` / `if left <= right`，會重複或越界。
  * 只用 `<` 而非 `≤`，導致**最後一列/欄漏掉**。
  * 內縮順序錯誤或迴圈邊界 off-by-one。

#### 🧫 測試樣例

* `[]` → `[]`
* `[[1,2,3]]`、`[[1],[2],[3]]`
* `[[1,2],[3,4]]` → `[1,2,4,3]`
* 長條形與一般矩陣皆應覆蓋。

#### 🔄 變形題與延伸

* **LC 59**：產生螺旋填入矩陣（寫入版）。
* **逆時針**版本 / **任意起點** / 回傳**第 k 個**走訪元素。

#### 📌 附加筆記（中文）

* **為何用 `≤`？** 能確保**剩最後一列/欄**時仍會被處理；(3)(4) 的 `if` 再避免重複。
* **處理順序固定**：上列 → 右欄 → 下列 → 左欄；順序錯會破壞正確性。
* **邊界在「取用後」再內縮**，避免漏走或重複。
* **迴圈範圍含端點**：`left..right` 與 `top..bottom` 都是**含右端/下端**的區間。
* **空輸入**務必先行特判。
* **不修改原矩陣**，僅讀取組合輸出。

<br>

# 🎤 Full Spoken-Style Interview Answer

### 1) Clarify the problem, examples, and constraints

“Let me restate the problem to make sure I’ve got it right. We’re given an `m × n` integer matrix, and we need to return a flat list of all elements **in clockwise spiral order**. Spiral order means I start at the top‑left, sweep the top row left→right, then go down the rightmost column, then sweep the bottom row right→left, then go up the leftmost column, and keep shrinking the rectangle until I’ve visited every cell.

For examples:

* If `matrix = [[1,2,3],[4,5,6],[7,8,9]]`, the output should be `[1,2,3,6,9,8,7,4,5]`.
* If it’s a single row like `[[1,2,3]]`, the output is `[1,2,3]`.
* If it’s a single column like `[[1],[2],[3]]`, the output is `[1,2,3]`.
* If it’s empty (`[]` or `[[]]`), the output is `[]`.

Typical constraints are modest—enough that an `O(mn)` pass over the matrix is fine. There’s no need to mutate the matrix; we can just read values in order.”

### 2) Edge cases

“Edge cases I’ll keep in mind:

* **Empty matrix** or **empty first row** → return empty list.
* **One row** or **one column** → make sure I don’t skip it or duplicate values when boundaries meet.
* **Highly rectangular** shapes like `1×N`, `M×1`, `2×N`, `M×2` → ensure my boundary checks handle narrow strips.
* **Boundaries touching** (e.g., `top == bottom` or `left == right`) → I still need to process that last row or column exactly once.”

### 3) Brute-force vs. optimal approach

“Two natural approaches:

* **Direction simulation with `visited`**: keep a `visited` grid and a direction vector `[(0,1),(1,0),(0,-1),(-1,0)]`. I try to step forward; if the next cell is out of bounds or visited, I turn right. This is very straightforward, but it costs **O(mn)** extra space for `visited`.

* **Boundary‑shrinking (four pointers)**: Maintain `top`, `bottom`, `left`, `right`. Walk the top row, right column, bottom row, left column; then shrink the boundaries inward and repeat while `top ≤ bottom` and `left ≤ right`. This touches each cell once, uses **O(1)** extra space (besides the output), and is clean if I’m careful with conditions.

I’ll implement the **boundary‑shrinking** method because it’s optimal in space and very readable once the checks are right.”

### 4) Explain and implement the optimal code

“I’ll talk through the code as I write it.

* First, I’ll handle the empty matrix guard.
* I’ll initialize `top = 0`, `bottom = m - 1`, `left = 0`, `right = n - 1`.
* While `top ≤ bottom` and `left ≤ right`, I’ll do one ‘ring’:

  1. Traverse the **top row** from `left` to `right`; then `top += 1`.
  2. Traverse the **right column** from `top` to `bottom`; then `right -= 1`.
  3. If `top ≤ bottom`, traverse the **bottom row** from `right` to `left`; then `bottom -= 1`.
  4. If `left ≤ right`, traverse the **left column** from `bottom` to `top`; then `left += 1`.

Two details I’ll be explicit about:

* I use `≤` (not `<`) in the **while** and in the checks for steps (3) and (4). Using `≤` lets me process the last remaining row/column; the extra `if` guards avoid double‑visiting after boundaries move.
* I only shrink a boundary **after** I’ve consumed that edge.

Let me code it in Python first, then JavaScript.”

#### Python (spoken + commented)

```python
from typing import List

def spiralOrder(matrix: List[List[int]]) -> List[int]:
    """
    Return elements of an m x n matrix in clockwise spiral order.
    Boundary-shrinking approach; O(1) extra space, O(mn) time.
    """
    # 1) Guard for empty input: if matrix is [] or first row is []
    if not matrix or not matrix[0]:
        return []

    res: List[int] = []

    # 2) Initialize the four boundaries of the current "ring"
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    # 3) Keep walking rings while the remaining rectangle is valid
    while top <= bottom and left <= right:
        # ---- Top row: left -> right ----
        for col in range(left, right + 1):
            res.append(matrix[top][col])
        top += 1  # we've consumed the top edge, shrink it inward

        # ---- Right column: top -> bottom ----
        for row in range(top, bottom + 1):
            res.append(matrix[row][right])
        right -= 1  # consumed the right edge

        # ---- Bottom row: right -> left (only if rows remain) ----
        # Use <= so we include the last remaining row exactly once.
        if top <= bottom:
            for col in range(right, left - 1, -1):
                res.append(matrix[bottom][col])
            bottom -= 1  # consumed the bottom edge

        # ---- Left column: bottom -> top (only if cols remain) ----
        if left <= right:
            for row in range(bottom, top - 1, -1):
                res.append(matrix[row][left])
            left += 1  # consumed the left edge

    return res
```

#### JavaScript (spoken + commented)

```javascript
/**
 * Return elements of matrix in clockwise spiral order.
 * Boundary-shrinking approach; O(1) extra space, O(mn) time.
 * @param {number[][]} matrix
 * @return {number[]}
 */
function spiralOrder(matrix) {
  // 1) Guard for empty input
  if (!matrix || matrix.length === 0 || matrix[0].length === 0) return [];

  const res = [];

  // 2) Initialize the four boundaries
  let top = 0, bottom = matrix.length - 1;
  let left = 0, right = matrix[0].length - 1;

  // 3) Walk rings while rectangle is valid
  while (top <= bottom && left <= right) {
    // ---- Top row: left -> right ----
    for (let col = left; col <= right; col++) {
      res.push(matrix[top][col]);
    }
    top += 1; // shrink top boundary

    // ---- Right column: top -> bottom ----
    for (let row = top; row <= bottom; row++) {
      res.push(matrix[row][right]);
    }
    right -= 1; // shrink right boundary

    // ---- Bottom row: right -> left (if rows remain) ----
    if (top <= bottom) {
      for (let col = right; col >= left; col--) {
        res.push(matrix[bottom][col]);
      }
      bottom -= 1; // shrink bottom boundary
    }

    // ---- Left column: bottom -> top (if cols remain) ----
    if (left <= right) {
      for (let row = bottom; row >= top; row--) {
        res.push(matrix[row][left]);
      }
      left += 1; // shrink left boundary
    }
  }

  return res;
}
```

**Quick sanity dry‑run (spoken):**
“Take `[[1,2,3,4],[5,6,7,8],[9,10,11,12]]`.
Top row → `1,2,3,4`; right col → `8,12`; bottom row (rev) → `11,10,9`; left col (up) → `5`.
Shrink; next ring: top row → `6,7`. Done.
So we get `[1,2,3,4,8,12,11,10,9,5,6,7]`.”

**Why `≤`?**
“I keep `while (top <= bottom && left <= right)` so a single remaining row or column is still processed. The `if (top <= bottom)` and `if (left <= right)` checks before steps (3) and (4) make sure I don’t double‑visit after boundaries move.”

### 5) Time and space complexity

“Time is **O(mn)** because each element is appended exactly once.
Extra space is **O(1)** beyond the output list/array, since I’m just tracking four integers.”

### 6) Follow‑up questions I’d ask or anticipate

* “Would you like a **counter‑clockwise** version? That’s just changing the traversal order.”
* “If we needed to **generate** an `n × n` spiral matrix filled with `1..n²` (LeetCode 59), we can use the same boundary logic but **write** values instead of reading them.”
* “Do we ever need to start from a different corner or from the **center**? The boundary idea still applies with minor adjustments.”
* “If the matrix were extremely large or streamed, we’d discuss memory constraints and whether we can process in strips or require random access.”

**(Optional)** Small correctness note I’d highlight if asked:
“The two places candidates often slip are using `<` instead of `≤` and forgetting the guard checks before traversing the bottom row and left column. Using `≤` ensures the last row/column gets visited; the guards prevent duplicates when boundaries cross after shrinking.”

<br>

## 🎯 Real-World Applications｜實際應用場景


### 🗺️ Path Planning in Robotics｜機器人路徑規劃

**EN**
Robots, especially cleaning robots (like Roomba), sometimes follow a spiral path to cover an area efficiently. The spiral ensures all cells are visited without revisiting the same spot too early, which is similar to the matrix spiral traversal.
**ZH**
機器人（尤其是掃地機器人，例如 Roomba）有時會採用螺旋路徑來高效覆蓋區域。螺旋確保所有位置都被走訪，且避免過早重複走訪，這與矩陣的螺旋遍歷非常相似。

---

### 🖼️ Image Processing and Pixel Access｜影像處理與像素存取

**EN**
In image processing, a spiral scan can be used to process pixels starting from the center or from a corner, for effects like radial blur, ripple patterns, or detecting objects radiating outward.
**ZH**
在影像處理中，螺旋掃描可用於從中心或角落開始處理像素，例如製作放射模糊、漣漪效果，或偵測由內向外擴散的物體。

---

### 🗄️ Data Storage and Retrieval in 2D Arrays｜二維陣列資料儲存與檢索

**EN**
Certain storage formats or compression algorithms store or retrieve data in spiral order to optimize for specific access patterns, for example when streaming progressive image data or generating fractal-based data layouts.
**ZH**
部分儲存格式或壓縮演算法會以螺旋順序儲存或讀取資料，以優化特定的存取模式，例如串流漸進式影像資料或產生基於分形的資料佈局。

---

### 🕹️ Game Development – Map Exploration｜遊戲開發中的地圖探索

**EN**
In some games, a spiral search is used for revealing the map outward from a starting point, e.g., uncovering tiles in a strategy game when a player moves into unexplored territory.
**ZH**
在某些遊戲中，會用螺旋搜尋方式從起點向外揭開地圖，例如策略遊戲中玩家移動到未探索區域時，一圈圈揭示地圖方格。

---

### 📊 Visualization of Data in Spiral Layouts｜資料螺旋佈局視覺化

**EN**
Spiral layouts are sometimes used in data visualization to compactly arrange time series, logs, or events in a circular fashion for pattern recognition. The traversal logic is similar to a spiral matrix.
**ZH**
在資料視覺化中，有時會採用螺旋佈局將時間序列、日誌或事件以圓形方式緊湊排列，便於辨識模式。其遍歷邏輯與螺旋矩陣相似。
