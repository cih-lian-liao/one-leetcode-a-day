# 🧠 LeetCode 14: Longest Common Prefix

#### ✨ UMPIRE Method (English Version)

### 🔍 U — Understand

#### Problem:

* Input: A list of strings
* Output: The longest common prefix string shared among all input strings
* If there's no common prefix, return an empty string `""`.

#### Example:

```text
Input: ["flower", "flow", "flight"]
Output: "fl"

Input: ["dog", "racecar", "car"]
Output: ""
```

---

### 🔎 M — Match

This is a **string prefix comparison** problem.
Common patterns:

* Vertical scanning (compare character by character)
* Horizontal scanning (reduce prefix step-by-step)
* Trie (advanced)
* Binary Search (advanced)

We'll use **Vertical Scanning**, which is simple and effective.

---

### 🧠 P — Plan

Step-by-step vertical scan:

1. Start from index `0` of the first string.
2. Check whether every other string has the same character at that index.
3. If all match, move to next index.
4. If not, return the substring up to that index.

---

### 💻 I — Implement

#### ✅ Python (Vertical Scanning)

```python
from typing import List

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""  # No input strings

        # Loop through each character index of the first string
        for i in range(len(strs[0])):
            char = strs[0][i]  # Character to compare

            # Compare this character with the same index in all other strings
            for s in strs[1:]:
                # If the index is out of range OR characters don't match
                if i >= len(s) or s[i] != char:
                    return strs[0][:i]  # Return prefix up to mismatch
        return strs[0]  # All characters matched, return whole first string
```

#### ✅ JavaScript (Vertical Scanning)

```javascript
function longestCommonPrefix(strs) {
  if (!strs.length) return ""; // Empty input

  // Loop through characters of the first string
  for (let i = 0; i < strs[0].length; i++) {
    const char = strs[0][i]; // Character to compare

    // Check this character in all other strings
    for (let j = 1; j < strs.length; j++) {
      // If out of bounds OR mismatch found
      if (i >= strs[j].length || strs[j][i] !== char) {
        return strs[0].substring(0, i); // Return prefix
      }
    }
  }
  return strs[0]; // All matched
}
```

---

### 🧪 R — Review & Test

#### Test Cases:

```text
Input: ["flower", "flow", "flight"] => Output: "fl"
Input: ["dog", "racecar", "car"]   => Output: ""
Input: ["apple"]                   => Output: "apple"
Input: ["a", "a", "a"]             => Output: "a"
Input: ["ab", "a"]                 => Output: "a"
```

---

### 📈 E — Evaluate

* **Time Complexity**: O(S), where S is the total number of characters in all strings.
* **Space Complexity**: O(1), no extra space used.

#
#
#

#### 🧠 中文版本：UMPIRE 方法

### 🔍 U — 理解題目

#### 題目說明：

* 輸入：一個字串陣列
* 輸出：所有字串共同擁有的最長前綴（Longest Common Prefix）
* 若沒有任何共同前綴，則回傳空字串 `""`

#### 範例：

```text
Input: ["flower", "flow", "flight"]
Output: "fl"

Input: ["dog", "racecar", "car"]
Output: ""
```

---

### 🔎 M — 題型對應

這是一題經典的「字串前綴比對」問題。
常見解法：

* 垂直掃描（Vertical Scanning）：逐字元比對
* 水平掃描：一個一個合併比較
* Trie 樹（進階）
* 二分搜尋（進階）

我們這裡使用最直觀好實作的 **垂直掃描法**。

---

### 🧠 P — 解題規劃

#### 步驟：

1. 以第一個字串為基準，從索引 0 開始掃描
2. 比對所有其他字串在相同索引的字元是否相同
3. 若一致就繼續下一個字元
4. 若遇到不一致，回傳目前為止的前綴

---

### 💻 I — 程式碼實作

#### ✅ Python（垂直掃描）

