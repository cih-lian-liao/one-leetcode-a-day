

# 🧠 LeetCode 79 - Word Search (UMPIRE Method)

### 🇺 U — Understand the Problem

#### 🧩 English

We are given a 2D board of characters and a string word. We must determine whether the word exists in the board. The word can be constructed by sequentially adjacent cells (horizontally or vertically), and each letter cell may **not** be used more than once.

#### Example:

```
Input:
board = [["A","B","C","E"],
         ["S","F","C","S"],
         ["A","D","E","E"]]
word = "ABCCED"

Output: true
```

---

### 🧩 M — Match with a Known Pattern

This is a classic **Backtracking + DFS** problem on a grid.

* ✅ Backtracking is used because we need to "try a path," and if it doesn't work, we undo the move (backtrack) and try other directions.
* ✅ DFS is used to explore all directions recursively (up/down/left/right).
* ✅ We use a visited marker (e.g., setting cell to `#`) to avoid revisiting the same cell.

---

### 🗺️ P — Plan

1. Loop through each cell of the grid.
2. If a cell matches the first character of the word, start DFS.
3. During DFS:

   * If we match all characters, return `True`.
   * If out of bounds or mismatch or already visited, return `False`.
   * Temporarily mark the current cell as visited (e.g., with `#`).
   * Recursively search in all 4 directions.
   * Backtrack by restoring the original character.
4. If any path returns true, return true.
5. If no valid path found, return false.

---

### 💻 I — Implement

#### 🐍 Python (with detailed comments)

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        # Define the recursive DFS function
        def dfs(r, c, index):
            # If we have matched all characters in the word
            if index == len(word):
                return True
            # If out of bounds or cell mismatch or already visited
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != word[index]:
                return False

            # Temporarily mark the current cell as visited
            temp = board[r][c]
            board[r][c] = "#"

            # Explore all 4 directions
            found = (dfs(r + 1, c, index + 1) or
                     dfs(r - 1, c, index + 1) or
                     dfs(r, c + 1, index + 1) or
                     dfs(r, c - 1, index + 1))

            # Restore the cell's original value after exploration
            board[r][c] = temp
            return found

        # Try to start DFS from every cell
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:  # Start only if first character matches
                    if dfs(i, j, 0):
                        return True
        return False
```

---

#### 🌐 JavaScript (with detailed comments)

```javascript
var exist = function(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    // Recursive DFS function
    const dfs = (r, c, index) => {
        // If all characters matched
        if (index === word.length) return true;

        // Out of bounds or character mismatch or already visited
        if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] !== word[index]) {
            return false;
        }

        // Temporarily mark the cell as visited
        const temp = board[r][c];
        board[r][c] = '#';

        // Explore all 4 directions
        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        // Restore the cell
        board[r][c] = temp;
        return found;
    };

    // Try every cell as a starting point
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === word[0]) {
                if (dfs(i, j, 0)) return true;
            }
        }
    }
    return false;
};
```

---

### 🔍 R — Review

* ✅ Proper DFS and backtracking applied
* ✅ Handles visited cells correctly
* ✅ Checks all possible paths from all valid starting points
* ✅ Restores board state after each search

---

### 📊 E — Evaluate

* **Time Complexity:** O(M × N × 4^L)

  * M × N is the size of the board
  * L is the length of the word
* **Space Complexity:** O(L) for recursion stack

#
#
#

# 🧠 LeetCode 第 79 題 - 單詞搜尋 (UMPIRE 方法 中文版)

### 🇺 U — 理解題目

#### 🧩 中文說明

給你一個 `m x n` 的字母網格 `board` 和一個單詞 `word`。請你判斷這個單詞是否可以在網格中找到。

* 單詞必須透過「相鄰格子」組成（只能上下左右）。
* 同一格不能被重複使用。

#### 範例：

```
輸入:
board = [["A","B","C","E"],
         ["S","F","C","S"],
         ["A","D","E","E"]]
word = "ABCCED"

