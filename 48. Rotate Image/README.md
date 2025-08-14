# 🌀 LeetCode 48 — Rotate Image (90° Clockwise)｜UMPIRE — English

### 🧠 Understand

* **Problem**: Given an `n × n` integer matrix, rotate it **90° clockwise in-place** (i.e., modify the input directly; don’t allocate another `n × n` matrix).
* **Input → Output** example:
  `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`
* **Constraints**:

  * Square matrix: `n == matrix.length == matrix[i].length`
  * Aim for **O(n²) time**, **O(1) extra space**
* **Clarify**: Values can be any integers; rotation is purely index mapping.

### 🧩 Match

* This is a **matrix transformation**. Two canonical **in-place** strategies:

  1. **Transpose** across the main diagonal, then **reverse each row** (simple & readable).
  2. **Layer-by-layer 4-way swap** (treat matrix as concentric rings).

> We’ll implement **Transpose + Reverse Rows**, which is the most interview-friendly.

### 🗺️ Plan

* **Key identity** for 90° clockwise: `(r, c) → (c, n−1−r)`.
* **How “transpose + reverse rows” achieves that**:

  * **Transpose** maps `(r, c) → (c, r)` (mirror over main diagonal).
  * **Reverse each row** maps `(c, r) → (c, n−1−r)`, which is exactly the rotation.
* **Steps**

  1. **Transpose**: For all `i < j`, swap `matrix[i][j]` with `matrix[j][i]`.
  2. **Reverse each row** in-place.

### 💻 Implement

#### 🐍 Python — Transpose + Reverse Rows (with detailed comments)

```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotate the n x n matrix by 90 degrees clockwise in-place.
        Do not return anything; modify the input matrix directly.

        Strategy:
        1) Transpose across the main diagonal
        2) Reverse each row
        Both steps are in-place, so extra space stays O(1).
        """
        n = len(matrix)

        # -------- Step 1: Transpose (mirror along the main diagonal) --------
        # Only swap the upper triangle (j > i) to avoid double-swapping and self-swaps.
        for i in range(n):
            for j in range(i + 1, n):  # j starts at i+1 to skip diagonal and below-diagonal
                # Swap (i, j) with (j, i)
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # -------- Step 2: Reverse each row in-place --------
        # Two-pointer reverse per row keeps space O(1).
        for i in range(n):
            left, right = 0, n - 1
            while left < right:
                matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
                left += 1
                right -= 1

        # NOTE:
        # - You could also do: for row in matrix: row.reverse()
        #   That is in-place too, but the two-pointer pattern is language-agnostic and explicit.
```

#### 🟨 JavaScript — Transpose + Reverse Rows (with detailed comments)

```javascript
/**
 * Rotate the n x n matrix by 90 degrees clockwise in-place.
 * @param {number[][]} matrix
 * @return {void}
 */
function rotate(matrix) {
  const n = matrix.length;

  // -------- Step 1: Transpose (mirror along the main diagonal) --------
  // Swap only when j > i to avoid double-swapping and self-swaps.
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      const tmp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = tmp;
      // ES6 alternative: [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // -------- Step 2: Reverse each row in-place --------
  // Using two pointers makes the in-place intent explicit across languages.
  for (let i = 0; i < n; i++) {
    let l = 0, r = n - 1;
    while (l < r) {
      const tmp = matrix[i][l];
      matrix[i][l] = matrix[i][r];
      matrix[i][r] = tmp;
      l++; r--;
    }
    // Practical note: matrix[i].reverse() also reverses in place in JS.
  }
}
```

### 🔁 Review (Walkthrough)

* Start:

  ```
  1 2 3
  4 5 6
  7 8 9
  ```
* After **transpose**:

  ```
  1 4 7
  2 5 8
  3 6 9
  ```
* After **reverse rows**:

  ```
  7 4 1
  8 5 2
  9 6 3
  ```
* Matches expected output.

### 📈 Evaluate

* **Time**: `O(n²)` — every element is touched a constant number of times.
* **Space**: `O(1)` — swaps in place; no extra `n × n` storage.
* **Edge cases**:

  * `n = 1` → unchanged.
  * `n = 2` → easy to hand-check.
  * Negative / duplicate values → irrelevant to index mapping.
