
# 🧩 LeetCode 73 - Set Matrix Zeroes


#### 🌈 UMPIRE - ENGLISH VERSION

### 🔍 Understand

We are given a matrix of size `rows x cols`.
If any element is `0`, we must set its entire **row and column** to `0`, **in-place**.

**Constraints:**

* Do not return a new matrix
* Modify the original matrix directly
* Use as little extra space as possible (ideally **O(1)**)

---

### 🧠 Match

This is a classic **matrix in-place transformation** problem.

We can use:

* The **first row and first column** as markers to avoid extra space
* Two boolean variables to track whether the first row and column should be zeroed

---

### 📝 Plan

1. Check whether the **first row** or **first column** needs to be zeroed
2. Use the rest of the matrix to **mark** the rows and columns that need to be zeroed
3. Iterate again and **zero out cells** based on those markers
4. Finally, handle the **first row and first column**

---

### 🛠️ Implement

#### 🐍 Python (with rows/cols and detailed comments)

```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])  # Get matrix dimensions

    # Step 1: Check if the first row needs to be zeroed
    first_row_zero = False
    for col in range(cols):
        if matrix[0][col] == 0:
            first_row_zero = True
            break

    # Step 2: Check if the first column needs to be zeroed
    first_col_zero = False
    for row in range(rows):
        if matrix[row][0] == 0:
            first_col_zero = True
            break

    # Step 3: Use first row and column as markers
    for row in range(1, rows):
        for col in range(1, cols):
            if matrix[row][col] == 0:
                matrix[row][0] = 0  # Mark the row
                matrix[0][col] = 0  # Mark the column

    # Step 4: Zero cells based on markers (skip row 0 and col 0)
    for row in range(1, rows):
        for col in range(1, cols):
            if matrix[row][0] == 0 or matrix[0][col] == 0:
                matrix[row][col] = 0

    # Step 5: Zero the first row if needed
    if first_row_zero:
        for col in range(cols):
            matrix[0][col] = 0

    # Step 6: Zero the first column if needed
    if first_col_zero:
        for row in range(rows):
            matrix[row][0] = 0
```

---

#### 🌐 JavaScript (with rows/cols and detailed comments)

```javascript
var setZeroes = function(matrix) {
  const rows = matrix.length;
  const cols = matrix[0].length;

  // Step 1: Check if first row needs to be zeroed
  let firstRowZero = false;
  for (let col = 0; col < cols; col++) {
    if (matrix[0][col] === 0) {
      firstRowZero = true;
      break;
    }
  }

  // Step 2: Check if first column needs to be zeroed
  let firstColZero = false;
  for (let row = 0; row < rows; row++) {
    if (matrix[row][0] === 0) {
      firstColZero = true;
      break;
    }
  }

  // Step 3: Use matrix itself to mark zeros
  for (let row = 1; row < rows; row++) {
    for (let col = 1; col < cols; col++) {
      if (matrix[row][col] === 0) {
        matrix[row][0] = 0; // Mark row
        matrix[0][col] = 0; // Mark column
      }
    }
  }

  // Step 4: Apply markers to zero out the matrix
  for (let row = 1; row < rows; row++) {
    for (let col = 1; col < cols; col++) {
      if (matrix[row][0] === 0 || matrix[0][col] === 0) {
        matrix[row][col] = 0;
      }
    }
  }

  // Step 5: Zero out the first row if needed
  if (firstRowZero) {
    for (let col = 0; col < cols; col++) {
      matrix[0][col] = 0;
    }
  }

  // Step 6: Zero out the first column if needed
  if (firstColZero) {
    for (let row = 0; row < rows; row++) {
      matrix[row][0] = 0;
    }
  }
};
```

---

### 🔎 Review

* **Time Complexity:** O(rows × cols)
* **Space Complexity:** O(1) (constant extra variables only)

<br>

# 🌟 UMPIRE - 中文版本

### 🔍 理解（Understand）

你被給定一個 `rows x cols` 的矩陣，若其中某個元素為 `0`，需要將它所在的「整列」與「整欄」都設為 `0`。
要求**原地修改矩陣**，且最好只用 O(1) 的額外空間。

---

### 🧠 對應（Match）

這是一個經典的「矩陣原地修改」問題，解法要點：

* 利用**第 0 列與第 0 欄作為標記空間**
* 額外使用兩個布林變數紀錄第 0 列與第 0 欄是否需要清空

---

### 📝 計畫（Plan）