輸出: true
```

---

### 🧩 M — 類型比對

這題屬於典型的 **回溯法 + 深度優先搜尋（DFS）**

* ✅ 回溯法：探索一條路，如果錯了就退回來換條路。
* ✅ DFS：在網格上做遞迴式四向搜尋。
* ✅ 為了避免重複使用格子，我們會「暫時改變格子的值」或用 visited 陣列記錄。

---

### 🗺️ P — 解題計畫

1. 先遍歷整個網格，找出所有可能起點（與 `word[0]` 相同的格子）。
2. 對每個起點進行 DFS 遞迴：

   * 若成功比對整個單詞，回傳 `True`
   * 若越界、字母不符、已經訪問，則停止此路徑
3. 在 DFS 中暫時標記目前格子為 `#`，探索後恢復。
4. 若所有路徑都無法成功配對，回傳 `False`。

---

### 💻 I — 程式實作（含詳細註解）

#### 🐍 Python（含註解）

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        def dfs(r, c, index):
            if index == len(word):  # 所有字母都找到了
                return True
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != word[index]:
                return False  # 越界、字母不符、已走過

            temp = board[r][c]
            board[r][c] = "#"  # 標記已訪問

            # 往上下左右四個方向探索
            found = (dfs(r + 1, c, index + 1) or
                     dfs(r - 1, c, index + 1) or
                     dfs(r, c + 1, index + 1) or
                     dfs(r, c - 1, index + 1))

            board[r][c] = temp  # 回溯，恢復原本的字
            return found

        # 從所有可能起點進行 DFS
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    if dfs(i, j, 0):
                        return True
        return False
```

---

#### 🌐 JavaScript（含註解）

```javascript
var exist = function(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    const dfs = (r, c, index) => {
        if (index === word.length) return true; // 全部字母比對成功
        if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] !== word[index]) {
            return false; // 越界或字母不符
        }

        const temp = board[r][c];
        board[r][c] = '#'; // 標記已訪問

        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        board[r][c] = temp; // 回溯
        return found;
    };

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === word[0]) {
                if (dfs(i, j, 0)) return true;
            }
        }
    }
    return false;
};
```

---

### 🔍 R — 檢查重點

* ✅ 每次搜尋都有檢查邊界與字母是否吻合。
* ✅ 正確使用遞迴進行 DFS。
* ✅ 回溯時有恢復狀態。
* ✅ 從每個潛在起點都試過了。

---

### 📊 E — 效能評估

* **時間複雜度**：O(M × N × 4^L)，其中：

  * M × N 是格子總數
  * L 是 word 長度
* **空間複雜度**：O(L) 為遞迴堆疊深度

#
#
#

# 🎤 Full Spoken Interview-Style Explanation: LeetCode 79 – Word Search
### ✅ 1. Clarify the Problem

> "Let me clarify the problem first.
> We are given a 2D grid of characters and a string `word`.
> Our goal is to check whether we can construct this word from letters of sequentially adjacent cells, where adjacent means horizontally or vertically neighboring.

Each letter must be used only once in a given path.
The output should be `true` if the word exists in the board, otherwise `false`.

Is that correct?
Are there any constraints on the board size or the length of the word I should be aware of?"

---

### ⚠️ 2. Discuss Edge Cases

> "Here are some edge cases I'm thinking about:

* If the board is empty or the word is empty, we should return false.
* If the word is longer than the total number of cells in the board, then it's impossible to form the word, so we can return false early.
* If the board has multiple paths that start with the same character, we need to try all of them."

---

### 🧠 3. Consider Brute-force and Optimal Approach

> "Let me first consider a brute-force approach:
> We can start from every cell in the board and try to explore in all directions using a depth-first search. At each step, we check whether the current character matches the character in the word.

The key challenges are:

* Avoid revisiting the same cell.
* Backtrack correctly to explore alternative paths.

So instead of trying every possible permutation of paths, we can use a **recursive DFS with backtracking**, marking the current cell as visited, and then restoring it once we've explored that path.
This avoids using extra space for a separate visited array and keeps the solution elegant."

---

### 💻 4. Explain and Implement Optimal Code (Python)

> "Now I’ll walk through the optimal solution using DFS and backtracking in Python. I’ll explain as I code."

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        # This helper function will perform DFS from a given cell
        def dfs(r, c, index):
            # Base case: if all characters are matched
            if index == len(word):
                return True
            # If out of bounds or already visited or char mismatch
            if r < 0 or r >= rows or c < 0 or c >= cols or board[r][c] != word[index]:
                return False

            # Mark the current cell as visited by altering it
            temp = board[r][c]
            board[r][c] = "#"

            # Recursively search in all four directions
            found = (dfs(r + 1, c, index + 1) or
                     dfs(r - 1, c, index + 1) or
                     dfs(r, c + 1, index + 1) or
                     dfs(r, c - 1, index + 1))

            # Backtrack: restore the cell
            board[r][c] = temp
            return found

        # Try every cell as a starting point
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    if dfs(i, j, 0):
                        return True
        return False
```

