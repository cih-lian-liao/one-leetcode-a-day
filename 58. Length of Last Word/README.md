
# ✂️ LeetCode 58 - Length of Last Word


#### ✨ UMPIRE Method (English Version)


### 🔍 U - Understand the Problem

#### Problem:
You are given a string `s` containing words and spaces. Your task is to return the **length of the last word**.

#### A word:
A maximal substring consisting of only **non-space** characters.

#### Constraints:
- The input will not be empty.
- Words are separated by a single or multiple spaces.

#### Examples:
- Input: `"Hello World"` → Output: `5`
- Input: `"   fly me   to   the moon  "` → Output: `4`
- Input: `"luffy is still joyboy"` → Output: `6`

---

### 🧠 M - Match to Known Patterns

- This is a **string parsing** problem.
- Related to: trimming, splitting, reverse traversal.
- Consider the difference between **words** and **spaces**.
- Avoid relying only on `.split()` in interviews if you want to show lower-level logic.

---

### 📝 P - Plan the Solution

#### Plan 1 - Using `.strip()` and `.split()`:
1. Trim the string to remove leading/trailing spaces.
2. Split the string into words using `" "` as the delimiter.
3. Return the length of the last word in the resulting list.

#### Plan 2 - Reverse Traversal:
1. Start from the end of the string.
2. Skip any trailing spaces.
3. Count the number of characters until you reach a space.
4. Return that count.

---

### 💻 I - Implement the Code

#### 🐍 Python (Plan 2: Reverse Traversal)

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        length = 0
        i = len(s) - 1

        # Step 1: Skip trailing spaces
        while i >= 0 and s[i] == " ":
            i -= 1

        # Step 2: Count characters until the next space or beginning of string
        while i >= 0 and s[i] != " ":
            length += 1
            i -= 1

        return length
````

#### 🌐 JavaScript (Plan 2: Reverse Traversal)

```javascript
var lengthOfLastWord = function(s) {
    let length = 0;
    let i = s.length - 1;

    // Step 1: Skip trailing spaces
    while (i >= 0 && s[i] === " ") {
        i--;
    }

    // Step 2: Count characters in the last word
    while (i >= 0 && s[i] !== " ") {
        length++;
        i--;
    }

    return length;
};
```

---

### 🔎 R - Review and Optimize

| Approach     | Time Complexity | Space Complexity | Notes                         |
| ------------ | --------------- | ---------------- | ----------------------------- |
| `.split()`   | O(n)            | O(n)             | Very readable, extra space    |
| Reverse scan | O(n)            | O(1)             | Efficient, interview-friendly |

---

### 📊 E - Evaluate the Complexity

* **Time Complexity**: O(n), we scan the string once
* **Space Complexity**: O(1), we only use a few variables (no extra data structures)

#
#
#

#### 🈶 UMPIRE 方法（中文版本）

---

### 🔍 U - 理解題目

#### 題目說明：

你會得到一個字串 `s`，裡面包含單字與空格。請回傳「最後一個單字」的長度。

#### 定義：

* 單字：一段連續的、**非空格的字元**組成的字串。

#### 範例：

* 輸入："Hello World" → 回傳：5
* 輸入："   fly me   to   the moon  " → 回傳：4
* 輸入："luffy is still joyboy" → 回傳：6

---

### 🧠 M - 題型比對

* 這是一題基本的**字串處理題**。
* 可以使用 `.strip()` + `.split()` 快速解。
* 如果想展示更底層邏輯，可以用倒序掃描法，效率更高、無需額外空間。
* 常見考點：如何處理尾部空格、如何分辨字元與空格。

---

### 📝 P - 解題計畫

#### 方法一（split 版）：

1. 使用 `strip()` 移除首尾空白。
2. 使用 `split()` 將字串切成字串陣列。
3. 回傳最後一個字串的長度。

#### 方法二（倒序掃描）：

1. 從尾部開始，先跳過所有空格。
2. 遇到第一個不是空格的字元就開始計數。
3. 遇到下一個空格或開頭就停止。
4. 回傳計數值。

---

### 💻 I - 程式碼實作

#### 🐍 Python（倒序掃描法）

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        length = 0
        i = len(s) - 1

        # 步驟一：跳過尾端空格
        while i >= 0 and s[i] == " ":
            i -= 1

        # 步驟二：開始計算最後一個單字的長度
        while i >= 0 and s[i] != " ":
            length += 1
            i -= 1

        return length
