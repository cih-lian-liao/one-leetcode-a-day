# 🧭 UMPIRE Notes (English Version)

### 🎯 Understand

We are given a string `s` that contains words separated by spaces (some of which may be extra spaces at the start, end, or between words).
The goal is to reverse the **order of the words**, remove leading and trailing spaces, and reduce **multiple spaces to single spaces** between words.

**Example:**

```
Input:  "  hello   world  "
Output: "world hello"
```

---

### 🧩 Match

This is a **string manipulation** problem.

We can solve this using:

* **Two Pointers** to reverse words manually
* **Manual trimming** and **splitting**
* Avoiding built-in `split()` or `reverse()` for algorithmic rigor

---

### 🛠️ Plan

1. Initialize a pointer `i` from the end of the string.
2. Skip trailing spaces.
3. Identify each word by scanning backwards using two pointers: `i` (start) and `j` (end).
4. Extract the word and store it in a list.
5. After processing all words, join them with a single space.

---

### 💻 Implement

#### ✅ Python (with comments)

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        res = []  # Store the words in reversed order
        i = len(s) - 1  # Start from the end of the string

        while i >= 0:
            # Step 1: Skip any trailing or extra spaces
            while i >= 0 and s[i] == ' ':
                i -= 1
            if i < 0:
                break

            # Step 2: Mark the end of the current word
            j = i

            # Step 3: Move left to find the start of the word
            while i >= 0 and s[i] != ' ':
                i -= 1

            # Step 4: Append the word to result list
            res.append(s[i + 1:j + 1])

        # Step 5: Join words with a single space
        return ' '.join(res)
```

---

#### ✅ JavaScript (with comments)

```javascript
var reverseWords = function(s) {
    const res = [];
    let i = s.length - 1;

    while (i >= 0) {
        // Step 1: Skip spaces from the end
        while (i >= 0 && s[i] === ' ') i--;
        if (i < 0) break;

        // Step 2: Set j to current i (end of the word)
        let j = i;

        // Step 3: Move left to find the beginning of the word
        while (i >= 0 && s[i] !== ' ') i--;

        // Step 4: Extract word and add to result
        res.push(s.slice(i + 1, j + 1));
    }

    // Step 5: Return the joined words
    return res.join(' ');
};
```

---

### 🧪 Review

**Test Case:**

```text
Input:  "  the sky   is blue  "
Output: "blue is sky the"
```

Edge Cases:

* Empty string
* Only spaces
* Single word

---

### 📊 Evaluate

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(n)  |

#
#
#

# 📝 UMPIRE 中文版筆記


### 🎯 理解題目

你會得到一個字串 `s`，其中包含許多單字與空格（可能有前後空格，也可能有多個連續空格）。
你的任務是：

1. 反轉單字的順序
2. 移除開頭與結尾空格
3. 單字之間僅保留一個空格

**範例：**

```
輸入:  "  hello   world  "
輸出: "world hello"
```

---

### 🧩 類比問題類型

這是一題典型的**字串處理 + 雙指標 Two Pointers**問題，會考察：

* 如何自己實作 split + trim 的效果
* 如何反轉單字順序
* 如何手動處理多餘空格

---

### 🛠️ 解題計畫

1. 從字串尾端往前掃描，用變數 `i`。
2. 略過結尾空格。
3. 記錄單字的末尾與開頭（`j` 到 `i`）。
4. 擷取單字，加到結果陣列。
5. 最後用 `' '.join()` 或 `.join(' ')` 連接單字。

---

### 💻 程式實作

#### ✅ Python 解法（含註解）

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        res = []  # 用來儲存反轉後的單字
        i = len(s) - 1  # 指標從最後一個字元開始

        while i >= 0:
            # Step 1: 跳過空格
            while i >= 0 and s[i] == ' ':
                i -= 1
            if i < 0:
                break

            # Step 2: 找到單字的尾端
            j = i

            # Step 3: 繼續往左走直到遇到空格
            while i >= 0 and s[i] != ' ':
                i -= 1

            # Step 4: 擷取單字並加入結果陣列
            res.append(s[i + 1:j + 1])

        # Step 5: 用空格把單字串起來
        return ' '.join(res)
```

