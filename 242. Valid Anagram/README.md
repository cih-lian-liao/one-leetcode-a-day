# 🧩 LeetCode 242 — Valid Anagram (UMPIRE, Hash Map Approach)

### 🔍 Understand the Problem

#### Problem

Given two strings `s` and `t`, determine whether `t` is an anagram of `s` (i.e., they contain exactly the same characters with the same frequencies).

#### Inputs / Outputs

* **Input:** strings `s`, `t`
* **Output:** boolean — `true` if an anagram, else `false`

#### Constraints & Observations

* If `len(s) != len(t)`, they **cannot** be anagrams.
* Character order doesn’t matter; **counts** do.
* Typical LeetCode constraints are lowercase a–z, but we’ll write a **general, Unicode-safe** solution using hash maps.

---

### 🧠 Match to Patterns

* **Counting with a hash map (dictionary / Map)**:
  Build frequency counts from `s`, subtract with `t`. If any count dips below zero or a char is missing, return `false`.

  * Time: **O(n)**
  * Space: **O(k)**, where `k` is the number of distinct characters
* **Sorting** is simpler but costs **O(n log n)** — we’ll prefer counting.

---

### 🗺️ Plan the Solution

1. If lengths differ → return `false`.
2. Iterate `s`, increment each character’s count in a hash map.
3. Iterate `t`, decrement each character’s count:

   * If a character doesn’t exist in the map → return `false`.
   * If a count becomes negative → return `false`.
4. If we finish without early returns, they’re anagrams → return `true`.

---

### 🛠️ Implement

#### 🐍 Python — Hash Map (Unicode-friendly) with step-by-step comments

```python
def isAnagram(s: str, t: str) -> bool:
    # 1) Quick length check — different lengths cannot be anagrams
    if len(s) != len(t):
        return False

    # 2) Frequency table using a plain dict
    freq = {}

    # Count characters from s
    for ch in s:
        # dict.get(ch, 0) returns current count if present; else 0
        freq[ch] = freq.get(ch, 0) + 1

    # 3) Subtract counts using characters from t
    for ch in t:
        # If ch never appeared in s, t has an extra character → not an anagram
        if ch not in freq:
            return False
        freq[ch] -= 1
        # If any count dips below zero, t has too many of that character
        if freq[ch] < 0:
            return False

    # 4) All counts are balanced to zero by construction — an anagram
    return True
```