* **Correctness sketch**: The composed mapping of transpose + row-reverse equals `(r, c) → (c, n−1−r)`.

### 🧾 Additional Notes & Pitfalls

* **Why `for (j = i + 1)` on transpose?**
  To avoid double-swapping and self-swaps; we only need the upper triangle.
* **Mutability matters**: Both Python lists and JS arrays are mutable; we must modify in-place as required by the problem.
* **Don’t allocate a new `n × n`**: That violates the in-place requirement (common interview gotcha).
* **Row reverse vs column reverse**: Clockwise uses **row reverse** after transpose; counterclockwise would need **column reverse** after transpose (or other equivalent sequences).
* **Testing tip**: Try `n=1`, `n=2`, a 4×4 with distinct values, and matrices containing negatives.

### 🧪 Quick Tests

* `[[1]]` → `[[1]]`
* `[[1,2],[3,4]]` → `[[3,1],[4,2]]`
* `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`

<br>

# 🌪️ LeetCode 48 — 旋轉圖像（順時針 90°）｜UMPIRE（中文）

### 🧐 理解

* **題目**：給定 `n × n` 整數方陣，將其**原地**旋轉 **90° 順時針**（直接修改輸入，不可建立另一個 `n × n` 矩陣）。
* **範例**：
  `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`
* **限制**：

  * 方陣：`n == matrix.length == matrix[i].length`
  * 期望 **時間 O(n²)**、**額外空間 O(1)**
* **澄清**：元素值無限制；旋轉本質是索引映射。

### 🧲 匹配

* 這是**矩陣變換**。兩種常見**原地**方法：

  1. **主對角線轉置**後，**反轉每一列**（簡潔、易讀、面試友善）。
  2. **分層四點循環交換**（把矩陣視為一圈一圈的「環」）。

> 本題實作採用 **轉置 + 反轉列**。

### 📝 計畫

* **核心映射**（順時針 90°）：`(r, c) → (c, n−1−r)`。
* **為何「轉置 + 反轉列」可達成**：

  * **轉置**：`(r, c) → (c, r)`（對主對角線鏡射）。
  * **反轉列**：`(c, r) → (c, n−1−r)`，正是所需結果。
* **步驟**

  1. **轉置**：對所有 `i < j`，交換 `matrix[i][j]` 與 `matrix[j][i]`。
  2. **反轉每一列**：以雙指標原地反轉。

### 🛠️ 實作

#### 🐼 Python（轉置 + 反轉列，含詳細註解）

```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        將 n x n 矩陣原地順時針旋轉 90 度。
        不回傳任何值，直接修改傳入的 matrix。

        策略：
        1) 轉置（主對角線鏡射）
        2) 反轉每一列
        兩步皆為原地操作，額外空間 O(1)。
        """
        n = len(matrix)

        # -------- 步驟一：轉置（只處理上三角，避免重複與自我交換） --------
        for i in range(n):
            for j in range(i + 1, n):  # j 從 i+1 起跳，跳過對角與下三角
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # -------- 步驟二：反轉每一列（雙指標原地反轉） --------
        for i in range(n):
            left, right = 0, n - 1
            while left < right:
                matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
                left += 1
                right -= 1

        # 備註：
        # - 也可寫成：for row in matrix: row.reverse()
        #   同為原地反轉；雙指標寫法較通用、語意更清楚。
```

#### 🟧 JavaScript（轉置 + 反轉列，含詳細註解）

```javascript
/**
 * 將 n x n 矩陣原地順時針旋轉 90 度。
 * @param {number[][]} matrix
 * @return {void}
 */
function rotate(matrix) {
  const n = matrix.length;

  // -------- 步驟一：轉置（只處理 j > i 的上三角） --------
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      const tmp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = tmp;
      // ES6 也可用解構交換：
      // [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // -------- 步驟二：反轉每一列（雙指標原地反轉） --------
  for (let i = 0; i < n; i++) {
    let l = 0, r = n - 1;
    while (l < r) {
      const tmp = matrix[i][l];
      matrix[i][l] = matrix[i][r];
      matrix[i][r] = tmp;
      l++; r--;
    }
    // 備註：matrix[i].reverse() 在 JS 中也是原地反轉。
  }
}
```

