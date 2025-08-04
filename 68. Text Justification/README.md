

# 📄 LeetCode 68 - Text Justification


#### 🌐 English Version - UMPIRE Method


### 🔍 U — Understand the Problem

You are given a list of words and a maxWidth value. Format the text such that:

* Each line contains as many words as possible
* Each line is exactly `maxWidth` characters long
* Lines (except the last one) must be **fully justified**:

  * Words are separated by evenly distributed spaces
  * Extra spaces are added to the **leftmost** gaps if not divisible
* The last line is **left-justified**, and no extra space is inserted between words

---

### 🧪 M — Match Examples

#### Example 1

```txt
Input:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16

Output:
[
  "This    is    an",
  "example  of text",
  "justification.  "
]
```

#### Example 2

```txt
Input:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16

Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
```

---

### 🛠️ P — Plan

1. Use a list `line` to collect words for the current line
2. Once adding a word would overflow `maxWidth`, format the current line:

   * If only one word, left-justify and pad with spaces
   * If multiple words, distribute spaces:

     * Compute evenly divided spaces
     * Add extra spaces to the left
3. Repeat for all lines
4. Handle the **last line** separately (left-justified)

---

### 💻 I — Implement

#### 🐍 Python

```python
from typing import List

class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        res = []              # Stores the final justified lines
        line = []             # Current line words
        num_of_letters = 0    # Total characters in the line (excluding spaces)

        for word in words:
            # Check if adding this word would exceed maxWidth
            if num_of_letters + len(word) + len(line) > maxWidth:
                spaces = maxWidth - num_of_letters

                if len(line) == 1:
                    # Only one word → pad spaces at end
                    res.append(line[0] + ' ' * spaces)
                else:
                    # Distribute spaces evenly
                    space_between = spaces // (len(line) - 1)
                    extra = spaces % (len(line) - 1)

                    # Add extra space to leftmost gaps
                    for i in range(extra):
                        line[i] += ' '

                    # Join words with calculated spaces
                    res.append((' ' * space_between).join(line))

                # Reset for next line
                line = []
                num_of_letters = 0

            line.append(word)
            num_of_letters += len(word)

        # Handle the last line (left-justified)
        last_line = ' '.join(line)
        res.append(last_line + ' ' * (maxWidth - len(last_line)))

        return res
```

---

#### 🌐 JavaScript

```javascript
var fullJustify = function(words, maxWidth) {
    const res = [];
    let line = [], lineLength = 0;

    for (let word of words) {
        if (lineLength + word.length + line.length > maxWidth) {
            const spaces = maxWidth - lineLength;

            if (line.length === 1) {
                res.push(line[0] + ' '.repeat(spaces));
            } else {
                const spaceBetween = Math.floor(spaces / (line.length - 1));
                let extra = spaces % (line.length - 1);

                for (let i = 0; i < extra; i++) {
                    line[i] += ' ';
                }

                res.push(line.join(' '.repeat(spaceBetween)));
            }

            line = [];
            lineLength = 0;
        }

        line.push(word);
        lineLength += word.length;
    }

    const lastLine = line.join(' ');
    res.push(lastLine + ' '.repeat(maxWidth - lastLine.length));

    return res;
};
```

---

### 🔎 R — Review

Test on:

* A single long word
* Words that exactly fill maxWidth
* One word per line
* Spaces that don’t divide evenly
* Final line with multiple words

---

### 📊 E — Evaluate

| Type             | Complexity                       |
| ---------------- | -------------------------------- |
| Time Complexity  | O(n), where n = total characters |
| Space Complexity | O(n) for output list             |

---

### 🧠 Solution Build-up｜解法建構思路

1. Start by collecting words in a `line` buffer until the next word would overflow
2. Once full, calculate how many spaces to insert:

   * Evenly divide space
   * Extra spaces go to the leftmost gaps
3. Use `.join()` to format
4. Reset buffer and move to next line
5. At the end, format the last line with left justification

#
#
#

#### 📘 中文版 - UMPIRE 方法

### 🔍 U — 理解問題