#### 🌐 JavaScript — Map (Unicode-friendly) with step-by-step comments

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
  // 1) Quick length check
  if (s.length !== t.length) return false;

  // 2) Frequency table using Map
  const freq = new Map();

  // Build counts from s
  for (const ch of s) {
    // If ch exists, add 1; otherwise start from 0
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }

  // 3) Subtract using t
  for (const ch of t) {
    // If ch wasn't counted before, t has an extra char
    if (!freq.has(ch)) return false;

    const next = freq.get(ch) - 1;

    // If count would go negative, t has too many of this char
    if (next < 0) return false;

    // Keep map tidy: remove when count reaches 0; otherwise update
    if (next === 0) freq.delete(ch);
    else freq.set(ch, next);
  }

  // 4) If all counts are balanced, map is empty → an anagram
  return freq.size === 0;
};
```

---

### ✅ Review & Test

* Happy paths:

  * `("anagram","nagaram") → true`
  * `("a","a") → true`
* Negative:

  * `("rat","car") → false`
  * `("aa","a") → false`
* Edge cases:

  * `("", "") → true` (both empty)
  * Unicode-safe examples:

    * `("é", "é")` → might require normalization in real products (see notes)
    * `("äb", "bä") → true`

---

### ⏱️ Evaluate (Complexity & Trade-offs)

* **Time:** O(n) — single pass to count, single pass to subtract.
* **Space:** O(k) — distinct characters stored.
* For strict lowercase a–z, space can be treated as O(1) with a fixed-size array, but hash map is more general.

---

### 📎 Additional Notes (Practical Pitfalls & Unicode)

* **Normalization matters in real apps:** visually identical strings can be different sequences (`é` vs `e` + combining accent). Consider NFC normalization:

  * Python: `import unicodedata; s = unicodedata.normalize("NFC", s)`
  * JS: `s = s.normalize("NFC")`
* **Case-insensitive anagrams?** Use robust case folding (Python `str.casefold()`; JS typically `toLowerCase()` or `toLocaleLowerCase()`).
* **Early exits** reduce work: length check up front; negative count → return `false`.
* **Map tidy-up** (JS) is optional; it helps conceptual clarity and potential memory savings.
* **Do not assume a–z** unless guaranteed by constraints; otherwise prefer the hash map solution you implemented here.

<br>

# 🎯 LeetCode 242 — 有效的變位詞（UMPIRE，Hash Map 解法）

### 🔎 理解題目

#### 題意

給定兩個字串 `s` 與 `t`，若 `t` 是 `s` 的變位詞（字元種類與每個字元的出現次數完全相同），回傳 `true`，否則回傳 `false`。

#### 輸入 / 輸出

* **輸入：** 字串 `s`, `t`
* **輸出：** 布林值 — 是否為變位詞

#### 重點與觀察

* 若 `len(s) != len(t)` → 一定不是變位詞。
* 重點在於「**字元頻率**」是否一致，順序無關。
* 以 **Hash Map（字典 / Map）** 計數可支援一般字元與 Unicode。

---

### 🧭 對應模式

* **Hash Map 計數法：**
  先對 `s` 計數，再用 `t` 扣減。若遇到「字元不存在」或「頻率變負」，立即回傳 `false`。

  * 時間：**O(n)**
  * 空間：**O(k)**（不同字元的種類數）
* **排序** 雖直覺但成本 **O(n log n)**，本題更建議計數法。

---

### 🧮 規劃解法

1. 長度不同 → 直接 `false`。
2. 走訪 `s`，建立字元→次數的頻率表。
3. 走訪 `t`，對應字元的頻率遞減：

   * 若該字元不存在 → `false`
   * 若頻率變成負數 → `false`
4. 全部扣完沒有提前結束 → 代表頻率平衡 → `true`。

---

### 🧰 程式實作

#### 🐼 Python — Hash Map（支援通用字元）含詳細註解

```python
def isAnagram(s: str, t: str) -> bool:
    # 1) 長度不同，不可能是變位詞
    if len(s) != len(t):
        return False

    # 2) 使用 dict 建立頻率表
    freq = {}

    # 對 s 的每個字元計數
    for ch in s:
        # 若 ch 已存在，取出原本的次數並 +1；不存在則從 0 開始 +1
        freq[ch] = freq.get(ch, 0) + 1

    # 3) 對 t 的每個字元做扣減
    for ch in t:
        # 若 ch 在 freq 中不存在，代表 t 多了某個字元
        if ch not in freq:
            return False
        freq[ch] -= 1
        # 扣到負數代表 t 中該字元過多
        if freq[ch] < 0:
            return False

    # 4) 走到這裡代表所有字元剛好平衡（頻率歸零）
    return True
```

#### 🪄 JavaScript — Map（支援通用字元）含詳細註解

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
  // 1) 長度檢查
  if (s.length !== t.length) return false;

  // 2) 以 Map 建立頻率表
  const freq = new Map();

  // 對 s 計數
  for (const ch of s) {
    // 如果 ch 沒出現過，freq.get(ch) 會是 undefined，所以用 || 0 轉成 0
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }

  // 3) 對 t 扣減
  for (const ch of t) {
    // 若 ch 不存在，表示 t 多了一個字元
    if (!freq.has(ch)) return false;

    const next = freq.get(ch) - 1;

    // 次數若變負數，表示 t 中該字元過多
    if (next < 0) return false;

    // 變 0 就移除，保持 map 精簡；否則更新
    if (next === 0) freq.delete(ch);
    else freq.set(ch, next);
  }

  // 4) 若全部抵銷，Map 會清空，表示兩者為變位詞
  return freq.size === 0;
};
```

---

### 🔬 測試與檢視

* 正向案例：

  * `("anagram","nagaram") → true`
  * `("a","a") → true`
* 反例：

  * `("rat","car") → false`
  * `("aa","a") → false`
* 邊界情況：

  * `("","") → true`
  * Unicode 例子（產品中可考慮正規化）：

    * `("é","é")`、`("äb","bä")`

---

### ⚖️ 複雜度評估

* **時間複雜度：** O(n)
* **空間複雜度：** O(k)（不同字元的種類數）
* 若題目保證僅 a–z，可用固定陣列優化空間為近似 O(1)；但 Hash Map 更通用。

---

### 🧷 附加筆記（實務注意事項）

* **Unicode 正規化：** 視覺相同不代表位元序列相同（例：`é` vs `e` + 結合符）。
  產品中可考慮 NFC 正規化：

  * Python：`unicodedata.normalize("NFC", s)`
  * JS：`s.normalize("NFC")`