### 🔎 檢查（演練）

* 初始：

  ```
  1 2 3
  4 5 6
  7 8 9
  ```
* **轉置**後：

  ```
  1 4 7
  2 5 8
  3 6 9
  ```
* **反轉每列**後：

  ```
  7 4 1
  8 5 2
  9 6 3
  ```
* 與期望結果一致。

### 🧮 複盤

* **時間複雜度**：`O(n²)`（每個元素處理常數次）。
* **空間複雜度**：`O(1)`（全程原地交換）。
* **邊界情況**：

  * `n = 1` → 不變。
  * `n = 2` → 易於手算驗證。
  * 元素可重複/為負 → 不影響索引映射。
* **正確性**：轉置 + 反轉列組合映射即 `(r, c) → (c, n−1−r)`。

### 📌 附加筆記與注意事項

* **為何轉置時只跑 `j = i + 1`？**
  避免重複交換（把結果又換回去）與自我交換（對角元素），剛好涵蓋所有需要交換的上三角配對。
* **務必原地**：不可建立新的 `n × n`，否則違反題意（常見面試陷阱）。
* **API 與可讀性**：雖可用 `row.reverse()`，但雙指標更能清楚表達「原地反轉」邏輯，也較通用於沒有內建 `reverse()` 的場合。
* **順、逆時針差異**：

  * 順時針：**轉置 → 反轉列**。
  * 逆時針：**轉置 → 反轉欄**（或先反轉列再轉置等等效序列）。
* **測試建議**：`n=1`、`n=2`、`n=3` 經典案例、含負數與重複值之 4×4。

### 🧫 快速測試

* `[[1]]` → `[[1]]`
* `[[1,2],[3,4]]` → `[[3,1],[4,2]]`
* `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`

<br>

## 🎤 Full Spoken-Style Interview Answer

**Clarify the problem, read examples & constraints — spoken**

“Sure—let me restate the task in my own words. We’re given a **square** matrix, `n by n`. I need to rotate it **90 degrees clockwise**, and I must do it **in place**, which means I change the original matrix—no building a second `n by n` matrix.

For a quick example, if I start with:

```
1 2 3
4 5 6
7 8 9
```

after a 90° clockwise turn, it should look like:

```
7 4 1
8 5 2
9 6 3
```

The matrix is always square, and the values can be anything—positives, negatives, duplicates—that’s all fine. Time goal is around `O(n^2)`; extra space should be `O(1)`.”

---

**Edge cases — spoken**

“Edge cases I’m thinking about:

* When `n = 1`, nothing changes.
* For `n = 2`, it’s a nice quick hand-check.
* Repeated or negative numbers don’t matter since I’m only moving positions.
* For larger matrices, `O(n^2)` time is expected because we have to touch most cells anyway.”

---

**Brute force vs. optimal — spoken**

“A straightforward idea is to make a **new** matrix and write each value to its new spot, like `out[c][n-1-r] = in[r][c]`. That’s easy to write, but it uses `O(n^2)` extra space, which breaks the ‘in-place’ rule.

The usual **in-place** approaches are:

1. **Transpose, then reverse each row.**
   First I flip the matrix over its main diagonal (so `(r, c)` becomes `(c, r)`), then I reverse every row. Put together, that gives a 90° clockwise rotation. This version is short and clear.
2. **Layer-by-layer 4-way swap.**
   Think of the matrix as rings. For each ring, move top → right → bottom → left with a temp variable. Also works, just a bit more index-heavy.

I’ll do **transpose + reverse rows** because it’s clean and easy to talk through.”

---

**Explain & implement the optimal code — spoken while coding (Python first)**

“I’ll code it in Python and talk through it. Step one is the **transpose**. I’ll loop `i` from `0` to `n-1`, and `j` from `i+1` to `n-1`. Starting `j` at `i+1` makes sure I only swap each pair once and I don’t touch the diagonal. Step two is to **reverse each row** in place using two pointers.”