> "So the idea is to try every cell that matches the first letter and initiate a DFS from there. If any path results in matching the full word, we return true."

---

### 📈 5. Discuss Time and Space Complexity

> "**Time complexity:**
> In the worst case, we start DFS from every cell, and from each cell, we explore up to 4 directions, and we do that for up to `L` characters in the word.
> So the time complexity is **O(M × N × 4^L)** where:

* M × N is the size of the board,
* L is the length of the word.

**Space complexity:**
We’re using the call stack for recursion, which can go as deep as `L`.
So space complexity is **O(L)**."

---

### 🧩 6. Mention Follow-up Questions

> "Some follow-up questions that might come up:

* How would you modify this if diagonal movements were allowed?
* What if the word can wrap around the board edges?
* Could we optimize further using memoization or pruning if there are repeated subproblems?
* What if we needed to find **all** occurrences of the word and return their starting coordinates?"

---

### 🗣️ Bonus: How to transition naturally

> "That’s my approach! Would you like me to also write the JavaScript version? Or explain how this could be optimized in large boards?"

#
#
#

# 🧠 LeetCode 79 – Word Search (Interview Spoken Explanation 中英對照筆記)

### 🔍 1. Clarify the Problem | 理解題目與釐清需求

#### 🗣️ English

Let me clarify the problem first.
We are given a 2D grid of characters and a string `word`.
Our goal is to check whether we can construct this word from letters of sequentially adjacent cells, where **adjacent means horizontally or vertically neighboring**.
Each letter cell may **not** be used more than once in a path.

Is that correct?
Are there any constraints on the board size or the length of the word I should be aware of?

#### 📘 中文

讓我先釐清題目。
我們會被給一個二維字母網格 `board`，還有一個字串 `word`。
我們的目標是確認這個單詞能否透過「相鄰的格子」拼湊出來。
這裡的「相鄰」指的是上下左右方向。
而每一個格子只能使用一次，不能重複。

這樣理解正確嗎？
請問在板子的大小或單詞長度上有什麼額外限制嗎？

---

### 🧩 2. Discuss Edge Cases | 邊界條件討論

#### 🗣️ English

Here are some edge cases I'm thinking about:

* If the board is empty or the word is empty, we should return false.
* If the word is longer than the total number of cells, it's impossible to match.
* If the board has repeated starting characters, we must explore **all** possible paths starting from those cells.

#### 📘 中文

以下是我想到的一些邊界條件：

* 如果 `board` 或 `word` 是空的，應該直接回傳 `false`。
* 如果 `word` 比所有格子的總數還要長，那就不可能拼出來。
* 如果 `board` 裡有多個符合起始字母的格子，我們要從每個地方都試一次。

---

### 🧠 3. Brute-force vs Optimal Approach | 暴力與最佳解法討論

#### 🗣️ English

Let me first consider a brute-force approach:
We can try to build the word from every starting cell, checking all possible sequences. But this is very inefficient.

So instead, we use a **recursive DFS with backtracking**.
From every starting point, we try matching each character in the word by searching up/down/left/right.
If a path doesn't work, we undo (backtrack) and try another one.
To avoid using a cell more than once, we temporarily mark it as visited (like changing the value to `#`), and restore it after backtracking.

#### 📘 中文

讓我先討論暴力法：
我們可以從每個格子出發，嘗試走出所有可能的路徑。但這樣效率非常低。