```python
from typing import List

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""  # 沒有輸入字串

        # 逐字元比對第一個字串
        for i in range(len(strs[0])):
            char = strs[0][i]  # 目前比對的字元

            for s in strs[1:]:
                # 若某字串已結束或字元不相符
                if i >= len(s) or s[i] != char:
                    return strs[0][:i]  # 回傳目前的前綴
        return strs[0]  # 所有字元皆相符
```

#### ✅ JavaScript（垂直掃描）

```javascript
function longestCommonPrefix(strs) {
  if (!strs.length) return ""; // 無輸入

  for (let i = 0; i < strs[0].length; i++) {
    const char = strs[0][i]; // 目前比對字元

    for (let j = 1; j < strs.length; j++) {
      // 若超出字串長度 或 字元不一致
      if (i >= strs[j].length || strs[j][i] !== char) {
        return strs[0].substring(0, i); // 回傳目前前綴
      }
    }
  }
  return strs[0]; // 全部相符
}
```

---

### 🧪 R — 測試範例

```text
Input: ["flower", "flow", "flight"] => Output: "fl"
Input: ["dog", "racecar", "car"]   => Output: ""
Input: ["apple"]                   => Output: "apple"
Input: ["a", "a", "a"]             => Output: "a"
Input: ["ab", "a"]                 => Output: "a"
```

---

### 📈 E — 效能分析

* **時間複雜度**：O(S)，其中 S 為所有字串總字元數
* **空間複雜度**：O(1)，沒有使用額外記憶體空間


#
#
#


### 🧾 `[:i]` 是什麼？

這是一種 **切片（slice）語法**，常用來從字串或列表中，取出 **一段子序列（substring 或 sublist）**。


### 🔍 `[:i]` 的含義

```python
str[:i]
```

### 意思是：

從索引 `0` 開始，到索引 `i - 1` 為止（**不包含 `i` 本身**）的子字串。


### 📦 舉例說明

```python
s = "flower"
print(s[:2])   # -> "fl"
print(s[:4])   # -> "flow"
print(s[:0])   # -> "" (空字串)
print(s[:len(s)])  # -> "flower" (整個字串)
```

### 🧠 更通用的語法形式

```python
s[start:end]  # 從 start 到 end-1
```

* `s[:i]` → 相當於 `s[0:i]`
* `s[i:]` → 從 i 到最後
* `s[start:end:step]` → 可以加上步長


### 📎 小提醒

切片操作是 **不包含右邊界** 的（right-exclusive），也就是「左閉右開」區間 `[start, end)`。


### 💬 在 LeetCode 14 中的用途

```python
return strs[0][:i]
```

這行的意思是：
👉 回傳第一個字串從第 0 個字元到第 `i-1` 個字元的部分，也就是目前為止大家共同擁有的 **最長前綴**。


如果你想要 JavaScript 中的對應語法，則是使用：

```javascript
str.substring(0, i);
```

功能一樣：從 index 0 到 index i（不包含 i）。

#
#
#


# 🎤 Full Spoken-Style Interview Answer

**LeetCode 14 — Longest Common Prefix**


### 1. ✅ Clarify the Problem

"Sure! So the problem asks us to find the longest common prefix string among an array of strings. If there's no common prefix, we just return an empty string.

For example, if the input is `['flower', 'flow', 'flight']`, the output should be `'fl'`. But if the input is `['dog', 'racecar', 'car']`, since there's no common prefix among all of them, we return `''`."

---

### 2. 🚧 Discuss Edge Cases

"Let's consider some edge cases before diving in:

* If the array is empty, we should return an empty string.
* If the array has only one string, the prefix is the string itself.
* If there's at least one string that is empty, the common prefix is also empty.
* If all strings are the same, we return the full string.
* If only the first character matches, we return just that one character, or empty if none match."

---

### 3. 💡 Consider Brute-Force and Optimal Approach

"Alright, so the brute-force idea would be to compare all strings character-by-character from index 0 onward.

There are a few ways to do this:

* One is horizontal scanning — start with the first string and trim it down based on comparison with each following string.
* Another way is vertical scanning — for each character index, compare that character across all strings.