```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotate the n x n matrix 90 degrees clockwise, in place.
        """
        n = len(matrix)

        # Step 1: Transpose (swap across the main diagonal).
        # Only swap when j > i to avoid double-swapping and self-swaps.
        for i in range(n):
            for j in range(i + 1, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # Step 2: Reverse each row in place (two-pointer swap).
        for i in range(n):
            left, right = 0, n - 1
            while left < right:
                matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
                left  += 1
                right -= 1
```

“As a quick self-check, if I do this on the 3×3 example: after transpose I get

```
1 4 7
2 5 8
3 6 9
```

and after reversing each row I get

```
7 4 1
8 5 2
9 6 3
```

which is exactly what we want.”

---

**Same idea in JavaScript — spoken while coding**

“Now I’ll write the same thing in JavaScript. It’s the same two steps: transpose, then reverse each row.”

```javascript
/**
 * Rotate the n x n matrix 90 degrees clockwise, in place.
 * @param {number[][]} matrix
 * @return {void}
 */
function rotate(matrix) {
  const n = matrix.length;

  // Step 1: Transpose (swap across the main diagonal).
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      const tmp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = tmp;
      // ES6 swap also works:
      // [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // Step 2: Reverse each row in place (two pointers).
  for (let i = 0; i < n; i++) {
    let l = 0, r = n - 1;
    while (l < r) {
      const tmp = matrix[i][l];
      matrix[i][l] = matrix[i][r];
      matrix[i][r] = tmp;
      l++; r--;
    }
    // Note: matrix[i].reverse() is also in-place in JS if you prefer.
  }
}
```

---

**Time/space complexity — spoken**

“Runtime is `O(n^2)` overall. Transpose is about half the cells, then reversing rows hits each row once. Space is `O(1)`—just a couple of temp variables for swaps.”

---

**Follow-up questions — spoken**

“If you ask me to rotate **counterclockwise** 90°, I’d do transpose and then **reverse each column** instead of each row.
For **180°**, I could run the 90° rotation twice, or just reverse each row and then reverse the order of rows.
If the matrix isn’t square, this exact in-place trick doesn’t fit nicely—we’d usually return a new matrix, or use a more complex cycle approach.”

---

### Handy lines you can say (very plain, interview-friendly)

* “I’ll restate the goal to make sure I’ve got it: rotate the `n by n` matrix 90° clockwise, in place.”
* “I’ll go with a simple two-step in-place method: **transpose**, then **reverse each row**.”
* “During transpose, I only swap when `j > i` so I don’t undo my own work or touch the diagonal.”
* “Reversing each row in place with two pointers keeps the extra space at `O(1)`.”
* “Overall time is `O(n^2)` and space is `O(1)`. This is the standard clean solution.”
* “For counterclockwise, I’d transpose and reverse columns instead.”

<br>

# 🌀 LeetCode 48 — Rotate Image (90° Clockwise)｜Spoken-Style Notes (EN ⇄ ZH)

### 🧠 Understand｜題意理解

#### English

* **Task**: Rotate an `n × n` integer matrix **90° clockwise in place** (mutate the input; don’t create another `n × n`).
* **Given → Expected**:

  ```
  1 2 3      7 4 1
  4 5 6  →   8 5 2
  7 8 9      9 6 3
  ```
* **Constraints**: Square matrix; aim for **O(n²)** time and **O(1)** extra space.
* **What “in place” means**: Change the same `matrix` object; no new full-sized buffer.

#### 中文

* **任務**：將 `n × n` 的整數矩陣**原地**旋轉 **90° 順時針**（直接修改輸入；不可建立另一個 `n × n`）。
* **輸入 → 輸出**（如上例）。
* **限制**：方陣；目標 **時間 O(n²)**、**額外空間 O(1)**。
* **「原地」的意思**：必須在同一個 `matrix` 上完成，不能新建同尺寸矩陣。

---

### 🧪 Edge Cases｜邊界情況

#### English