因此，我們使用 **DFS（深度優先搜索）加上回溯（backtracking）**。
從每個可能的起點出發，往上下左右四個方向遞迴尋找字母。
如果走不通，就回溯，換另一條路試試。
為了避免重複走同一格子，我們會「暫時把該格子設為特殊符號 `#`」，走完這條路後再還原。

---

### 💻 4. Implement with Explanation | 編碼與講解

#### 🐍 Python Version（含註解）

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        # DFS helper function
        def dfs(r, c, index):
            # If we've matched all letters, return True
            if index == len(word):
                return True
            # Out of bounds or mismatch or already visited
            if r < 0 or r >= rows or c < 0 or c >= cols or board[r][c] != word[index]:
                return False

            # Mark current cell as visited
            temp = board[r][c]
            board[r][c] = "#"

            # Explore 4 directions
            found = (dfs(r+1, c, index+1) or
                     dfs(r-1, c, index+1) or
                     dfs(r, c+1, index+1) or
                     dfs(r, c-1, index+1))

            # Restore original character (backtrack)
            board[r][c] = temp
            return found

        # Try every cell as a starting point
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    if dfs(i, j, 0):
                        return True
        return False
```

---

#### 🌐 JavaScript Version（含註解）

```javascript
var exist = function(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    const dfs = (r, c, index) => {
        // Base case: all letters matched
        if (index === word.length) return true;

        // Out of bounds or mismatch or already visited
        if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] !== word[index]) {
            return false;
        }

        // Mark the cell as visited
        const temp = board[r][c];
        board[r][c] = '#';

        // Explore 4 directions
        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        // Restore cell after search
        board[r][c] = temp;
        return found;
    };

    // Try every cell as the start
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === word[0] && dfs(i, j, 0)) {
                return true;
            }
        }
    }
    return false;
};
```

---

### ⏱️ 5. Time and Space Complexity | 時間與空間複雜度

#### 🗣️ English

**Time complexity:**
O(M × N × 4^L), where M × N is the board size, and L is the length of the word.
Each letter can go 4 directions, and we try every starting cell.

**Space complexity:**
O(L) due to recursion stack.

#### 📘 中文

**時間複雜度：**
O(M × N × 4^L)，其中 M × N 是整個網格的大小，L 是 word 的長度。
因為每次都可能往四個方向走，最多走 L 次。

**空間複雜度：**
O(L)，來自遞迴呼叫的堆疊深度。

---

### 💬 6. Follow-up Questions | 延伸問題討論

#### 🗣️ English

* What if diagonal moves were allowed?
* What if cells can be reused after a cooldown?
* What if we need to return all valid starting positions of the word?
* Could this be optimized for large boards (e.g., using a Trie if checking multiple words)?

#### 📘 中文

* 如果允許斜方向的移動怎麼辦？
* 如果每個格子可以多次使用但需要冷卻時間呢？
* 如果要找出所有可能的起點，而不是只問是否存在？
* 若我們要查很多單詞，是否可以使用 Trie 來加速？

#
#
#

# 🧠 **Spoken English Interview Explanation — LeetCode 79: Word Search**


### 1️⃣ **Clarify the problem**

> “Let me make sure I understand the problem correctly.
> We’re given a 2D board of characters and a word, and we need to determine if the word can be formed by sequentially adjacent letters on the board.
> Adjacency here means horizontally or vertically neighboring cells — no diagonals.
> Each letter cell may only be used **once per path**.

The function should return true if the word exists in the board, and false otherwise.

Does that match your understanding as well? Are there any constraints I should keep in mind, like maximum word length or board size?”

---

### 2️⃣ **Discuss edge cases**

> “Here are a few edge cases I’d like to consider before jumping into the solution:

* If either the board or the word is empty, we should return false.
* If the word’s length is longer than the total number of cells in the board, it’s impossible to match, so we can return false early.
* If there are multiple possible starting points in the grid (like multiple cells with the first character of the word), we need to try them all.
* Also, it's important to make sure we’re not revisiting the same cell during one path — that’s something to handle carefully.”

---

### 3️⃣ **Consider brute-force and optimal approach**

> “So, at a high level, a brute-force solution would be to try every possible path from every starting point on the board.
> This would be very inefficient because the number of possible paths grows exponentially.

Instead, we can take a more optimal approach using **DFS with backtracking**.
Here’s the idea:

* We’ll scan the board for every cell that matches the first character of the word.
* From there, we’ll recursively explore all 4 directions — up, down, left, and right.
* To avoid revisiting a cell, we’ll mark it as visited (for example, by temporarily replacing the character with a special symbol like `'#'`).
* If the current path doesn’t lead to a full match, we’ll backtrack — meaning we restore the character and try other directions.”

---

### 4️⃣ **Explain and implement optimal code**

#### 🐍 Python (spoken walkthrough)

> “Let me walk you through the Python implementation step by step, and explain as I code.”

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        # Define the recursive DFS helper
        def dfs(r, c, index):
            # Base case: if all characters matched
            if index == len(word):
                return True
            # Out of bounds, mismatch, or already visited
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != word[index]:
                return False

            # Mark current cell as visited by replacing its value
            temp = board[r][c]
            board[r][c] = "#"

            # Explore all 4 directions
            found = (dfs(r+1, c, index+1) or
                     dfs(r-1, c, index+1) or
                     dfs(r, c+1, index+1) or
                     dfs(r, c-1, index+1))

            # Backtrack — restore the character
            board[r][c] = temp
            return found

        # Try starting DFS from every cell that matches word[0]
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    if dfs(i, j, 0):
                        return True
        return False
```

