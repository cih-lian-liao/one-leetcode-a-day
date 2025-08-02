

# 📘 LeetCode 28 - Find the Index of the First Occurrence in a String

#### 🧩 English Version - UMPIRE Method

### 🔍 U — Understand the Problem

Given two strings `haystack` and `needle`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

If `needle` is an empty string, return `0` (as defined by the problem spec).

---

### 🧠 M — Match Examples

#### Example 1

```txt
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
```

#### Example 2

```txt
Input: haystack = "leetcode", needle = "leeto"
Output: -1
```

#### Example 3

```txt
Input: haystack = "a", needle = ""
Output: 0
```

---

### 🛠️ P — Plan

#### ✅ Approach 1: Built-in function

Use language-provided search utilities like `find()` in Python or `indexOf()` in JavaScript.

#### ✅ Approach 2: Manual comparison (Brute-force)

Check all possible substrings of `haystack` of the same length as `needle`, one by one.

---

### 💻 I — Implement

#### 🐍 Python - Built-in Function

```python
def strStr(haystack: str, needle: str) -> int:
    """
    Uses Python's built-in string find method.
    Returns the index of the first occurrence of needle in haystack.
    """
    return haystack.find(needle)
```

#### 🐍 Python - Manual Comparison (Brute-force)

```python
def strStr(haystack: str, needle: str) -> int:
    # Edge case: if needle is an empty string, return 0
    if needle == "":
        return 0

    # Loop through all valid starting indices in haystack
    for i in range(len(haystack) - len(needle) + 1):
        # Extract substring from haystack starting at i
        if haystack[i:i+len(needle)] == needle:
            return i  # Match found at index i

    # If no match found, return -1
    return -1
```

#### 🌐 JavaScript - Built-in Function

```javascript
var strStr = function(haystack, needle) {
    // Uses JavaScript's built-in indexOf function
    return haystack.indexOf(needle);
};
```

#### 🌐 JavaScript - Manual Comparison (Brute-force)

```javascript
var strStr = function(haystack, needle) {
    // Edge case: return 0 for empty needle
    if (needle === "") return 0;

    // Loop through haystack until the last possible starting index
    for (let i = 0; i <= haystack.length - needle.length; i++) {
        // Extract substring from haystack starting at index i
        if (haystack.slice(i, i + needle.length) === needle) {
            return i;  // Found a match
        }
    }

    // If no match is found
    return -1;
};
```

---

### 🔎 R — Review

Test cases to validate the implementation:

```python
assert strStr("hello", "ll") == 2
assert strStr("aaaaa", "bba") == -1
assert strStr("", "") == 0
```

---

### 📈 E — Evaluate

| Approach          | Time Complexity    | Space Complexity |
| ----------------- | ------------------ | ---------------- |
| Built-in Function | Varies (optimized) | O(1)             |
| Manual Comparison | O((N-M+1)×M)       | O(1)             |

#
#
#

#### 📙 中文版 - UMPIRE 方法

### 🔍 U — 理解問題

給你兩個字串 `haystack`（乾草堆）與 `needle`（針），請回傳 `needle` 第一次在 `haystack` 中出現的起始索引。如果 `needle` 不存在於 `haystack` 中，請回傳 `-1`。

如果 `needle` 是空字串，根據題目定義應回傳 0。

---

### 🧠 M — 匹配範例

#### 範例 1

```txt
輸入: haystack = "sadbutsad", needle = "sad"
輸出: 0
```

#### 範例 2

```txt
輸入: haystack = "leetcode", needle = "leeto"
輸出: -1
```

#### 範例 3

```txt
輸入: haystack = "a", needle = ""
輸出: 0
```

---

### 🛠️ P — 解題計畫

#### ✅ 解法一：使用內建函式

* Python 使用 `find()`，JavaScript 使用 `indexOf()`。

#### ✅ 解法二：手動比對（暴力解法）

* 對 `haystack` 中每一個可能的位置進行比對，看是否出現 `needle`。

---

### 💻 I — 程式碼實作

#### 🐍 Python - 內建函式法

```python
def strStr(haystack: str, needle: str) -> int:
    # 使用 Python 內建的 find 方法，找不到會回傳 -1
    return haystack.find(needle)
```

#### 🐍 Python - 手動比對法