* `n = 1` → no change.
* `n = 2` → quick hand check is easy.
* Duplicates / negatives are fine (we’re just moving positions).
* Large `n` still **O(n²)** since we must touch most cells anyway.

#### 中文

* `n = 1` → 不變。
* `n = 2` → 易於手算驗證。
* 重複值／負數不影響（只是索引映射）。
* 較大 `n` 也需 **O(n²)**，因為基本上要處理所有元素。

---

### 🧲 Approaches｜思路比較

#### English

* **Brute force (not in place)**: create a new matrix and write `out[c][n-1-r] = in[r][c]` → `O(n²)` space ❌.
* **In place (two classics)**:

  1. **Transpose + reverse each row** ✅

     * Transpose mirrors across the main diagonal: `(r, c) → (c, r)`.
     * Reverse each row: `(c, r) → (c, n−1−r)` → overall `(r, c) → (c, n−1−r)`.
     * Short, clear, interview-friendly.
  2. **Layer-by-layer 4-way swap** ✅

     * Treat the matrix as rings. Rotate four cells at a time with a temp.
     * Also `O(1)` extra space, but indexing is fussier.

#### 中文

* **暴力（非原地）**：新建矩陣 `out[c][n-1-r] = in[r][c]` → 額外空間 `O(n²)` ❌。
* **原地（兩種經典）**：

  1. **轉置 + 反轉每列** ✅

     * 轉置（主對角線鏡射）：`(r, c) → (c, r)`；
     * 反轉每列：`(c, r) → (c, n−1−r)` → 合成 `(r, c) → (c, n−1−r)`。
     * 簡潔清楚、面試友善。
  2. **分層四點交換** ✅

     * 把矩陣視為同心環；每次四點循環交換。
     * 亦為 `O(1)` 但索引較繁。

> We’ll implement **Transpose + Reverse Rows** below.
> 下方採用 **轉置 + 反轉每列**。

---

### 🛠️ Implementation Overview｜實作概觀

#### English

* **Step 1: Transpose** — for all `i < j`, swap `matrix[i][j]` with `matrix[j][i]`.
  Start inner loop at `j = i + 1` to avoid double-swaps and self-swaps.
* **Step 2: Reverse each row** — two pointers per row; swap toward the center.

#### 中文

* **步驟一：轉置** — 對所有 `i < j`，交換 `matrix[i][j]` 與 `matrix[j][i]`。
  內層從 `j = i + 1` 開始，避免重複交換與自我交換。
* **步驟二：反轉每列** — 每列用雙指標由外向內交換。

---

### 🐍 Python Implementation（Spoken-Style Comments）｜Python 實作（含口語化註解）

#### English

```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotate the n x n matrix 90 degrees clockwise, in place.
        """

        n = len(matrix)

        # Step 1: Transpose the matrix.
        # Idea: mirror across the main diagonal.
        # Only visit j > i so each pair is swapped once and we skip the diagonal.
        for i in range(n):
            for j in range(i + 1, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # Step 2: Reverse each row in place.
        # Two-pointer pattern: move left/right toward the center and swap.
        for i in range(n):
            left, right = 0, n - 1
            while left < right:
                matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
                left  += 1
                right -= 1

        # Note:
        # - In Python, you could also write: for row in matrix: row.reverse()
        #   which reverses in place. Two pointers keep the intent language-agnostic.
```

#### 中文

```python
from typing import List

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        將 n x n 矩陣原地旋轉 90 度（順時針）。
        """

        n = len(matrix)

        # 步驟一：轉置
        # 想法：對主對角線鏡射。
        # 只處理 j > i，確保每對只交換一次，且略過對角線。
        for i in range(n):
            for j in range(i + 1, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # 步驟二：反轉每一列（原地）
        # 雙指標：由外向內交換。
        for i in range(n):
            left, right = 0, n - 1
            while left < right:
                matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
                left  += 1
                right -= 1

        # 備註：
        # - 也可用 row.reverse() 原地反轉；雙指標寫法較通用、語意更清楚。
```

---

### 🧑‍💻 JavaScript Implementation（Spoken-Style Comments）｜JavaScript 實作（含口語化註解）