* **大小寫不敏感：**

  * Python 推薦 `casefold()`（比 `lower()` 更完整）；
  * JS 可用 `toLowerCase()` / `toLocaleLowerCase()`（需注意語言特例）。
* **提早返回：**

  * 長度先檢查；
  * 遇到扣減變負數立即 `false`，節省時間。
* **只限 a–z？** 若題目未明確限制，**不要**假設只有 a–z，使用 Hash Map 更安全。
* **Map 清理策略（JS）：** 計數歸零就刪除，有助理解與記憶體管理，但非必要。
* **測試覆蓋：**

  * 空字串、單一字元、重複字元、完全不同字元、Unicode（重音、emoji）等情況都要測。

<br>

# 🎤 Full Spoken-Style Interview Answer


### 1️⃣ Clarify the problem and read the provided examples and constraints

**You can say:**
“Okay, so the problem is asking me to determine if two given strings are anagrams of each other. That means both strings need to contain exactly the same characters, with the same frequency of each character, but the order of the characters can be different.

For example, if the first string is `anagram` and the second string is `nagaram`, the output should be true, because even though the order is different, both contain the same letters with the same counts. But if I take `rat` and `car`, the output should be false, because one has a `t` and the other has a `c`.

From what I understand, the inputs are just two strings. Typically, the constraints in LeetCode say lowercase English letters, but in real-world cases, it could be Unicode. For now, I’ll focus on the lowercase case, but I’ll also think about how to adapt it.”

---

### 2️⃣ Discuss edge cases

**You can say:**
“Before I jump into a solution, I’d like to think about some edge cases:

* If the two strings have different lengths, they can’t possibly be anagrams, so I can return false immediately.
* If both strings are empty, then technically, yes, they are anagrams of each other.
* If one string has a character that doesn’t exist in the other, it’s automatically false.
* If there are repeated characters, for example `aab` and `aba`, that should still be true. So my solution needs to check not just the presence of characters, but also their frequencies.

And, if we’re dealing with Unicode or accented characters like `é` versus `e + ́`, normalization might matter, but for the core solution I’ll ignore that until the follow-up.”

---

### 3️⃣ Consider brute-force and optimal approach

**You can say:**
“A brute-force solution would be to generate all permutations of one string and see if the other string exists in that set. But that’s factorial time complexity, which is completely infeasible for anything more than very short strings.

Another simple approach would be sorting. I could sort both strings and compare if the sorted versions are equal. That would take O(n log n) time because of sorting, and O(n) extra space depending on the sorting algorithm.

But I know there’s a more optimal approach, which is to use counting. Specifically, I can use a hash map (or dictionary) to count the frequency of each character in the first string, then subtract the frequency while iterating over the second string. If everything balances out perfectly, they are anagrams; otherwise, they’re not. This gives me O(n) time and O(k) space, where k is the number of distinct characters.”

---

### 4️⃣ Explain and implement optimal code

**You can say while writing code in Python first:**
“So let me implement the counting approach. I’ll start by checking the length: if they’re different, return false. Then I’ll build a frequency map for the first string. For every character, I’ll increase the count. Then I’ll iterate over the second string and decrease the count. If a character isn’t in the map or if the count goes negative, I can immediately return false. Finally, if I finish both loops without problems, then the strings are anagrams.

Here’s how it looks in Python:”

```python
def isAnagram(s: str, t: str) -> bool:
    # Step 1: Length check
    if len(s) != len(t):
        return False

    # Step 2: Build frequency dictionary for s
    freq = {}
    for ch in s:
        freq[ch] = freq.get(ch, 0) + 1

    # Step 3: Subtract with t
    for ch in t:
        if ch not in freq:
            return False
        freq[ch] -= 1
        if freq[ch] < 0:
            return False

    # Step 4: If everything balanced, return true
    return True
```

**Now you can say in JavaScript:**
“And if I were to write the same logic in JavaScript, I’d use a Map object to store the frequencies. Here’s how it looks:”

```javascript
var isAnagram = function(s, t) {
  if (s.length !== t.length) return false;

  const freq = new Map();

  // Count characters in s
  for (const ch of s) {
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }

  // Subtract with t
  for (const ch of t) {
    if (!freq.has(ch)) return false;

    const next = freq.get(ch) - 1;
    if (next < 0) return false;

    if (next === 0) freq.delete(ch);
    else freq.set(ch, next);
  }

  return freq.size === 0;
};
```