* 給你一個單字列表 `words` 和一個整數 `maxWidth`
* 每行需要剛好對齊到 `maxWidth` 寬度
* 單字之間的空格必須平均分配（左右對齊）
* 若不能平均 → 左邊優先多加一格空格
* **最後一行** 要左對齊

---

### 🧪 M — 範例匹配

#### 範例

```txt
Input:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16

Output:
[
  "This    is    an",
  "example  of text",
  "justification.  "
]
```

---

### 🛠️ P — 解題計畫

1. 使用 `line` 收集單字，只要不超出行寬就繼續加
2. 一旦超出，就進行對齊：

   * 只有一個單字 → 直接右補空格
   * 多個單字 → 平均分配空格，左側優先補
3. 重複直到所有單字處理完
4. 最後一行要單獨處理 → 左對齊

---

### 💻 I — 實作（請參考英文段落）

---

### 🔎 R — 檢查與測試

* 多個單字剛好塞滿
* 空格無法平均分配
* 只有一個單字的行
* 最後一行只有一個或多個單字
* 空字串處理

---

### 📊 E — 效能分析

| 類別    | 複雜度  |
| ----- | ---- |
| 時間複雜度 | O(n) |
| 空間複雜度 | O(n) |

---

### 🧠 解法建構邏輯

1. 先思考如何將單字依序放進一行，不超出行寬
2. 一旦塞不下，處理目前行的排版（空格平均補、左側多補）
3. 特殊情況：若只有一個單字 → 靠左補空格
4. 最後一行不做對齊 → 只左對齊再補空格至 `maxWidth`
5. 過程中維持 `line` 與 `num_of_letters` 狀態即可完成整體排版邏輯

#
#
#

# 💡 Solution Build-up｜解法建構思路


#### 🧱 Step 1: 從問題中提煉出核心任務

* 將一連串單字 `words` 分成「剛好不超過 `maxWidth`」的一行行
* 每行需做到左右對齊
* 最後一行則左對齊即可

---

#### 🧠 Step 2: 先處理「分行」的邏輯

> 將單字依序加入 `line` 暫存行中，並累加字母數（不含空格）
> 每次判斷是否加入 `word` 後，行內字母數 + 最少空格數 `len(line)` 是否超出 `maxWidth`

```python
if num_of_letters + len(word) + len(line) > maxWidth:
```

這一行代表「如果再加入 `word`，行寬就超出，就先把這行處理掉」

---

#### 🎨 Step 3: 處理這一行的排版（Justify）

* 若這一行只有一個單字 → 直接靠左補空格
* 若有多個單字 → 計算該行還剩多少空格空間

  ```python
  spaces = maxWidth - num_of_letters
  ```
* 接著：

  * 每個間隔分配基本空格數
  * 多餘空格分給左邊
  * 再用 `.join()` 合併每個單字和空格

這一步是這題邏輯的精髓之一

---

#### 🔁 Step 4: 處理完一行後，記得重置暫存變數

> 把 `line` 和 `num_of_letters` 清空，準備開始下一行的收集

```python
line = []
num_of_letters = 0
```

---

#### 🏁 Step 5: 處理最後一行（left-justified）

因為最後一行不會觸發超出 `maxWidth` 的條件，所以要在最後另外補處理

```python
last_line = ' '.join(line)
res.append(last_line + ' ' * (maxWidth - len(last_line)))
```

* 單字之間只補一個空格
* 剩下的空格統一補在右側

---

#### 📌 邏輯流程圖摘要：

```text
1. 初始化 res, line, num_of_letters

2. 對每個單字：
    - 如果加入 word 後會超出 maxWidth：
        → 先處理目前這一行：
            - 若只有一個字 → 補空格在右邊
            - 若多個字 → 計算基本空格、平均補，左側多補
        → 將格式化好的行加到結果
        → 清空 line 與字母總數
    - 然後將 word 加入新行

3. 處理最後一行：左對齊，尾部補空格

4. 回傳 res
```

#
#
#


# 🎤 Full Spoken-Style Interview Answer

