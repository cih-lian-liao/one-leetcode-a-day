

# 💡 LeetCode 13 - Roman to Integer


### 🧠 English Version｜UMPIRE Method

### ❓ U - Understand

#### Problem

We are given a string `s` that represents a Roman numeral.
Our task is to convert it to the corresponding integer.

#### Input

* A string of valid Roman numerals, e.g., `"III"`, `"IV"`, `"MCMXCIV"`

#### Output

* An integer representing the value of the Roman numeral

#### Constraints

* 1 <= `s.length` <= 15
* `s` contains only valid Roman numerals

---

### 🔍 M - Match

This is a **mapping** and **greedy** problem.
We should use a hash map to store symbol-to-value mappings, and scan the string from left to right.
If the current character is less than the next character, we subtract it. Otherwise, we add it.

---

### 📝 P - Plan

1. Create a dictionary (map) of Roman numeral values.
2. Initialize a result variable to 0.
3. Loop over the string from left to right.
4. If the next character is larger than the current one → subtract current.
5. Otherwise → add current.
6. Return result.

---

### 🛠️ I - Implement

#### 🐍 Python Code

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        # Step 1: Define Roman symbol-to-value map
        roman_map = {
            'I': 1, 'V': 5, 'X': 10,
            'L': 50, 'C': 100, 'D': 500, 'M': 1000
        }

        total = 0  # Step 2: Initialize result accumulator

        # Step 3: Loop through the string
        for i in range(len(s)):
            current_value = roman_map[s[i]]

            # Step 4: If next value exists and is greater → subtract current
            if i + 1 < len(s) and current_value < roman_map[s[i + 1]]:
                total -= current_value
            else:
                total += current_value

        return total  # Step 5: Return final result
```

#### 🌐 JavaScript Code

```javascript
var romanToInt = function(s) {
    // Step 1: Create symbol-to-value mapping
    const romanMap = {
        'I': 1, 'V': 5, 'X': 10,
        'L': 50, 'C': 100, 'D': 500, 'M': 1000
    };

    let total = 0;  // Step 2: Initialize result

    // Step 3: Loop through string
    for (let i = 0; i < s.length; i++) {
        const current = romanMap[s[i]];
        const next = romanMap[s[i + 1]];

        // Step 4: If next is larger, subtract current
        if (next > current) {
            total -= current;
        } else {
            total += current;
        }
    }

    return total;  // Step 5: Return final result
};
```

---

### 🧪 R - Review

#### Test Cases

| Input       | Output |
| ----------- | ------ |
| `"III"`     | 3      |
| `"IV"`      | 4      |
| `"IX"`      | 9      |
| `"LVIII"`   | 58     |
| `"MCMXCIV"` | 1994   |

Edge Case:

* No invalid inputs are expected per constraints.

---

### 📈 E - Evaluate

* **Time Complexity**: O(n)
* **Space Complexity**: O(1) (fixed map size)

#
#
#

### 🌸 中文版本｜UMPIRE 筆記法

### ❓ U - Understand｜理解題目

#### 題目說明：

給定一個代表羅馬數字的字串 `s`，請將其轉換為整數數值。

#### 輸入：

* 一個只包含合法羅馬數字的字串，如 `"III"`、`"IV"`、`"MCMXCIV"`

#### 輸出：

* 對應的整數

#### 限制：

* 1 <= `s.length` <= 15
* `s` 保證是有效的羅馬數字

---

### 🔍 M - Match｜對應類型

這是一道 **對應表 + 貪婪判斷** 類型的題目。

我們建立一個「羅馬字元 → 數字」的對應表，並從左到右掃描。
若當前字母的數值小於下一個，表示是減法情況 → 減掉；否則直接加總。

---

### 📝 P - Plan｜規劃步驟

1. 建立一個對應字典 `roman_map`
2. 初始化變數 `total = 0`
3. 用 for loop 遍歷字串 s
4. 若下一個字母大於當前 → 減掉當前
5. 否則 → 加上當前
6. 回傳總和

---

### 🛠️ I - Implement｜實作程式碼

#### 🐍 Python 解法

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        # 步驟一：建立對應表
        roman_map = {
            'I': 1, 'V': 5, 'X': 10,
            'L': 50, 'C': 100, 'D': 500, 'M': 1000
        }

        total = 0  # 步驟二：初始化總和變數

        # 步驟三：從左到右遍歷字串
        for i in range(len(s)):
            current_value = roman_map[s[i]]

            # 若下一個存在且比較大 → 減法情況
            if i + 1 < len(s) and current_value < roman_map[s[i + 1]]:
                total -= current_value
            else:
                total += current_value

        return total  # 步驟五：回傳答案
```