#### English

```javascript
/**
 * Rotate the n x n matrix 90 degrees clockwise, in place.
 * @param {number[][]} matrix
 * @return {void}
 */
function rotate(matrix) {
  const n = matrix.length;

  // Step 1: Transpose (mirror across the main diagonal).
  // Visit only j > i so we don't double-swap or touch the diagonal.
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      const tmp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = tmp;
      // ES6 swap also works:
      // [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // Step 2: Reverse each row in place (two-pointer).
  for (let i = 0; i < n; i++) {
    let l = 0, r = n - 1;
    while (l < r) {
      const tmp = matrix[i][l];
      matrix[i][l] = matrix[i][r];
      matrix[i][r] = tmp;
      l++; r--;
    }
    // Practical note: matrix[i].reverse() is also in-place in JS.
  }
}
```

#### 中文

```javascript
/**
 * 原地將 n x n 矩陣順時針旋轉 90 度。
 * @param {number[][]} matrix
 * @return {void}
 */
function rotate(matrix) {
  const n = matrix.length;

  // 步驟一：轉置（主對角線鏡射）。
  // 只處理 j > i，避免重複交換與自我交換（對角線）。
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      const tmp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = tmp;
      // 亦可用 ES6 解構交換：
      // [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // 步驟二：反轉每一列（雙指標原地反轉）。
  for (let i = 0; i < n; i++) {
    let l = 0, r = n - 1;
    while (l < r) {
      const tmp = matrix[i][l];
      matrix[i][l] = matrix[i][r];
      matrix[i][r] = tmp;
      l++; r--;
    }
    // 備註：matrix[i].reverse() 在 JS 中也是原地反轉。
  }
}
```

---

### 🔁 Walkthrough｜步驟演練

#### English

* Start:

  ```
  1 2 3
  4 5 6
  7 8 9
  ```
* After **transpose**:

  ```
  1 4 7
  2 5 8
  3 6 9
  ```
* After **reverse each row**:

  ```
  7 4 1
  8 5 2
  9 6 3
  ```

#### 中文

* 初始：

  ```
  1 2 3
  4 5 6
  7 8 9
  ```
* **轉置**後：

  ```
  1 4 7
  2 5 8
  3 6 9
  ```
* **反轉每列**後：

  ```
  7 4 1
  8 5 2
  9 6 3
  ```

---

### ⏱️ Complexity｜複雜度

#### English

* **Time**: `O(n²)` — transpose touches \~half the cells; reversing rows touches each element at most once more.
* **Space**: `O(1)` — in-place swaps with a few temporaries.

#### 中文

* **時間**：`O(n²)` — 轉置約處理一半元素；反轉每列再處理一次，每元素常數次。
* **空間**：`O(1)` — 僅少量暫存變數，原地交換。

---

### 🌱 Follow-ups｜延伸問題

#### English

* **90° counterclockwise**: transpose, then **reverse each column** (or other equivalent sequences).
* **180° rotation**: reverse each row, then reverse row order (or perform 90° twice).
* **Non-square matrices**: the neat in-place trick relies on `n × n`. For `m × n (m ≠ n)`, usually return a new matrix or apply a more complex cycle decomposition.

#### 中文

* **逆時針 90°**：轉置後**反轉每一欄**（亦可用其他等效序列）。
* **180°**：先反轉每列，再反轉整體列順序（或做兩次 90°）。
* **非方陣**：本原地技巧仰賴方陣；`m × n (m ≠ n)` 通常回傳新矩陣，或用更複雜的循環分解。

---

### 💬 Interview-Friendly Lines｜口語好背句

#### English

* “I’ll restate the goal: **rotate the `n by n` matrix 90° clockwise, in place**.”
* “I’ll use a **simple two-step** in-place method: **transpose**, then **reverse each row**.”
* “During transpose, I only swap when **`j > i`** so I don’t double-swap or touch the diagonal.”
* “Reversing each row **in place** with two pointers keeps extra space at **O(1)**.”
* “Overall time is **O(n²)** and space is **O(1)**. This is the standard clean solution.”

#### 中文