1. 檢查第 0 列與第 0 欄是否需要被設為全 0
2. 掃描整個矩陣，發現為 0 的格子時，把其所在行與列的第一格標記為 0
3. 根據這些標記來將該行該列其餘格子設為 0
4. 最後再處理第 0 列與第 0 欄

---

### 🛠️ 實作（請見英文區）

---

### 🔍 檢查（Review）

* **時間複雜度：** O(rows × cols)
* **空間複雜度：** O(1)，只用布林變數與原矩陣空間作標記

---

## 📝 附加筆記｜注意事項與常見錯誤

### ⚠️ 常見錯誤

| 錯誤                 | 原因                     |
| ------------------ | ---------------------- |
| 一邊掃描一邊修改           | 新產生的 0 會被當成標記使用，導致錯誤擴散 |
| 忘記記錄第 0 列與第 0 欄的狀態 | 最後清除會有誤，可能會丟失原始標記      |
| 修改順序錯誤             | 若提早清除第 0 列/欄，會破壞後續判斷條件 |

---

### ✅ 建議技巧

* **先檢查 row0 / col0**，**最後再清除**，確保標記不被覆蓋
* 使用語意清晰的變數名稱如 `rows`、`cols` 取代 `m`、`n`
* 寫題時**手動 trace** 一個範例矩陣，是非常重要的 debugging 練習

<br>


## 🎤 Full Spoken-Style Interview Answer



### 1. Clarify the problem and read the provided examples and constraints

> “Alright, let me first make sure I fully understand the problem.”

We’re given a 2D matrix of integers, and our task is: **if any cell in the matrix is 0, we need to set its entire row and column to 0**.

Importantly, we must **modify the matrix in-place**, meaning we can't return a new matrix—we have to make changes directly to the input. Ideally, we should also use **constant extra space**, so no extra 2D matrix.

For example, if the input is:

```
[
 [1, 1, 1],
 [1, 0, 1],
 [1, 1, 1]
]
```

Then after running the function, it should become:

```
[
 [1, 0, 1],
 [0, 0, 0],
 [1, 0, 1]
]
```

So we need to zero out **both the row and the column** that the 0 is in.

---

### 2. Discuss edge cases

> “Next, I want to think about any edge cases or tricky inputs.”

* What if the matrix is empty?
* What if the matrix has only one row or one column?
* What if the only 0 is at (0, 0)?
* What if **all values** are already 0?
* What if there are **no 0s** at all?

I’ll make sure my solution handles all of these properly, especially single-row/column scenarios and cases where the first row or column contains a zero.

---

### 3. Consider brute-force and optimal approach

> “Now I’ll talk about a few approaches and how I would improve them.”

The **brute-force** approach would be to loop through the entire matrix and whenever we find a 0, immediately set its entire row and column to 0. But that’s a problem—because if we modify the matrix while we’re scanning it, we might accidentally **overwrite original values** that we still need to check.

So, to avoid this, we can:

* First, scan the matrix and **record the rows and columns** that need to be zeroed.
* Then, do a second pass and zero out those rows and columns.

However, that requires **O(rows + cols)** additional space.

---

> “Let me now explain how to do it in **O(1) extra space**.”

The optimal solution uses the **first row and the first column of the matrix as marker space**.

Here's the idea:

* When we find a `0`, we mark the corresponding first row and first column cell as `0`.
* Then later, we use those markers to determine which rows and columns to zero out.
* Since the first row and column are being used as flags, we also need two booleans to remember if the **first row itself** or **first column itself** needs to be zeroed.

That way, we use **no extra space beyond two variables**, and we’re able to zero the matrix correctly.

---

### 4. Explain and implement optimal code

> “Now I’ll walk through the implementation step by step.”

```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])

    # Step 1: Check if the first row needs to be zeroed
    first_row_zero = False
    for col in range(cols):
        if matrix[0][col] == 0:
            first_row_zero = True
            break

    # Step 2: Check if the first column needs to be zeroed
    first_col_zero = False
    for row in range(rows):
        if matrix[row][0] == 0:
            first_col_zero = True
            break

    # Step 3: Use the first row and column to mark other rows/cols
    for row in range(1, rows):
        for col in range(1, cols):
            if matrix[row][col] == 0:
                matrix[row][0] = 0  # mark row
                matrix[0][col] = 0  # mark column

    # Step 4: Zero out based on markers (skip row 0 and col 0)
    for row in range(1, rows):
        for col in range(1, cols):
            if matrix[row][0] == 0 or matrix[0][col] == 0:
                matrix[row][col] = 0

    # Step 5: Zero out the first row if needed
    if first_row_zero:
        for col in range(cols):
            matrix[0][col] = 0

    # Step 6: Zero out the first column if needed
    if first_col_zero:
        for row in range(rows):
            matrix[row][0] = 0
```