#### 🌐 JavaScript 解法

```javascript
var romanToInt = function(s) {
    // 步驟一：建立對應表
    const romanMap = {
        'I': 1, 'V': 5, 'X': 10,
        'L': 50, 'C': 100, 'D': 500, 'M': 1000
    };

    let total = 0; // 步驟二：初始化總和

    // 步驟三：遍歷字串
    for (let i = 0; i < s.length; i++) {
        const current = romanMap[s[i]];
        const next = romanMap[s[i + 1]];

        // 若下一個比較大 → 減法情況
        if (next > current) {
            total -= current;
        } else {
            total += current;
        }
    }

    return total; // 步驟五：回傳結果
};
```

---

### 🧪 R - Review｜測試案例

| 輸入          | 輸出   |
| ----------- | ---- |
| `"III"`     | 3    |
| `"IV"`      | 4    |
| `"IX"`      | 9    |
| `"LVIII"`   | 58   |
| `"MCMXCIV"` | 1994 |

---

### 📈 E - Evaluate｜效能評估

* **時間複雜度**：O(n)，只需遍歷一次字串
* **空間複雜度**：O(1)，字典大小固定

#
#
#

# 🎤 Full Spoken-Style Interview Answer


### 🔍 1. Clarify the Problem

"Sure! So we’re given a string `s` representing a Roman numeral, and we need to convert it into an integer. Roman numerals are composed of letters like I, V, X, L, C, D, and M, each representing a specific value. For example, I is 1, V is 5, X is 10, and so on."

"The tricky part is the subtractive notation. For instance, 'IV' means 4 because I comes before V, which means we subtract 1 from 5. But 'VI' means 6 because I comes after V, so we just add. Our job is to parse this string and compute the correct integer value."

"Does that sound accurate?"

---

### 🚧 2. Discuss Edge Cases

"Some edge cases to consider might include:"

* "The smallest input, like `'I'`, which should return 1."
* "Strings with only additive notation like `'III'` or `'VIII'`."
* "Strings with multiple subtractive patterns like `'MCMXCIV'`, which contains `'CM'`, `'XC'`, and `'IV'`."

"We can assume the input is always a valid Roman numeral string since that’s given in the problem constraint."

---

### 🛠️ 3. Consider Brute-Force and Optimal Approach

"A brute-force solution might try to hardcode all the subtractive cases like 'IV', 'IX', 'XL', etc., and process those first by checking substrings. But that would be messy and not scalable."

"Instead, the optimal approach is to use a hash map to store the values of each Roman symbol. Then, we scan the string from left to right. At each position, we compare the current character with the next one. If the current character represents a smaller value, it means it’s a subtractive case, so we subtract. Otherwise, we add."

"This greedy approach allows us to handle both additive and subtractive cases in a single pass."

---

### 👩‍💻 4. Explain and Implement Optimal Code

#### Python Version

"Here’s how I would implement it in Python:"

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        # Step 1: Create a symbol-to-value map
        roman_map = {
            'I': 1, 'V': 5, 'X': 10,
            'L': 50, 'C': 100, 'D': 500, 'M': 1000
        }

        total = 0  # Step 2: Initialize result

        # Step 3: Loop through the string
        for i in range(len(s)):
            current_value = roman_map[s[i]]

            # Step 4: Check if the next character is larger
            if i + 1 < len(s) and current_value < roman_map[s[i + 1]]:
                total -= current_value
            else:
                total += current_value

        return total  # Step 5: Return the result