* 「我先複述目標：**將 `n × n` 矩陣原地順時針旋轉 90°**。」
* 「我用 **兩步** 原地法：**先轉置**、**再反轉每一列**。」
* 「轉置時只做 **`j > i`** 的交換，避免重複與對角線自我交換。」
* 「反轉每列用**雙指標原地**方式，額外空間維持 **O(1)**。」
* 「整體時間 **O(n²)**、空間 **O(1)**，是標準且乾淨的寫法。」

---

### 📌 Additional Notes & Pitfalls｜附加筆記與注意事項

#### English

* **Why `j = i + 1` during transpose?** It ensures we swap each `(i, j)`/`(j, i)` pair **once** and skip the diagonal.
* **Don’t allocate a new `n × n`**: it violates the in-place requirement (common interview gotcha).
* **API choices**: `row.reverse()` is in-place in Python and JS, but two-pointers make your intent explicit and portable.
* **Testing tips**: Try `n=1`, `n=2`, the classic `3×3`, and a `4×4` with negatives/duplicates.

#### 中文

* **為何轉置時用 `j = i + 1`？** 確保每一對 `(i, j)`/`(j, i)` 只交換一次，且跳過對角線。
* **別建立新的 `n × n`**：違反原地限制（面試常見陷阱）。
* **API 選擇**：Python/JS 的 `row.reverse()` 皆為原地；雙指標寫法更通用且語意清楚。
* **測試建議**：試 `n=1`、`n=2`、經典 `3×3`、以及含負數／重複值的 `4×4`。

---

### ✅ Quick Tests｜快速測試

#### English

* `[[1]]` → `[[1]]`
* `[[1,2],[3,4]]` → `[[3,1],[4,2]]`
* `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`

#### 中文

* `[[1]]` → `[[1]]`
* `[[1,2],[3,4]]` → `[[3,1],[4,2]]`
* `[[1,2,3],[4,5,6],[7,8,9]]` → `[[7,4,1],[8,5,2],[9,6,3]]`

<br>

# 🎯 Real-World Applications｜實際應用場景

### 🖼️ Image & Photo Processing｜影像與照片處理

**English** — Rotating bitmaps (icons, sprites, thumbnails) is common in editors, web image pipelines, or on-device filters. Doing it **in-place** matters when images are large or memory is tight (e.g., mobile, embedded). The “transpose + reverse rows” trick maps directly to reindexing pixel coordinates without allocating another full image.
**中文** — 在編輯器、網站影像管線或行動裝置濾鏡中，常需要旋轉點陣圖（icons、sprites、縮圖）。當影像很大或記憶體吃緊（如手機、嵌入式）時，**原地**旋轉特別重要；「**轉置 + 反轉列**」正好等價於不額外配置整張影像的像素重映射。

---

### 📱 Mobile Camera & EXIF Orientation｜手機相機與 EXIF 方向

**English** — Camera sensors often capture in one orientation and rely on **EXIF orientation**. Some pipelines must materialize the rotation (e.g., before upload or for apps that ignore EXIF). In-place rotation minimizes latency and memory spikes in processing pipelines.
**中文** — 相機感光元件常以固定方向輸出，再用 **EXIF 方向**標注。某些流程（上傳前或不支援 EXIF 的 App）需要真正把畫面旋轉；原地算法可降低延遲與額外記憶體峰值。

---

### 🎮 Games: Tile Maps & Sprites｜遊戲：方格地圖與精靈圖

**English** — 2D games store levels as `n×n` tile maps or sprite sheets. Rotations (90/180/270) help with **procedural generation**, level mirroring, and reusing assets. Doing it in-place avoids extra buffers during level loading.
**中文** — 2D 遊戲常用 `n×n` 方格地圖或精靈圖集。90/180/270 度旋轉可用於**關卡程序生成**、鏡像地圖與素材重用。原地進行可在載入關卡時避免額外緩衝區。

---

### 🧭 AR/VR Overlays & Mini-Maps｜擴增/虛擬實境疊圖與小地圖