**LeetCode 68 – Text Justification**


### 1️⃣ Clarify the Problem

🗣️

> Sure! So, we’re given an array of strings called `words` and an integer `maxWidth`, which represents the maximum number of characters allowed in each line of text.
>
> Our task is to format the text so that each line:
>
> * is exactly `maxWidth` characters long
> * is fully justified, meaning both left and right edges are aligned
>
> We need to:
>
> * put as many words as possible in each line
> * distribute spaces evenly between words
> * and if the number of spaces doesn’t divide evenly, extra spaces go to the **left** first
>
> The **last line** must be left-justified — that is, words separated by a single space, and any remaining space padded on the right.

> The output should be a list of strings, where each string represents one justified line.

---

### 2️⃣ Discuss Edge Cases

🗣️
Here are a few edge cases that I would want to consider:

* If a line contains only **one word**, we must left-align it and pad spaces at the end to reach `maxWidth`
* The **last line** should never be fully justified — it should be **left-justified only**
* If a word itself is longer than `maxWidth`, we might need to assume that all words are valid (as the problem guarantees)
* Input might be exactly divisible, or require uneven spacing, so we need to handle rounding space gaps carefully

---

### 3️⃣ Consider Brute-force and Optimal Approach

🗣️

> My approach will be **greedy**, line-by-line.
> I will scan the list of words and keep adding them to a temporary `line` buffer **until the next word would make the line exceed `maxWidth`**.

> Once I’ve reached that point, I will:
>
> * calculate the number of remaining spaces I can distribute
> * determine how to spread those spaces across the current words

> I’ll then format that line, append it to the result, and reset my buffer for the next line.

> At the end, I’ll format the **last line** separately with left-justification.

---

### 4️⃣ Explain and Implement Optimal Code (Python)

🗣️
Let me walk you through my implementation and explain while I write it:

```python
from typing import List

class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        res = []              # Final result lines
        line = []             # Buffer for words in the current line
        num_of_letters = 0    # Sum of all characters (excluding spaces)

        for word in words:
            # If adding this word plus the minimum required spaces exceeds maxWidth,
            # we justify the current line and reset.
            if num_of_letters + len(word) + len(line) > maxWidth:
                spaces = maxWidth - num_of_letters

                if len(line) == 1:
                    # If only one word, left-align it and pad right
                    res.append(line[0] + ' ' * spaces)
                else:
                    # Calculate space distribution
                    space_between = spaces // (len(line) - 1)
                    extra = spaces % (len(line) - 1)

                    for i in range(extra):
                        line[i] += ' '  # Distribute extra spaces to the left

                    # Join words with the evenly distributed space
                    res.append((' ' * space_between).join(line))

                # Reset line for the next group
                line = []
                num_of_letters = 0

            # Add current word to the line
            line.append(word)
            num_of_letters += len(word)

        # Handle the last line — left-justified
        last_line = ' '.join(line)
        res.append(last_line + ' ' * (maxWidth - len(last_line)))

        return res
```

🗣️

> So in this implementation:
>
> * We build up each line word by word
> * When we can’t fit a word, we justify and flush the line
> * We calculate how many spaces are needed and how to split them
> * The last line is left-justified with `.join()` and padding at the end

---

### 5️⃣ Discuss Time and Space Complexity

🗣️
Let’s talk about complexity:

* **Time complexity** is **O(n)** where `n` is the total number of characters across all words
  → We visit each word once, and do basic processing per word or per line

* **Space complexity** is also **O(n)** for storing the final list of justified lines

We don’t use any additional data structures beyond the output array and some temporary variables.

---

### 6️⃣ Mention Follow-up Questions

🗣️
Some potential follow-up questions that might be asked include:

* **What if we needed to support right-alignment only?**
* **How would this change if we were doing center-alignment?**
* **How would you handle localization (e.g., Chinese/Japanese words without spaces)?**
* **Could we stream this line by line in constant memory instead of returning a list?**

---

🧠

> That’s how I would approach and solve the text justification problem.
> Let me know if you’d like me to extend this for different alignments or edge cases.