---

#### ✅ JavaScript 解法（含註解）

```javascript
var reverseWords = function(s) {
    const res = [];
    let i = s.length - 1;

    while (i >= 0) {
        // Step 1: 從尾端跳過空格
        while (i >= 0 && s[i] === ' ') i--;
        if (i < 0) break;

        // Step 2: 紀錄單字尾端
        let j = i;

        // Step 3: 往左移動直到遇到空格
        while (i >= 0 && s[i] !== ' ') i--;

        // Step 4: 擷取單字並加入結果陣列
        res.push(s.slice(i + 1, j + 1));
    }

    // Step 5: 將單字用空格串接
    return res.join(' ');
};
```

---

### 🧪 複習測試案例

```text
輸入:  "  the sky   is blue  "
輸出: "blue is sky the"
```

#### 邊界情況：

* 空字串 → 輸出為空字串
* 全部都是空格 → 輸出為空字串
* 只有一個單字 → 輸出相同

---

### 📊 評估複雜度

| 項目    | 複雜度            |
| ----- | -------------- |
| 時間複雜度 | O(n)（掃過整個字串一次） |
| 空間複雜度 | O(n)（儲存單字）     |

#
#
#

### 使用fuctions的 Python 解法：

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # Step 1: Trim spaces
        trimmed = s.strip()

        # Step 2: Split into words (handles multiple spaces)
        words = trimmed.split()

        # Step 3: Reverse the list of words
        reversed_words = words[::-1]

        # Step 4: Join with a single space
        return ' '.join(reversed_words)
```

🔍 **說明**：

* `.strip()`：去除前後空格
* `.split()`：分割單詞並自動忽略多餘空格
* `[::-1]`：反轉陣列
* `' '.join()`：用單一空格拼接單詞

#
#
#


# 🎤 Full Spoken-Style Interview Answer


### 1️⃣ Clarify the Problem

Sure! So, we are given a string `s` that contains words separated by spaces. Some of the spaces might be extra — at the beginning, at the end, or between words.
Our goal is to **reverse the order of the words**, remove **leading and trailing spaces**, and ensure that there's only **one space between words** in the output.

Let me give an example to confirm my understanding:
If the input is: `"  hello   world  "`,
the output should be: `"world hello"`.

We are **not reversing the characters** inside the words, just the word **order**.

Is that correct?

---

### 2️⃣ Discuss Edge Cases

Here are some edge cases I'm considering:

* An empty string → should return an empty string.
* A string with only spaces → return an empty string.
* A string with a single word, possibly with leading or trailing spaces → return that word trimmed.
* Multiple words with multiple spaces in between → reduce to single spaces.

Would you like me to handle these explicitly in code?

---

### 3️⃣ Consider Brute-Force and Optimal Approach

**Brute-force idea**:
I could use built-in functions like `strip()`, `split()`, and `reverse()` to trim the spaces, split the string into words, reverse the list, and join the words again with a single space.

This is acceptable for most use cases, and in Python, this would be just one line.

But I believe you may be testing for deeper string manipulation skills, so I’ll consider a **manual Two Pointer approach** where I process the string character-by-character from right to left and manually extract each word.

That way, I avoid using high-level built-ins like `split()` or `reverse()` and demonstrate my control over low-level string operations.

---

### 4️⃣ Explain and Implement Optimal Code (with Commentary)

Let me walk you through the code using Python first:

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        res = []  # Store words in reversed order
        i = len(s) - 1  # Start from the end

        while i >= 0:
            # Step 1: Skip trailing spaces
            while i >= 0 and s[i] == ' ':
                i -= 1
            if i < 0:
                break  # We've processed all characters

            # Step 2: Mark the end of the word
            j = i

            # Step 3: Move left to find the start of the word
            while i >= 0 and s[i] != ' ':
                i -= 1

            # Step 4: Extract the word from s[i+1 to j] and append to result
            res.append(s[i + 1 : j + 1])

        # Step 5: Join the words with a single space
        return ' '.join(res)
```

And here's how I’d implement it in JavaScript:

```javascript
var reverseWords = function(s) {
    const res = [];
    let i = s.length - 1;

    while (i >= 0) {
        // Skip spaces from the end
        while (i >= 0 && s[i] === ' ') i--;
        if (i < 0) break;

        let j = i;

        // Move left to find the start of the word
        while (i >= 0 && s[i] !== ' ') i--;

        // Extract and push the word
        res.push(s.slice(i + 1, j + 1));
    }

    return res.join(" ");
};
```

So the idea is to scan the string backward, word by word, skipping spaces and appending each found word to the result list. Then we join everything with a single space.

---

### 5️⃣ Discuss Time and Space Complexity

* **Time Complexity** is **O(n)** where n is the length of the string.
  We go through each character only once during the backward scan.

* **Space Complexity** is also **O(n)** to store the reversed words in a list.

We’re not doing any unnecessary allocations or nested loops, so this is quite optimal.

---

### 6️⃣ Mention Follow-up Questions

Some possible follow-up questions could be:

* Can you solve this **in-place** if the input is a mutable character array?
* What if you were working in **C or C++** and had to manage memory manually?
* Can you do it in a **streaming fashion**, where input is a stream of characters and you don’t have access to the full string?

Also, I could be asked to compare this with the built-in `.split().reverse().join()` approach and justify when a manual approach is more appropriate.

#
#
#

## 🎯 Real-World Applications｜實際應用場景



### 📨 Text Preprocessing in Chatbots｜聊天機器人的文字預處理

#### EN

In chatbots or NLP systems, we often receive messy user input with irregular spacing.
Before feeding it to an NLP model or logic engine, we need to **clean and normalize** the sentence structure by trimming, compressing spaces, and sometimes reversing word order for context analysis.

#### 中文

在聊天機器人或自然語言處理系統中，使用者常常輸入不規則的文字（前後空格、多個空格）。
在送入 NLP 模型或分析引擎之前，必須先**清理與標準化句子格式**，這時就會使用像這題的字串處理技巧（例如去除多餘空格、甚至反轉語序）。

---

### 🗂️ Log Analysis or Command History Parsing｜日誌分析與指令紀錄解析

#### EN

System logs or command history may include commands with inconsistent spacing.
To improve searchability, clarity, or sorting, we might need to **reverse command sequences** or normalize them — for example, reversing shell commands in audit trails.

#### 中文

系統日誌或命令記錄中可能出現不一致的空格與格式。
為了改善可讀性與查詢效率，有時會需要**反轉命令的順序**，或者去除空格、清洗資料，例如反轉 shell audit log 中的指令。

---

### 🔍 Search Engine Query Handling｜搜尋引擎的查詢前處理

#### EN

Users often enter search queries with extra spaces or in reverse phrasing.
To improve match accuracy, we might **reverse the query terms** or remove extra spaces to create a consistent internal representation.

#### 中文

使用者在搜尋引擎輸入查詢時，常常帶有多餘空格，或用倒裝句表達。
為了提升查詢匹配率，系統需要先**反轉關鍵詞順序**或**移除空白字符**來建立一致的內部格式。

---

### 📄 Data Cleaning in ETL Pipelines｜ETL 資料清洗過程

#### EN

When ingesting unstructured or semi-structured text data from external sources, cleaning up string formatting—including reversing word order or standardizing spacing—is a common requirement in ETL (Extract, Transform, Load) pipelines.

#### 中文

在從外部資料源匯入非結構化或半結構化文字資料的過程中，常常需要**清洗字串格式**，包括反轉單詞順序、移除多餘空格等，這是 ETL 流程中的常見工作。

---

### 📲 Mobile App or Web UI Input Sanitization｜手機或網頁輸入欄位的處理

#### EN

User input in mobile apps or web forms often includes accidental extra spaces.
Before submitting or displaying the text, we often use **string normalization** functions like this one to ensure consistent display or backend processing.

#### 中文

在手機 App 或網站輸入欄位中，使用者常常會不小心輸入多個空格。
在資料送出或顯示前，會用到像這題一樣的**字串正規化處理**方法，確保前端與後端的資料一致性。