**English** — HUDs or mini-maps rendered as grids sometimes need quick rotations to match the user’s heading. Using index transforms (transpose + reverse) keeps the cost `O(n²)` with `O(1)` space—useful on performance-sensitive headsets.
**中文** — 以網格呈現的 HUD 或小地圖，可能需根據使用者朝向快速旋轉。用索引映射（轉置 + 反轉）可在 **O(n²)** 時間、**O(1)** 空間達成，適合對效能敏感的頭戴式裝置。

---

### 🎥 Video Pipelines & Transcoding｜視訊管線與轉碼

**English** — Rotating frames (e.g., from portrait capture) appears in **transcoding**, **streaming**, and **story editors**. In-place operations reduce memory bandwidth and help avoid pipeline back-pressure when processing high-resolution frames.
**中文** — 在**轉碼**、**串流**與**短片編輯器**中，常需旋轉畫格（例如直拍來源）。原地處理可降低記憶體頻寬需求，避免高解析度處理時造成管線壅塞。

---

### 🤖 Robotics & Path Planning on Grids｜機器人學與格點路徑規劃

**English** — Occupancy grids, heatmaps, or cost maps are square matrices. When robots change reference frames or simulate turns, quickly rotating these matrices helps with **sensor fusion** and **map alignment** without heavy memory allocation.
**中文** — 佔用格、熱度圖、成本圖本質是方陣。當機器人切換參考座標或模擬轉向時，快速旋轉這些矩陣有助**感測器融合**與**地圖對齊**，且不需大量額外記憶體。

---

### 🏥 Medical & 🗺️ Geospatial Rasters｜醫療影像與地理柵格

**English** — MRI/CT slices (DICOM) and satellite rasters are dense 2D arrays. For **registration**, **normalization**, or **previews**, in-place rotation is a memory-safe step before heavier analysis.
**中文** — MRI/CT 影像（DICOM）與衛星柵格資料皆是高密度二維陣列。為了**配準**、**正規化**或**預覽**，原地旋轉可作為進一步分析前的節省記憶體步驟。

---

### 📐 UI Layout & Canvas Tools｜介面版面與畫布工具

**English** — Gridded dashboards, whiteboards, or canvas layers sometimes rotate nodes or layers (stickers, tiles). Implementing rotation via index mapping avoids temporary copies and keeps UI responsive.
**中文** — 以網格為基礎的儀表板、白板或多層畫布，常需旋轉元件（貼紙、區塊）。用索引映射實作旋轉，可避免暫存拷貝，保持 UI 流暢。

---

### ⚙️ Systems: Cache Locality & In-Place Patterns｜系統：快取區域性與原地範式

**English** — The transpose step is a classic **cache-locality** challenge; careful loop orders reduce cache misses. The “transpose + reverse” pattern generalizes to other **index-only** transforms (flips, 180°, CCW 90°), a common interview-to-industry transfer.
**中文** — 轉置是典型的**快取區域性**問題；良好的迴圈順序可減少 cache miss。此「轉置 + 反轉」範式也可推廣到其他**僅改索引**的變換（翻轉、180°、逆時針 90°），是面試技巧向實務遷移的常見範例。

---

### 🧪 How to Talk About It (Practice Lines)｜口語練習句

**English** — “In real systems, we rotate large 2D arrays—images, maps, or frames. Doing it **in place** saves memory and avoids pauses. The **transpose-then-reverse** trick is fast, easy to reason about, and generalizes to flips and 180°.”
**中文** — 「真實系統中我們常要旋轉大型二維陣列——影像、地圖或畫格。**原地**進行可省記憶體、避免卡頓；**先轉置再反轉**既快又好理解，並可延伸到翻轉與 180° 旋轉。」

---

### ✅ Quick Mapping Cheatsheet｜快速對照小抄

**English** —

* 90° CW: **transpose → reverse rows**
* 90° CCW: **transpose → reverse columns**
* 180°: **reverse rows → reverse row order** (or 90° twice)
  **中文** —
* 順時針 90°：**轉置 → 反轉列**
* 逆時針 90°：**轉置 → 反轉欄**
* 180°：**反轉每列 → 反轉整體列順序**（或做兩次 90°）