> “So we check every cell. If we find the first character, we start the DFS from there.
> If any of those DFS paths return true, we return true immediately.
> If we try all cells and none of them work, we return false.”

#### 🌐 JavaScript (spoken walkthrough)

> “Now let me do a quick JavaScript version — it’s the same logic.”

```javascript
var exist = function(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    const dfs = (r, c, index) => {
        if (index === word.length) return true;
        if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] !== word[index]) {
            return false;
        }

        const temp = board[r][c];
        board[r][c] = '#'; // Mark visited

        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        board[r][c] = temp; // Backtrack
        return found;
    };

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === word[0] && dfs(i, j, 0)) {
                return true;
            }
        }
    }
    return false;
};
```

---

### 5️⃣ **Discuss time and space complexity**

> “Now let’s talk about the time and space complexity:

* **Time complexity** is O(M × N × 4^L) in the worst case, where:

  * M × N is the total number of cells,
  * L is the length of the word,
  * 4^L represents the branching factor for 4 directions at each character.

* **Space complexity** is O(L), where L is the depth of the recursion stack — one level for each character in the word.”

---

### 6️⃣ **Mention follow-up questions**

> “Here are some follow-up questions we might discuss:

* What if diagonal movement was allowed? That would increase our branching factor.
* What if we had to find and return **all** occurrences or paths of the word instead of just checking existence?
* Could we extend this solution to support searching for **multiple words**? In that case, we might consider using a Trie to optimize the lookups. (LeetCode No 212. Word Search II)
* If the board was extremely large, could we add caching or pruning logic to improve performance?”

#
#
#

### 🧐 1. Clarify the Problem | 理解題目

#### 🗣️ English

Let me make sure I understand the problem correctly.
We’re given a 2D board of characters and a string `word`.
We need to check if the word can be constructed by adjacent characters in the board, where adjacency means horizontally or vertically.
We cannot use the same cell more than once in a path.
The function should return `true` if the word exists, and `false` otherwise.

Are there any constraints on the board size or word length I should be aware of?

#### 📘 中文

讓我先確認我對題目的理解是否正確。
給定一個由字母組成的二維網格和一個單字，我們要確認這個單字是否可以透過「相鄰的格子」組成。
這裡的相鄰只包含上下左右（不包含斜角），而每個格子只能用一次。
如果這個單字存在於網格中，回傳 `true`，否則回傳 `false`。

請問有沒有什麼需要注意的限制條件，例如網格大小或字串長度？

---

### 🧱 2. Discuss Edge Cases | 討論邊界條件

#### 🗣️ English

Here are some edge cases I’d like to consider:

* If the board or the word is empty, we should return false.
* If the word is longer than the total number of cells in the board, then it’s impossible to find.
* If multiple cells contain the first letter, we need to try starting from each one.
* We must make sure not to revisit the same cell during a single path.