> “So again, the first row and first column act like flags. We're reusing the matrix to store metadata, and only use two additional booleans.”

---

### 5. Discuss time and space complexity

> “Let’s now analyze time and space complexity.”

* **Time Complexity:** O(rows × cols)
  We scan the entire matrix a few times, but it’s still linear in terms of number of elements.

* **Space Complexity:** O(1)
  We’re only using a constant number of variables—two booleans, and we’re reusing the matrix for storage.

So this is an **optimal solution both in time and space**.

---

### 6. Mention follow-up questions

> “Here are a few follow-up questions I might consider or expect in an interview:”

* What if you’re **not allowed** to modify the input matrix?
  Then you would need to create a copy and work on that, which increases space complexity.

* Can you still do it with **less memory**, if we allow only some buffer?
  You might be allowed O(rows) or O(cols) if full O(1) isn’t required.

* How would this change if the matrix is **sparse** or streamed in row by row?

* What if instead of zeroes, we’re asked to mark them with a special value, like -1, and then apply another rule later?


<br>


## 🎯 Real-World Applications｜實際應用場景



### 🧬 Medical Imaging Cleanup｜醫療影像數據清洗

#### 💬 English

In medical imaging, large matrices often represent pixels of a scan (like MRI or CT images).
If a sensor fails and returns a 0 (indicating loss of data), the entire row and column might be considered corrupted. Setting those rows and columns to 0 helps mark them as invalid or removes their impact during analysis.

#### 📝 中文

在醫療影像（如 MRI 或 CT）中，圖像可以用矩陣來表示像素。
如果某個感測器失效，回傳了 0，就可能表示那一行或那一列的數據不可信。將該行與該列設為 0 可以視為**標記損壞區域**，避免干擾後續分析與模型判讀。

---

### 📊 Spreadsheet Error Propagation｜試算表錯誤傳播

#### 💬 English

In spreadsheets or grid-based tools (like Excel or Google Sheets), when a single formula cell fails (e.g., divide by zero), the system might mark that cell as invalid.
To prevent showing misleading results, the system can zero out the entire row and column connected to the error cell.

#### 📝 中文

在像 Excel、Google Sheets 這類試算表中，若某個格子出現錯誤（例如除以 0），系統可能會把那格標記為錯誤。
為了防止誤導使用者，整行或整列可能會一併被清空或隱藏，以便視覺上標示出錯誤影響區域。

---

### 🚚 Supply Chain Disruption Simulation｜供應鏈中斷模擬

#### 💬 English

In supply chain models, a matrix might represent the availability of products across warehouses and regions.
If a warehouse goes offline (represented by a 0 in the matrix), then its entire row and related product columns might be shut down or blocked.

#### 📝 中文

在供應鏈模擬中，矩陣可用來表示各地倉庫與產品的供應狀況。
若某個倉庫發生故障（用矩陣中的 0 表示），就會導致整個該倉的出貨中斷，也可能影響相關產品列的運作，這就對應到**整列整欄被設為 0** 的情境。

---

### 🔐 Access Control Failure Response｜權限控管錯誤防禦

#### 💬 English

In security systems, if one user or role has an access violation (represented as 0), the system may lock access to that user’s entire row (actions) and the violated resource’s column.

#### 📝 中文

在資安系統中，矩陣可用來表示使用者與資源之間的存取權限。
若某使用者違規（以 0 表示），系統可能會封鎖該使用者的所有操作（列）與該資源對應的所有權限（欄），達成**矩陣中整列整欄清除的效果**。

---

### 🧯 Data Masking in Analytics｜分析資料的遮罩處理

#### 💬 English

In data analytics, sensitive or outlier data may need to be masked. When a 0 is found (e.g., due to missing or flagged data), analysts may mask the entire row and column during preprocessing to avoid skewing model results.

#### 📝 中文

在資料分析中，若某筆資料因敏感性或缺失被標記為 0，為了避免對模型訓練產生偏差，可能會在資料清洗階段，**將該筆資料所在的整列與整欄都設為 0（遮罩處理）**。


<br>