I’ll go with vertical scanning because it’s simple, readable, and efficient in most cases."

---

### 4. 🧑‍💻 Explain and Implement Optimal Code (with commentary)

#### 🐍 Python

"Let me walk through the code in Python step by step:"

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        # Edge case: empty input
        if not strs:
            return ""
        
        # Loop through characters in the first string
        for i in range(len(strs[0])):
            char = strs[0][i]  # Get the character at index i
            
            # Compare this character with the same index in the rest of the strings
            for s in strs[1:]:
                # If index out of range or character doesn't match, we stop
                if i >= len(s) or s[i] != char:
                    return strs[0][:i]  # Return prefix up to this point
        
        # If we made it through the entire first string, return it
        return strs[0]
```

🗣️「So what we’re doing is comparing each character position across all strings, and as soon as we find a mismatch, we return the prefix so far. Otherwise, if we get through the whole first string, it means it’s the full common prefix.」

---

#### 🌐 JavaScript

"Here's the same logic implemented in JavaScript:"

```javascript
function longestCommonPrefix(strs) {
  if (!strs.length) return "";

  for (let i = 0; i < strs[0].length; i++) {
    const char = strs[0][i]; // Character to compare

    for (let j = 1; j < strs.length; j++) {
      if (i >= strs[j].length || strs[j][i] !== char) {
        return strs[0].substring(0, i); // Return prefix up to this point
      }
    }
  }

  return strs[0]; // All characters matched
}
```

🗣️「It’s the same idea — we check character-by-character vertically. The first mismatch or end of string gives us the stopping point.」

---

### 5. 📊 Discuss Time/Space Complexity

"Let’s break down the complexity:

* **Time complexity** is O(S), where S is the sum of all characters in the input. That’s because in the worst case, we may compare every character of every string.
* **Space complexity** is O(1) since we aren’t using any extra space other than a few variables."

---

### 6. ❓Mention Follow-Up Questions

"Some follow-up questions the interviewer might ask include:

* Can you implement this using a trie (prefix tree)?
* How would you modify this to return the longest common suffix instead?
* What if the strings were coming from a streaming source — how would you design it then?
* Could you optimize this further using binary search on prefix length?

These variations test how well I understand the trade-offs between readability, memory use, and extensibility."

#
#
#

## 🎯 Real-World Applications｜實際應用場景


### 💬 Autocomplete Systems｜自動補全系統

#### English:

When building an autocomplete feature, such as search bars or code editors, you often need to find the longest shared prefix among all possible suggestions to decide what can be auto-filled immediately.

#### 中文：

在建立自動補全功能（例如搜尋列或程式碼編輯器）時，常常需要找出所有可能選項中最長的共同前綴，以決定可以立刻幫使用者補完哪些文字。

---

### 🧭 URL Routing and API Matching｜網址路由與 API 比對

#### English:

In backend services or web frameworks, matching routes often starts by checking common prefixes of URLs or endpoints to efficiently dispatch requests.

#### 中文：

在後端服務或網站框架中，比對路由（route）時經常會先判斷 URL 或 API endpoint 的共同前綴，以有效率地將請求導向正確的位置。

---

### 🗃️ File System Path Analysis｜檔案系統路徑分析

#### English:

When analyzing or comparing file paths (e.g., for syncing files or deduplicating backups), finding the longest common prefix helps identify shared directory structures.

#### 中文：

在分析或比較檔案路徑時（例如同步檔案或備份去重），找出最長共同前綴有助於判斷是否處於相同的目錄結構下。

---

### 🔎 Search Engine Optimization｜搜尋引擎最佳化（SEO）

#### English:

To optimize crawl behavior or cluster related keywords/URLs, search engines might group content that shares common prefixes in URLs or query patterns.

#### 中文：

搜尋引擎為了最佳化爬蟲行為或關鍵字聚類，可能會依據 URL 或查詢模式中相同的前綴來分組相關內容。

---

### 🧬 DNA Sequence Matching｜DNA 序列比對

#### English:

In bioinformatics, comparing DNA sequences often involves finding shared prefixes to determine similarity or alignment start points.

#### 中文：

在生物資訊領域，比對 DNA 序列時常需要找出序列的共同前綴，以評估相似程度或對齊的起始點。

---

### 📦 Data Compression｜資料壓縮

#### English:

When implementing prefix-based encoding schemes like Huffman Coding, finding shared prefixes is fundamental to grouping and optimizing symbols.

#### 中文：

在使用像 Huffman 編碼這類基於前綴的壓縮演算法時，找出共同前綴是壓縮效率的關鍵，有助於將符號有效分群與編碼。

#
#
#


# 🌳 Trie 入門與在 LeetCode 14 的應用



### 📌 什麼是 Trie？

Trie（又叫 Prefix Tree，前綴樹）是一種專門用來處理「字串集合」的**樹狀資料結構**。它特別適合以下情境：

* 自動補全（Autocomplete）
* 字典查找（Dictionary Search）
* 前綴比對（Prefix Matching）

---

### 🧱 Trie 的基本結構

每個節點（node）表示一個字元，節點之間的連結（edge）形成某個字串的前綴路徑。

舉個例子：

假設插入字串：

```
["flower", "flow", "flight"]
```

會形成這樣的 Trie：

```
       (root)
       / 
      f
     /
    l
   / \
  o   i
 /     \