```

"As I’m writing this code, I’m making sure that `i + 1 < len(s)` is used to avoid index out-of-range errors when accessing `s[i + 1]`. This helps us safely check for subtractive cases."

---

#### JavaScript Version

"And here’s a JavaScript version as well:"

```javascript
var romanToInt = function(s) {
    const romanMap = {
        'I': 1, 'V': 5, 'X': 10,
        'L': 50, 'C': 100, 'D': 500, 'M': 1000
    };

    let total = 0;

    for (let i = 0; i < s.length; i++) {
        const current = romanMap[s[i]];
        const next = romanMap[s[i + 1]];

        if (next > current) {
            total -= current;
        } else {
            total += current;
        }
    }

    return total;
};
```

"This version follows the same logic—comparing the current and next values and applying subtraction only when needed."

---

### ⏱️ 5. Discuss Time and Space Complexity

"The time complexity is O(n), where n is the length of the input string, since we go through the string once."

"The space complexity is O(1), because the map of Roman numerals is fixed in size, regardless of input."

---

### 💭 6. Mention Follow-up Questions

"Some potential follow-up questions could include:"

* "What if the input string contains invalid Roman numerals?"
* "Can we modify this solution to convert integers to Roman numerals instead?"
* "Can we implement a class that supports both directions?"

#
#
#

### 🎯 Real-World Applications｜實際應用場景


### 🧾 Text-to-Number Conversion in Legacy Systems｜舊系統中的文字數字轉換

**English**:
In many legacy financial or governmental systems, values may be represented using Roman numerals or other symbolic formats. Understanding how to parse and interpret these can be crucial when migrating data or building interpreters for old document formats.

**中文**：
在許多舊有的財務或政府系統中，數值可能會用羅馬數字或其他符號格式表示。在資料遷移或建構舊檔案格式的解譯器時，能夠正確解析這些數字格式是很重要的能力。

---

### 🏛️ Museum or Historical Archive Software｜博物館或歷史文獻系統

**English**:
When creating digital systems for museums or historical archives, Roman numerals often appear in document dates, monarch generations (e.g., Louis XIV), or volume numbers. Engineers may need to write parsers to convert these into standard integers for indexing or sorting.

**中文**：
在為博物館或歷史資料庫建立數位系統時，羅馬數字常出現在文獻的年份、君主的代號（如路易十四）、或卷冊編號中。工程師可能需要撰寫解析器，將這些轉換為整數，方便搜尋與排序。

---

### 📚 Natural Language Processing (NLP) Preprocessing｜自然語言處理前處理階段

**English**:
Roman numerals appear frequently in book titles, outlines, or numbered sections (e.g., "Chapter IX"). In NLP pipelines, it’s important to normalize such symbols into numbers to improve downstream tasks like classification or entity recognition.

**中文**：
羅馬數字常出現在書籍標題、大綱或章節編號中（例如 “Chapter IX”）。在自然語言處理的前處理流程中，將這些符號標準化成數字有助於後續的分類、命名實體辨識等任務。

---

### 🕹️ Game Development & Puzzles｜遊戲開發與數字謎題

**English**:
Some games use Roman numerals as part of the user interface (e.g., Final Fantasy I–XVI, or power levels). Developers might need to implement a converter to display or interpret such values dynamically.

**中文**：
某些遊戲會使用羅馬數字作為界面的一部分（例如《Final Fantasy I–XVI》或角色等級）。開發者可能需要設計轉換器，將數值動態地轉換並顯示為羅馬數字，或反之。

---

### 🔐 Document Format Parsers & Interpreters｜文件格式解析與解譯

**English**:
PDF generators or LaTeX interpreters often allow Roman numerals as a format option for footnotes or page numbers. When parsing such documents or implementing converters, being able to interpret Roman numerals is a required task.

**中文**：
像 PDF 生成器或 LaTeX 編譯器，通常支援用羅馬數字來標示頁碼或註解。在解析這類文件或開發轉換工具時，能正確處理羅馬數字是一項基本需求。
