
# 🧩 LeetCode 12 - Integer to Roman



#### ✨ UMPIRE Method (English Version)



### 🔍 U - Understand the Problem

#### Problem Statement:
Convert an integer from 1 to 3999 into its Roman numeral representation.

#### Input:
- An integer `num`, where 1 ≤ num ≤ 3999

#### Output:
- A string representing the Roman numeral equivalent of the integer

---

### 🧠 M - Match to Known Patterns

- This is a **greedy** problem: we aim to subtract the largest possible Roman value from the number repeatedly.
- We use a **mapping table** from values to Roman symbols.
- The mapping must be in **descending order** to apply the greedy approach correctly.

---

### 📝 P - Plan the Solution

1. Create a mapping list of tuples (value, symbol) in descending order.
2. Initialize an empty string `result`.
3. Loop through the mapping list:
   - For each value, compute how many times it fits into the current number.
   - Append the corresponding symbol that many times to the result.
   - Subtract the value * count from the number.
4. Return the final result.

---

### 💻 I - Implement the Solution

#### 🐍 Python

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Roman numeral mapping from largest to smallest
        roman_map = [
            (1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
            (100, "C"), (90, "XC"), (50, "L"), (40, "XL"),
            (10, "X"), (9, "IX"), (5, "V"), (4, "IV"), (1, "I")
        ]

        result = ""

        # Iterate over the mapping list
        for value, symbol in roman_map:
            count = num // value  # How many times this value fits
            result += symbol * count  # Append that many symbols
            num -= value * count  # Subtract the total value used

        return result
````

#### 🌐 JavaScript

```javascript
var intToRoman = function(num) {
    const romanMap = [
        [1000, "M"], [900, "CM"], [500, "D"], [400, "CD"],
        [100, "C"], [90, "XC"], [50, "L"], [40, "XL"],
        [10, "X"], [9, "IX"], [5, "V"], [4, "IV"], [1, "I"]
    ];

    let result = "";

    for (let [value, symbol] of romanMap) {
        let count = Math.floor(num / value);  // Number of times value fits
        result += symbol.repeat(count);       // Repeat symbol that many times
        num -= value * count;                 // Reduce the number
    }

    return result;
};
```

---

### 🔎 R - Review and Optimize

* ✅ Greedy algorithm ensures correct and optimal result
* ✅ No recursion or backtracking required
* ✅ Clean and readable structure
* ⚠️ Be cautious: using unordered `dict` instead of `list` in Python may break this logic

---

### 📊 E - Evaluate the Complexity

* **Time Complexity**: O(1) — Only 13 possible symbols, fixed iterations
* **Space Complexity**: O(1) — Only fixed-size variables used
#
#
#

#### 🈶 UMPIRE 方法（中文版本）



### 🔍 U - 理解題目

#### 題目說明：

將一個整數（1 到 3999）轉換為對應的羅馬數字表示法。

#### 輸入：

* 一個整數 `num`，1 ≤ num ≤ 3999

#### 輸出：

* 對應的羅馬數字字串

---

### 🧠 M - 題型比對

* 這是一道\*\*貪婪法（Greedy）\*\*的模擬題。
* 我們需要建立「整數對應羅馬字母」的對照表。
* 對照表必須從**大到小排序**，才能確保每次都優先減去最大的合法數字。

---

### 📝 P - 解題計畫

1. 建立一個從大到小排序的對照表（tuple list）。
2. 初始化一個空字串 `result`。
3. 走訪對照表：

   * 每次取出 `(value, symbol)`，計算 `value` 在當前數字中可放幾次。
   * 把對應的 `symbol * 次數` 加入結果。
   * 扣掉 `value * 次數`。
4. 回傳最終的字串。

---

### 💻 I - 程式實作

#### 🐍 Python

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # 建立對照表：整數 -> 羅馬符號
        roman_map = [
            (1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
            (100, "C"), (90, "XC"), (50, "L"), (40, "XL"),
            (10, "X"), (9, "IX"), (5, "V"), (4, "IV"), (1, "I")
        ]

        result = ""

        # 逐一嘗試每一種符號
        for value, symbol in roman_map:
            count = num // value  # 此值能使用幾次
            result += symbol * count  # 加入對應羅馬字母
            num -= value * count  # 扣掉處理過的數值

        return result
```

#### 🌐 JavaScript

```javascript
var intToRoman = function(num) {
    const romanMap = [
        [1000, "M"], [900, "CM"], [500, "D"], [400, "CD"],
        [100, "C"], [90, "XC"], [50, "L"], [40, "XL"],
        [10, "X"], [9, "IX"], [5, "V"], [4, "IV"], [1, "I"]
    ];

    let result = "";

    for (let [value, symbol] of romanMap) {
        let count = Math.floor(num / value);  // 該值能用幾次
        result += symbol.repeat(count);       // 重複加上羅馬符號
        num -= value * count;                 // 扣掉已處理部分
    }

    return result;
};
```

---

### 🔎 R - 檢查與優化

* ✅ 使用貪婪策略，每次選最大合法數字
* ✅ 避免過度重複或遞迴
* ✅ 對照表使用有序 tuple list，避免 dict 無序問題

---

### 📊 E - 時間與空間複雜度分析

* **時間複雜度**：O(1)，因為最多就跑 13 種符號
* **空間複雜度**：O(1)，只用固定大小變數

#
#
#


# 🎤 Full Spoken-Style Interview Answer



### 1️⃣ Clarify the Problem

"Sure, just to clarify: we're given an integer input between 1 and 3999, and we need to convert that into its Roman numeral representation as a string.

So for example:

* Input 3 → Output `"III"`
* Input 58 → Output `"LVIII"`
* Input 1994 → Output `"MCMXCIV"`

Am I correct in understanding that the integer will always be within the range 1 to 3999, and we don’t need to worry about invalid input or negative numbers?"

---

### 2️⃣ Discuss Edge Cases

"Here are some edge cases I’m thinking about:

* The smallest valid number is 1 → that should give `"I"`.
* The largest valid number is 3999 → which should convert to `"MMMCMXCIX"`.
* Also, I want to ensure that compound numerals like `900 = CM` or `4 = IV` are handled properly, since Roman numerals use a subtractive notation in these cases."

---

### 3️⃣ Consider Brute-Force and Optimal Approach

"The brute-force approach would be to manually build the Roman numeral digit by digit — possibly checking thousands, hundreds, tens, and units separately — but that might become messy.

Instead, I think we can take a **greedy approach**.
We start from the largest Roman numeral value and subtract as much as possible at each step while appending the corresponding symbol. This works because Roman numerals are built from the largest to the smallest values.

To implement this, we can use a mapping from integers to Roman numeral strings, sorted from largest to smallest."

---

### 4️⃣ Explain and Implement Optimal Code

"Let me walk you through the implementation in Python first."

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Step 1: Create a mapping from integer values to Roman symbols in descending order
        roman_map = [
            (1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
            (100, "C"), (90, "XC"), (50, "L"), (40, "XL"),
            (10, "X"), (9, "IX"), (5, "V"), (4, "IV"), (1, "I")
        ]

        result = ""

        # Step 2: Loop through the mapping and subtract as much as possible
        for value, symbol in roman_map:
            count = num // value  # how many times this symbol fits
            result += symbol * count  # append symbol multiple times
            num -= value * count  # subtract the total from the number

        return result
```

"And here's how the same logic would look in JavaScript:"

```javascript
var intToRoman = function(num) {
    const romanMap = [
        [1000, "M"], [900, "CM"], [500, "D"], [400, "CD"],
        [100, "C"], [90, "XC"], [50, "L"], [40, "XL"],
        [10, "X"], [9, "IX"], [5, "V"], [4, "IV"], [1, "I"]
    ];

    let result = "";

    for (let [value, symbol] of romanMap) {
        let count = Math.floor(num / value);  // how many times it fits
        result += symbol.repeat(count);       // append symbol that many times
        num -= value * count;                 // reduce the number
    }

    return result;
};
```

"As you can see, we iterate over the list, and at each step, we use as much of the current Roman value as we can, repeating the symbol and reducing the number."

---

### 5️⃣ Discuss Time and Space Complexity

"The time complexity here is **O(1)** because the Roman numeral system only uses a fixed set of 13 symbol-value pairs, so our loop runs at most 13 times.

The space complexity is also **O(1)**, since we only use a fixed amount of memory regardless of the input."

---

### 6️⃣ Mention Follow-Up Questions

"Some follow-up questions that could come up are:

* Could we reverse this logic? That is, given a Roman numeral, convert it back into an integer?
* How would we validate whether a Roman numeral string is valid?
* What if the input range is expanded beyond 3999? For example, how would we represent 4000 or higher, since standard Roman numerals don’t go beyond 3999 without special notations?"


#
#
#

## 🎯 Real-World Applications｜實際應用場景



### 💼 Formatting Systems｜格式轉換系統

**EN**:
This problem reflects real-world scenarios where you need to **convert numerical values into legacy formats or display-friendly formats**. For example, formatting IDs, encoding data in Roman numerals for historical documents, or displaying numbers in ancient-themed applications.

**ZH**：
這題模擬了實務中常見的「**數字格式轉換問題**」，例如需要把數字轉成特殊格式或舊系統能識別的格式。實務應用包含歷史文件格式處理、古文明主題的展示介面等。

**Example**:

* Converting chapter numbers in a digital book to Roman numerals: `"Chapter IV"`
* Formatting clock faces with Roman numerals (e.g., `"XII"` for 12)

---

### 🕹️ Game Development & UI｜遊戲開發與用戶介面設計

**EN**:
In video games or mobile apps with historical or fantasy settings, developers often **display level numbers, scores, or ranks using Roman numerals** to match the design theme.

**ZH**：
在遊戲或行動應用程式中，尤其是歷史風格或奇幻主題的設計，開發者常會用羅馬數字顯示關卡、分數或等級，以符合整體美術風格。

**Example**:

* "Level VII" in a medieval fantasy RPG game
* Character rank: `"Rank IX – Grandmaster"`

---

### 📚 Educational Tools｜教育工具與教材內容

**EN**:
Educational platforms may require converting between Arabic and Roman numerals to teach students about ancient numeral systems.

**ZH**：
教育平台或數學教材經常需要教學羅馬數字與阿拉伯數字的轉換，幫助學生理解古代計數法與文化歷史。

**Example**:

* Math practice software that auto-converts `22` to `XXII`
* History apps labeling years like `"MCMXLV"` (1945)

---

### 🧾 Data Normalization or Parsing｜資料清理與解析

**EN**:
Some legacy datasets, especially those in historical archives, genealogy databases, or scanned documents, may store numerals in Roman format. When processing such data, you might need to **convert Roman numerals back and forth** with integers.

**ZH**：
在某些舊資料系統中（如歷史檔案或家譜資料），數字可能是用羅馬數字儲存。處理這類資料時，往往需要能夠**轉換羅馬與整數格式**來進行資料清理或標準化。

**Example**:

* Parsing `"Volume XII"` from scanned PDFs and converting it to `12`
* Normalizing entries like `"Section IX"` to structured JSON data