w       g
|         \
e          h
|           \
r            t
```

---

### 🔍 為什麼 Trie 適合解這題？

因為 Trie 天生就是**用來找共同前綴**的資料結構！
我們只要把所有字串都插入 Trie，然後從 root 開始走，直到遇到：

* 分支（>1 個子節點）
* 或某個字串的結尾

就可以知道最長共同前綴是哪一段了。

---

### 🧑‍💻 Python 實作：用 Trie 解 LeetCode 14

#### Step 1: 定義 TrieNode 結構

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
```

#### Step 2: 建 Trie 並插入字串

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
```

#### Step 3: 找出最長共同前綴

```python
    def longest_common_prefix(self):
        prefix = ""
        node = self.root

        while node:
            # 如果目前節點剛好有 1 個子節點，且不是字尾，繼續往下走
            if len(node.children) == 1 and not node.is_end_of_word:
                char = next(iter(node.children))
                prefix += char
                node = node.children[char]
            else:
                break
        return prefix
```

#### Step 4: 主程式整合

```python
class Solution:
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""

        trie = Trie()
        for word in strs:
            trie.insert(word)

        return trie.longest_common_prefix()
```

---

### ✅ 為什麼值得學 Trie？

| 使用情境 | 與 LeetCode 題型關聯 |
| ---- | --------------- |
| 字首查找 | L14, L208, L720 |
| 自動補全 | L648, L1268     |
| 模糊比對 | L211, L676      |
| 排序結構 | L472            |

---

### 🧪 這題用 Trie 的優缺點

| 項目          | 說明                                              |
| ----------- | ----------------------------------------------- |
| ✅ 好處        | 寫法清楚，設計專門處理字首，非常適合延伸                            |
| ❌ 缺點        | 實作比較複雜，效能比不上簡單掃描解法                              |
| 🎯 面試 bonus | 如果你先用簡單方法，再主動說：「這題其實也可以用 Trie 結構來做，更擴展性強」，會大大加分 |

---

### 🤔 還沒學 Trie，該怎麼入門？

我建議你這樣安排：

1. **理解基本概念**：為什麼 Trie 是用字母當作 key，一層一層建下去。
2. **學習基本操作**：

   * 插入（Insert）
   * 查找（Search）
   * 判斷是否為 prefix（StartsWith）
3. **練習幾題 LeetCode Trie 題目**：

   * 208. Implement Trie
   * 211. Add and Search Word
   * 14. Longest Common Prefix（進階）
   * 648. Replace Words
   * 720. Longest Word in Dictionary