#### 📘 中文

以下是我想要先考慮的一些邊界情況：

* 如果 `board` 或 `word` 是空的，應該直接回傳 false。
* 如果單字長度比整個網格的格子總數還多，那根本不可能成立。
* 如果有很多格子都等於單字的第一個字母，我們必須從每個位置都試看看。
* 一條搜尋路徑中，不能重複使用同一格。

---

### 🧠 3. Brute-force and Optimal Approach | 暴力法與最佳解法

#### 🗣️ English

The brute-force approach would be trying to build the word from every cell and exploring all paths — this would be very inefficient.

Instead, we can use **DFS with backtracking**.
Here’s the plan:

* Start DFS from any cell that matches the first character.
* Explore in 4 directions (up, down, left, right).
* Temporarily mark a cell as visited using a special symbol like `#`.
* If we reach a dead end, backtrack by restoring the original letter.

#### 📘 中文

暴力法的話，就是從每一個格子出發，嘗試走出所有可能的路徑來組成單字，這樣的效率非常差。

我們可以改用 **DFS（深度優先搜尋）搭配回溯法（Backtracking）**。
具體做法是：

* 從所有等於單字第一個字母的格子開始 DFS。
* 每次探索四個方向（上、下、左、右）。
* 為了避免重複走相同格子，可以暫時將該格設為 `#` 作為標記。
* 若這條路走不通，就回溯並還原格子原本的內容。

---

### 💻 4. Implement the Optimal Code | 程式實作與說明

#### 🐍 Python Version

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        def dfs(r, c, index):
            # All characters matched
            if index == len(word):
                return True
            # Out of bounds or mismatch or visited
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != word[index]:
                return False

            temp = board[r][c]
            board[r][c] = "#"

            found = (dfs(r+1, c, index+1) or
                     dfs(r-1, c, index+1) or
                     dfs(r, c+1, index+1) or
                     dfs(r, c-1, index+1))

            board[r][c] = temp
            return found

        for i in range(rows):
            for j in range(cols):
                if board[i][j] == word[0]:
                    if dfs(i, j, 0):
                        return True
        return False
```

> 🗣️ “We try DFS from every possible starting cell. If the path works, we return true. If none of them match the word, we return false.”

---

#### 🌐 JavaScript Version

```javascript
var exist = function(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    const dfs = (r, c, index) => {
        if (index === word.length) return true;
        if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] !== word[index]) {
            return false;
        }

        const temp = board[r][c];
        board[r][c] = "#";

        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        board[r][c] = temp;
        return found;
    };

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === word[0] && dfs(i, j, 0)) {
                return true;
            }
        }
    }
    return false;
};
```

> 🗣️ “Same logic in JavaScript — start from any matching cell, explore recursively, and backtrack when needed.”

---

### ⏱️ 5. Time and Space Complexity | 時間與空間複雜度

#### 🗣️ English

**Time complexity:** O(M × N × 4^L)

* M × N is the number of cells in the board.
* L is the length of the word.
* 4^L comes from the fact that from each cell, we can explore up to 4 directions.

**Space complexity:** O(L) — due to the recursion stack.

#### 📘 中文

**時間複雜度：** O(M × N × 4^L)

* M × N 是網格的總格子數。
* L 是單字的長度。
* 因為每個字母可以往四個方向探索，所以總路徑可能是 4 的 L 次方。

**空間複雜度：** O(L)，來自遞迴的最大堆疊深度。

---

### 💡 6. Follow-up Questions | 延伸問題討論

#### 🗣️ English

* What if diagonal movement is allowed?
* What if we need to find and return all possible paths or starting coordinates?
* What if we’re given **a list of words** and asked to find all of them on the board?
  In that case, using a **Trie** would be more efficient.
* Can we optimize further by pruning impossible paths early?

#### 📘 中文

* 如果允許斜著走，該如何修改？
* 如果題目要求找出所有出現單字的路徑或起點？
* 如果不是一個單字，而是一個單字清單，該怎麼辦？
  這時候可以考慮用 **Trie（前綴樹）** 來提升效率。
* 是否可以加入剪枝策略來提早中止不可能成功的搜尋路徑？