```python
def strStr(haystack: str, needle: str) -> int:
    # 如果 needle 是空字串，直接回傳 0
    if needle == "":
        return 0

    # 從 haystack 中每個可能的位置開始比對
    for i in range(len(haystack) - len(needle) + 1):
        # 比對長度與 needle 一樣的子字串
        if haystack[i:i+len(needle)] == needle:
            return i  # 找到則回傳起始位置

    return -1  # 全部檢查完都沒找到
```

#### 🌐 JavaScript - 內建函式法

```javascript
var strStr = function(haystack, needle) {
    // 使用 JavaScript 內建 indexOf 函式
    return haystack.indexOf(needle);
};
```

#### 🌐 JavaScript - 手動比對法

```javascript
var strStr = function(haystack, needle) {
    // 若 needle 為空，根據題目定義回傳 0
    if (needle === "") return 0;

    // 從 haystack 每個合法位置開始取出等長子字串比對
    for (let i = 0; i <= haystack.length - needle.length; i++) {
        if (haystack.slice(i, i + needle.length) === needle) {
            return i;  // 找到匹配
        }
    }

    // 沒有匹配，回傳 -1
    return -1;
};
```

---

### 🔎 R — 測試回顧

```python
assert strStr("abc", "") == 0
assert strStr("aaaaa", "bba") == -1
assert strStr("mississippi", "issip") == 4
```

---

### 📈 E — 效能評估

| 解法   | 時間複雜度                | 空間複雜度 |
| ---- | -------------------- | ----- |
| 內建函式 | 視語言實作而定（可能為 KMP 或更優） | O(1)  |
| 手動比對 | O((N-M+1)×M)         | O(1)  |

#
#
#

# 🎤 Full Spoken-Style Interview Answer

*LeetCode 28 – Find the Index of the First Occurrence in a String*

### 1️⃣ Clarify the Problem

🗣️

> Sure! So, we’re given two input strings — one is called `haystack` and the other is called `needle`.
> We need to **find the index of the first occurrence of `needle` in `haystack`**, and return that index.
> If the `needle` is not present inside the `haystack`, then we return `-1`.

> For example, if `haystack = "sadbutsad"` and `needle = "sad"`, the output should be `0`, because `"sad"` first appears at index `0`.
> If `haystack = "leetcode"` and `needle = "leeto"`, we return `-1` because `"leeto"` doesn’t appear in the string.

> I’ll assume that both inputs are non-null and valid strings, and we can use either built-in functions or manual logic depending on the constraints.

---

### 2️⃣ Discuss Edge Cases

🗣️

> I’d like to go over a few edge cases before jumping into implementation.

* **Empty needle**: If the `needle` is an empty string, the problem says we should return `0` — this is a special case defined by the problem itself.
* **Needle longer than haystack**: If the `needle` is longer than the `haystack`, there’s no way it can be a substring, so we can immediately return `-1`.
* **Exact match**: If `haystack` and `needle` are exactly the same, we return `0` as it starts at the beginning.
* **Needle at the end of haystack**: We need to make sure our logic captures substring matching at the last valid position.

---

### 3️⃣ Consider Brute-Force and Optimal Approach

🗣️

> There are two main approaches I can think of:

#### ✅ Brute-force:

> We can iterate through all possible starting indices of the `haystack` and for each index, compare a substring of the same length as the `needle`.
> This has a worst-case time complexity of **O((N - M + 1) × M)**, where `N` is the length of the `haystack` and `M` is the length of the `needle`.

#### ✅ Built-in Function:

> If allowed, we can use built-in functions like `str.find()` in Python or `indexOf()` in JavaScript, which are optimized internally.

---

### 4️⃣ Explain and Implement Optimal Code

🗣️

> I’ll go ahead and implement the **brute-force solution** manually, which shows clear logic, and is acceptable unless a highly optimized solution like KMP is specifically required.

---

#### 🐍 Python – Manual Implementation with Explanation

```python
def strStr(haystack: str, needle: str) -> int:
    # If needle is an empty string, by definition return 0
    if needle == "":
        return 0

    # Loop through each valid starting index in haystack
    for i in range(len(haystack) - len(needle) + 1):
        # Extract a substring from haystack of the same length as needle
        if haystack[i:i+len(needle)] == needle:
            # If it matches, return the current index
            return i

    # If we reach here, needle was not found in haystack
    return -1
```

🗣️

> As you can see, I first check if the needle is empty, and if so, I return 0.
> Then I loop through all possible start positions in the `haystack` where a match could occur — that's up to `len(haystack) - len(needle)`.
> For each index `i`, I extract the substring starting at `i` with the same length as `needle` and compare it.
> If I find a match, I immediately return the index. Otherwise, I return `-1`.