```

#### 🌐 JavaScript（倒序掃描法）

```javascript
var lengthOfLastWord = function(s) {
    let length = 0;
    let i = s.length - 1;

    // 步驟一：先跳過結尾空格
    while (i >= 0 && s[i] === " ") {
        i--;
    }

    // 步驟二：開始計算最後一個單字長度
    while (i >= 0 && s[i] !== " ") {
        length++;
        i--;
    }

    return length;
};
```

---

### 🔎 R - 檢查與優化

| 方法         | 時間複雜度 | 空間複雜度 | 特性與建議        |
| ---------- | ----- | ----- | ------------ |
| `.split()` | O(n)  | O(n)  | 可讀性高，適合初學者   |
| 倒序掃描       | O(n)  | O(1)  | 無額外空間，適合面試使用 |

---

### 📊 效能分析

* **時間複雜度**：O(n)（最多走過一次整個字串）
* **空間複雜度**：O(1)（只使用少量變數）

#
#
#


# 🎤 Full Spoken-Style Interview Answer


### 1️⃣ Clarify the Problem

"Sure! Just to clarify the problem:

We're given a string `s` that consists of words and spaces. A word is defined as a **maximal substring of non-space characters**.

Our task is to return the **length of the last word** in the string.

For example:

* Input: `'Hello World'` → Output: `5`
* Input: `'   fly me   to   the moon  '` → Output: `4`

Is that understanding correct? And can we assume that the input string is non-empty?"

---

### 2️⃣ Discuss Edge Cases

"Here are some edge cases that come to mind:

* The input string has **only one word** like `'word'`
* The string contains **multiple trailing spaces**, like `'word   '`
* There are **multiple spaces between words**, for example: `'a   b   c'`
* The string is just `'     abc'`, where the word is at the very end but preceded by many spaces"

---

### 3️⃣ Consider Brute-Force and Optimal Approach

"The brute-force approach is to:

1. Use `strip()` to remove trailing and leading spaces
2. Then use `split(" ")` to split the string by spaces
3. Return the length of the last element in the list

However, this uses additional space, and in interviews, we typically prefer a more efficient approach.

So the **optimal solution** is to **scan the string from the end**, skipping any trailing spaces, and count the characters of the last word until we hit a space or the beginning of the string. This approach is **O(n) time and O(1) space**."

---

### 4️⃣ Explain and Implement Optimal Code

"Let me walk you through the optimal implementation in Python first."

#### 🐍 Python Code (spoken while coding):

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        length = 0
        i = len(s) - 1

        # First, we skip trailing spaces from the end
        while i >= 0 and s[i] == " ":
            i -= 1

        # Then, we count the characters of the last word
        while i >= 0 and s[i] != " ":
            length += 1
            i -= 1

        return length
```

"As you can see, we move backward through the string. First we ignore any spaces at the end, then we count the letters until we hit a space again."

"Here's the same approach in JavaScript:"

#### 🌐 JavaScript Code (spoken while coding):

```javascript
var lengthOfLastWord = function(s) {
    let length = 0;
    let i = s.length - 1;

    // Skip trailing spaces
    while (i >= 0 && s[i] === ' ') {
        i--;
    }

    // Count the characters of the last word
    while (i >= 0 && s[i] !== ' ') {
        length++;
        i--;
    }

    return length;
};
```

"This approach avoids creating extra arrays or substrings, which keeps our space usage constant."

---

### 5️⃣ Discuss Time and Space Complexity

"The time complexity is **O(n)** since in the worst case we scan the entire string once.

The space complexity is **O(1)** because we're using only a few integer variables, no extra data structures or arrays."

---

### 6️⃣ Mention Follow-Up Questions

"Some follow-up questions that might come up could include:

* What if we had to find the *first* word instead of the last?
* How would we handle punctuation or special characters?
* What if we had to return the last word itself, not just its length?
* Can we generalize this logic to work with non-space delimiters, like commas or tabs?"

#
#
#



## 🎯 Real-World Applications｜實際應用場景



### ✏️ Text Editor Functionality｜文字編輯器的功能處理

**EN**:
When implementing a text editor or a note-taking app, you may need to identify the length of the last word the user typed—for example, to support **real-time word count**, **autocomplete suggestions**, or **styling just the last word**.

**ZH**：
在開發文字編輯器或筆記應用程式時，常常會需要判斷使用者輸入內容中的「最後一個單字」長度，例如要支援**即時字數統計**、**自動完成提示**、或是**針對最後一個字加粗或上色**等功能。

**Example**:

* Auto-format the last word if it exceeds a certain length.
* Show real-time grammar suggestions for the last word only.

---

### 🧾 Form Input Processing｜表單輸入處理

**EN**:
In form validations or user feedback systems, extracting and analyzing the **last word of a sentence or response** is useful—especially in surveys or search engines where user intent often lies in the final word.

**ZH**：
在處理表單輸入或用戶反饋時，判斷「句尾最後一個單字」的內容與長度很有幫助。例如問卷調查、搜尋框輸入等場景中，最後一個單字往往透露使用者真正的意圖。

**Example**:

* Highlight the last word typed into a search box for predictive search suggestions
* Ensure the last word in a sentence meets formatting rules

---

### 📃 Natural Language Processing (NLP)｜自然語言處理應用

**EN**:
In NLP preprocessing steps, it's common to extract or manipulate **specific words in a sentence**, especially the last word when doing sequence-based prediction models like LSTMs or GPT.

**ZH**：
在自然語言處理（NLP）的前處理流程中，常需要抽取或分析句子中的特定單字，尤其是**最後一個字詞**，常常是訓練語言模型時的關鍵，例如在 LSTM 或 GPT 類模型中做預測。

**Example**:

* Feed the last word of a sentence into a next-word prediction model
* Tag the final word in a sentence for part-of-speech labeling

---

### 🔍 Command Line Parsing｜命令列解析

**EN**:
In systems programming or command-line tools, it is common to **parse the last argument or token** from a user command. Being able to split and analyze from the end of the input is key for flexibility and efficiency.

**ZH**：
在系統程式設計或 CLI 工具開發中，經常需要分析使用者輸入指令的「**最後一個參數**」。這種情況下，要能夠從尾部解析最後一段字元，是提升彈性與效能的關鍵。

**Example**:

* Extracting the last argument passed to a shell script
* Validating the last word in a git commit message for formatting rules