---

### 5️⃣ Discuss time/space complexity

**You can say:**
“In terms of complexity, this algorithm runs in O(n) time, because I just do one pass over s and one pass over t, which is linear. The space complexity depends on the alphabet size. For English lowercase letters, it’s O(1), because the maximum is just 26. But if we allow Unicode, then it’s O(k), where k is the number of distinct characters in the strings.

Compared to the sorting solution, which is O(n log n), this is more efficient and scales better.”

---

### 6️⃣ Mention follow-up questions

**You can say:**
“One natural follow-up question is: what if the strings contain Unicode characters, including accented letters or emoji? In that case, I’d recommend normalizing the strings first, for example using NFC normalization, so that visually identical characters are treated as equal. After normalization, the same hash map counting approach works.

Another follow-up could be: what if we want the comparison to be case-insensitive? In that case, I would apply case folding (in Python I’d use `casefold()`, in JavaScript I’d use `toLowerCase()` or `toLocaleLowerCase()`).

And finally, if the strings are extremely large, early exits on length mismatch or negative counts are important to save time.”

<br>

## 🎯 Real-World Applications｜實際應用場景

---

### 📝 Text & Document Processing｜文字與文件處理

**EN:**
Checking if two words are anagrams is similar to tasks in **text normalization** and **document comparison**. For example, when building a plagiarism detection tool, search engine, or spell-checker, we might need to check whether two strings contain the same underlying set of characters or if content is just reordered.

**中文：**
判斷兩個字是否為變位詞，其實和 **文字正規化**、**文件比對** 很類似。例如，在建立防抄襲工具、搜尋引擎或拼字檢查器時，系統可能需要檢查兩個字串是否包含相同的基本字元，或者只是順序不同。

---

### 🔐 Security & Cryptography｜安全與加密

**EN:**
Anagram-like checks are used in **hashing, encryption, and password validations**. For example, ensuring that password rules require a certain set of characters, or validating that a cipher text preserves character distribution, both rely on counting character frequencies.

**中文：**
在 **雜湊、加密與密碼驗證** 相關領域，也會用到類似的概念。例如：確保密碼符合一定的字元分布規則，或者檢查密文是否保有原始字元的統計特徵，這些都需要計算字元頻率。

---

### 🔍 Data Deduplication & Integrity｜資料去重與完整性檢查

**EN:**
When maintaining large datasets, sometimes two entries may look different but actually represent the same entity with reordered or scrambled fields. By comparing character counts, systems can detect duplicates or validate data integrity.

**中文：**
在管理大型資料集時，有時候兩筆資料看似不同，但其實只是欄位順序不同或內容被打亂。透過比較字元頻率，可以協助檢測重複資料或驗證資料完整性。

---

### 💬 Natural Language Processing (NLP)｜自然語言處理

**EN:**
In NLP, character frequency analysis is a building block for **language modeling, spam filtering, and authorship attribution**. Anagram checking is a simplified example of comparing character distributions, which is crucial in detecting stylistic similarity or anomalies.

**中文：**
在 NLP 中，字元頻率分析是 **語言建模、垃圾郵件過濾、作者風格辨識** 的基礎。判斷變位詞其實就是一個簡化版的「字元分布比對」，而這種分布比較在檢測文字風格相似度或異常行為時非常重要。

---

### 🎮 Gaming & Puzzle Validation｜遊戲與謎題驗證

**EN:**
Many word games (like Scrabble, Boggle, or crossword helpers) need to check if a word can be formed from a given set of letters. This is essentially an anagram validation problem: do the letters match with the required frequencies?

**中文：**
許多文字遊戲（例如 Scrabble 拼字遊戲、Boggle 字母拼圖或填字遊戲）都需要檢查某個單字能否由一組字母組成。這本質上就是一個變位詞驗證問題：字母與次數是否匹配？

---

### 🛒 E-commerce Search & Recommendations｜電商搜尋與推薦

**EN:**
In e-commerce platforms, search engines may want to match products even when users type queries with jumbled characters, typos, or different word orders. Comparing character counts can be a first step to detect “fuzzy matches.”

**中文：**
在電商平台中，搜尋引擎可能需要對應使用者輸入的亂序關鍵字、拼字錯誤或不同順序的字詞。比較字元次數分布，可以作為偵測「模糊匹配」的第一步。