---

#### 🌐 JavaScript – Manual Implementation

```javascript
var strStr = function(haystack, needle) {
    // Return 0 if needle is an empty string
    if (needle === "") return 0;

    // Iterate through each possible starting index
    for (let i = 0; i <= haystack.length - needle.length; i++) {
        // Extract the substring and compare
        if (haystack.slice(i, i + needle.length) === needle) {
            return i;
        }
    }

    // Return -1 if no match was found
    return -1;
};
```

🗣️

> This is the same logic in JavaScript. I use `slice()` to extract substrings and compare them with `needle`.
> If no match is found, I return `-1` at the end.

---

### 5️⃣ Discuss Time & Space Complexity

🗣️

> In terms of **time complexity**, the worst case is:

* We loop through up to `N - M + 1` positions (where `N = haystack.length`, `M = needle.length`)
* At each position, we may compare up to `M` characters

So total time complexity is **O((N - M + 1) × M)** in the worst case.

> For **space complexity**, we are not using any extra space apart from a few variables, so it’s **O(1)**.

> If we used `str.find()` or `indexOf()`, the time complexity could be better depending on internal implementation — some may use KMP or Boyer-Moore behind the scenes.

---

### 6️⃣ Mention Follow-Up Questions

🗣️

> A few follow-up questions that could be asked include:

* **How would you find all occurrences, not just the first one?**
  We can store the indices in a list instead of returning early.

* **What if you had to optimize for time?**
  Then I might implement the **KMP algorithm** which runs in **O(N + M)** time. It avoids rechecking previously matched characters by using a prefix table.

* **How would this work on large inputs?**
  For massive texts or streaming data, we might consider suffix arrays, or use Rabin-Karp with hashing to improve average-case performance.

---

🧠

> That’s my approach for solving this problem. Let me know if you’d like me to walk through the KMP version as well!

#
#
#

## 🎯 Real-World Applications｜實際應用場景

### 📝 Text Search in Documents｜文件中的文字搜尋

> **EN**: Applications like Google Docs or Microsoft Word use substring search algorithms to highlight the first occurrence of a searched keyword.
> **ZH**：像 Google Docs 或 Word 這類文件編輯工具，會使用子字串搜尋來找到並標記使用者輸入的關鍵字第一次出現的位置。

---

### 🔎 Search Highlights on Webpages｜網頁內的關鍵字搜尋與定位

> **EN**: Browsers (like Chrome) implement substring search to jump to the first match when using Ctrl+F (Find).
> **ZH**：瀏覽器（像 Chrome）實作 Ctrl+F 功能時會搜尋字串第一次出現的位置，這個過程背後就類似這題的邏輯。

---

### 📧 Email Spam Detection｜垃圾郵件偵測

> **EN**: Spam filters may scan email bodies for specific keywords or patterns. Substring matching helps locate these keywords early in the content.
> **ZH**：垃圾郵件偵測系統會掃描郵件內容中是否包含某些敏感字詞，若快速找到這些「關鍵字第一次出現的位置」，就能判斷是否屬於垃圾信。

---

### 🔐 Command Injection Detection in Security｜資訊安全中的命令注入比對

> **EN**: In security systems, substring search is used to detect the presence of dangerous commands (e.g., `rm -rf`) within input strings.
> **ZH**：資訊安全系統會檢查使用者輸入是否含有危險字串，例如 `rm -rf`，這時就會用到類似本題的比對方法。

---

### 🧬 DNA Sequence Matching｜DNA 序列比對

> **EN**: In bioinformatics, scientists search for specific gene sequences (substrings) within a long DNA strand.
> **ZH**：在生物資訊學中，研究人員會搜尋某段特定基因序列是否出現在一長串的 DNA 序列中，就像是用 `needle` 找 `haystack`。

---

### 🛒 Search Suggestions in E-Commerce｜電商搜尋建議比對

> **EN**: When a user types in a product search bar, the system searches the product names to find those containing the typed keywords.
> **ZH**：在購物網站中，使用者輸入文字時，系統會即時比對產品名稱中是否包含使用者輸入的關鍵字，並顯示建議。

---

### 💬 Chatbot Keyword Triggers｜聊天機器人觸發關鍵字偵測

> **EN**: A chatbot may search for specific trigger phrases in user input to decide which response or workflow to execute.
> **ZH**：聊天機器人會分析使用者的輸入內容，若偵測到某些子字串（如「我要訂餐」），就會進入對應流程，這也是子字串比對的應用。
