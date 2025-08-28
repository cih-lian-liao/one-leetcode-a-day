# 🌟 LeetCode 290 — Word Pattern｜UMPIRE Notes

### 🧩 English — UMPIRE (Plan A: Two-Map Bijection)

#### 🧭 Understand (U)

* We’re given a pattern string `pattern` (e.g., `"abba"`) and a space-separated string `s` (e.g., `"dog cat cat dog"`).
* We must verify a **bijection** between:

  * each **character** in `pattern` ↔ each **word** in `s`.
* Necessary conditions:

  * Same length: `len(pattern) == number_of_words`.
  * One-to-one mapping both ways (no char maps to two words; no word maps to two chars).

#### 🔍 Match (M)

* This is a classic **bijection / isomorphism** check (akin to LC 205: Isomorphic Strings).
* Idiomatic approach in interviews: **two hash maps** (char→word, word→char) to enforce consistency in both directions.

#### 📝 Plan (P)

1. Split `s` by spaces into `words`.
2. If `len(pattern) != len(words)`, immediately return `False`.
3. Create two maps:

   * `p2w`: pattern char → word
   * `w2p`: word → pattern char
4. Iterate index `i` over the pairs `(pattern[i], words[i])`:

   * If `pattern[i]` already in `p2w`, it **must** equal `words[i]`, else `False`.
   * If `words[i]` already in `w2p`, it **must** equal `pattern[i]`, else `False`.
   * Otherwise, set both mappings.
5. If the loop finishes without conflict, return `True`.

#### 🛠️ Implement (I)

##### 🐍 Python

```python
def wordPattern(pattern: str, s: str) -> bool:
    """
    Check if 's' follows the same pattern as 'pattern' via a bijection:
    - Each pattern character maps to exactly one word
    - Each word maps back to exactly one pattern character
    Time:  O(n) where n = len(pattern) = number of words
    Space: O(k) for distinct chars/words
    """
    # 1) Split 's' into words by single spaces per problem statement
    words = s.split()

    # 2) Quick length check: different lengths can never match
    if len(pattern) != len(words):
        return False

    # 3) Two dictionaries to enforce bijection
    p2w = {}  # pattern char -> word
    w2p = {}  # word -> pattern char

    # 4) Walk both sequences in lockstep
    for ch, w in zip(pattern, words):
        # If ch was seen before, it must map to the same word 'w'
        if ch in p2w:
            if p2w[ch] != w:
                # Contradiction: same char maps to a different word
                return False
        # If w was seen before, it must map back to the same char 'ch'
        if w in w2p:
            if w2p[w] != ch:
                # Contradiction: same word maps to a different char
                return False

        # Establish mappings (idempotent if they already match)
        p2w[ch] = w
        w2p[w] = ch

    # 5) No conflicts: it's a valid bijection
    return True
```

##### 🟨 JavaScript

```javascript
/**
 * Verify bijection between pattern characters and words in s.
 * Time:  O(n)
 * Space: O(k)
 * @param {string} pattern
 * @param {string} s
 * @return {boolean}
 */
var wordPattern = function(pattern, s) {
  // 1) Split by spaces (problem guarantees single spaces)
  const words = s.split(' ');

  // 2) Lengths must match exactly
  if (pattern.length !== words.length) return false;

  // 3) Two maps to enforce one-to-one mapping
  const p2w = new Map(); // pattern char -> word
  const w2p = new Map(); // word -> pattern char

  // 4) Iterate in lockstep
  for (let i = 0; i < pattern.length; i++) {
    const ch = pattern[i];
    const w  = words[i];

    // If we've mapped 'ch' before, it must map to the same 'w'
    if (p2w.has(ch) && p2w.get(ch) !== w) return false;

    // If we've mapped 'w' before, it must map back to the same 'ch'
    if (w2p.has(w) && w2p.get(w) !== ch) return false;

    // Establish both directions (safe if identical mapping repeats)
    p2w.set(ch, w);
    w2p.set(w, ch);
  }

  // 5) All constraints satisfied
  return true;
};
```

#### 🧪 Review (R)

##### ⏱️ Time & Space

* **Time:** `O(n)` (single pass).
* **Space:** `O(k)` for distinct chars/words stored in the maps.

##### 🧮 Quick Tests

* `("abba", "dog cat cat dog")` → `True`
* `("abba", "dog cat cat fish")` → `False`
* `("aaaa", "dog cat cat dog")` → `False`
* `("abba", "dog dog dog dog")` → `False`

#### 🔄 Evaluate (E)

* **Early exit** on any contradiction to save time.
* **Edge cases:** empty inputs (both empty → `True`), case sensitivity (`Dog` ≠ `dog`), extra/missing words (caught by length check).
* **Robustness:** if input might contain multiple spaces, normalize first (e.g., split on regex), though LC290 uses single spaces.

---

### 🧠 中文 — UMPIRE（Plan A：雙哈希表雙射）

#### 🎯 理解（U）

* 給定模式字串 `pattern`（如 `"abba"`）與以空白分隔的字串 `s`（如 `"dog cat cat dog"`）。
* 檢查是否存在**雙射**：

  * `pattern` 的每個字元 ↔ `s` 的每個單字一一對應且互相唯一。

#### 🧷 匹配（M）

* 經典**雙向映射 / 同構**檢查（類似 LC205）。
* 面試常用：**兩個映射表**（字元→單字、單字→字元）同步約束，避免單向漏網。

#### 🗺️ 規劃（P）

1. 以空白分割 `s` 得到 `words`。
2. 若 `len(pattern) != len(words)`，立即回傳 `False`。
3. 建立：

   * `p2w`：字元→單字
   * `w2p`：單字→字元
4. 逐一配對 `(pattern[i], words[i])`：

   * 若字元已對應，必須等於當前單字，否則 `False`。
   * 若單字已對應，必須等於當前字元，否則 `False`。
   * 否則同時建立兩邊對應。
5. 全程無衝突則 `True`。

#### 🔧 實作（I）

##### 🐼 Python

```python
def wordPattern(pattern: str, s: str) -> bool:
    """
    驗證 pattern 與 s 是否為雙射關係：
    - 每個字元對應唯一單字
    - 每個單字回對應唯一字元
    時間 O(n)，空間 O(k)
    """
    # 1) 以空白切割字串 s，取得單字序列
    words = s.split()

    # 2) 長度不一致，必不可能匹配
    if len(pattern) != len(words):
        return False

    # 3) 兩個字典維持雙向一一對應
    p2w = {}  # 字元 -> 單字
    w2p = {}  # 單字 -> 字元

    # 4) 對位走訪
    for ch, w in zip(pattern, words):
        # 既有對應需一致：字元 -> 單字
        if ch in p2w and p2w[ch] != w:
            return False
        # 既有對應需一致：單字 -> 字元
        if w in w2p and w2p[w] != ch:
            return False

        # 建立雙向對應（若同樣對應重複出現不影響）
        p2w[ch] = w
        w2p[w] = ch

    # 5) 無矛盾即為有效
    return True
```

##### 🐣 JavaScript

```javascript
/**
 * 驗證 pattern 與 s 是否形成雙射。
 * 時間 O(n)，空間 O(k)
 * @param {string} pattern
 * @param {string} s
 * @return {boolean}
 */
var wordPattern = function(pattern, s) {
  // 1) 題目保證以單一空白分隔
  const words = s.split(' ');

  // 2) 長度先行篩掉不可能情況
  if (pattern.length !== words.length) return false;

  // 3) 建立雙向映射
  const p2w = new Map(); // 字元 -> 單字
  const w2p = new Map(); // 單字 -> 字元

  // 4) 逐一檢查一致性
  for (let i = 0; i < pattern.length; i++) {
    const ch = pattern[i];
    const w  = words[i];

    if (p2w.has(ch) && p2w.get(ch) !== w) return false;
    if (w2p.has(w) && w2p.get(w) !== ch) return false;

    p2w.set(ch, w);
    w2p.set(w, ch);
  }

  // 5) 全程無衝突即通過
  return true;
};
```

#### 🪄 檢視（R）

##### ⌛ 複雜度

* **時間：** `O(n)` 單次遍歷。
* **空間：** `O(k)` 儲存不重複字元/單字之映射。

##### 🧰 快速測試

* `("abba", "dog cat cat dog")` → `True`
* `("abba", "dog cat cat fish")` → `False`
* `("aaaa", "dog cat cat dog")` → `False`
* `("abba", "dog dog dog dog")` → `False`

#### 🧼 反思（E）

* **及早返回**：一旦發現衝突，立即 `False`，避免無謂運算。
* **邊界**：空輸入情況（兩者皆空 → `True`）、大小寫敏感、額外或不足的單字（長度檢查攔截）。
* **健壯性**：若實務資料存在多空白/雜訊，可先正規化（修剪、合併空白）。

---

### 📌 Extra Notes / 附加筆記（Coding Pitfalls & Tips）

* **Don’t use a single map only：** 只做「字元→單字」會漏掉「不同字元→同一單字」的非法情況；務必**雙向**約束。
* **Length check first：** 先比長度可立刻剪枝大量不匹配案例。
* **Be careful with splitting：** 題目是單一空白；若換題或資料來源不保證，請用更強健的切分（如正則 `/\s+/`）。
* **Idempotent mapping：** 同一對應重複出現屬於一致狀態，不要過度判斷；只有**衝突**時才回傳 `False`。
* **Case sensitivity & normalization：** 視需求先做 `lower()` 或 trim，避免隱性不一致。
* **Early return is intentional：** 一旦邏輯矛盾，後面不可能救回，立刻退出能保持 `O(n)` 並更高效。
* **Interview clarity：** 面試時先口述「我會用兩個映射維持雙射，逐步檢查一致性」，然後邊寫邊驗證例子，清楚表達時間/空間複雜度。

<br>

# 🎤 Full Spoken-Style Interview Answer

### 1. **Clarify the Problem & Restate Examples**

*"Okay, let me first restate the problem to make sure I understand it correctly.*

We are given a string `pattern`, which consists of characters, and a string `s`, which consists of words separated by spaces. We want to check if `s` follows the same pattern as `pattern`. By 'follows the same pattern,' it means that there should be a one-to-one mapping, or a bijection, between each character in the pattern and each word in `s`.

So, for example, if the pattern is `'abba'` and the string is `'dog cat cat dog'`, this should return `true`, because `'a'` maps to `'dog'` and `'b'` maps to `'cat'`, and it is consistent throughout.

But if the string was `'dog cat cat fish'`, then it should return `false` because the last `'a'` would need to map back to `'dog'`, but it actually maps to `'fish'`.

Also, another example is `'aaaa'` and `'dog cat cat dog'`. This should return `false` because `'a'` cannot map to both `'dog'` and `'cat'` at the same time.

And finally, `'abba'` and `'dog dog dog dog'` should also return `false`, because different characters `'a'` and `'b'` are both mapping to the same word `'dog'`, which is not allowed."\*

---

### 2. **Discuss Edge Cases**

\*"Now let me think about some edge cases.

* If the lengths are different — for example, the pattern has 4 characters but the string has 3 words — it should immediately return `false`.
* If the pattern is empty and the string is also empty, I would consider that a match, so it should return `true`.
* Another edge case could be where words repeat but pattern doesn’t, or pattern repeats but words don’t, which should both return `false`.
* And one more thing: the mapping has to be **consistent and unique** in both directions. So it’s not just one character → one word, but also one word → one character."\*

---

### 3. **Consider Brute Force and Optimal Approach**

\*"If I think about a brute-force way, I could literally try to map every possible combination of pattern characters to words, but that would be exponential and completely inefficient.

Instead, the optimal solution is to enforce the mapping directly as we walk through the pattern and the words. So, as we iterate, we make sure that each character always maps to the same word, and each word always maps to the same character. If there’s ever a conflict, we can immediately return `false`. This gives us a very clean O(n) solution."\*

---

### 4. **Explain and Implement Optimal Code**

*"So let me explain how I would implement it step by step."*

#### Python (Spoken Style Walkthrough)

\*"First, I’ll split the string `s` into a list of words. Then I’ll check if the length of the pattern is equal to the length of the words list. If not, I can just return `false` immediately.

Next, I’ll create two hash maps, or dictionaries in Python: one from pattern characters to words, and one from words to pattern characters. Then I’ll iterate over the pattern and words together.

At each step, I’ll check two conditions:

1. If this pattern character has already been mapped, it must be mapped to the same word. If it maps to a different word, that’s a contradiction and I return `false`.
2. Similarly, if the current word has already been mapped, it must be mapped to the same pattern character. If not, that’s another contradiction and I return `false`.

If both conditions are satisfied, I set the mapping in both dictionaries.

At the end of the loop, if we never encountered a conflict, then we can safely return `true`."\*

```python
def wordPattern(pattern: str, s: str) -> bool:
    words = s.split()

    if len(pattern) != len(words):
        return False

    p2w = {}
    w2p = {}

    for ch, w in zip(pattern, words):
        # If char already mapped, check consistency
        if ch in p2w and p2w[ch] != w:
            return False
        # If word already mapped, check consistency
        if w in w2p and w2p[w] != ch:
            return False

        # Set the mapping if not already set
        p2w[ch] = w
        w2p[w] = ch

    return True
```

#### Spoken Commentary for JavaScript

*"In JavaScript, it’s basically the same idea. I’ll use a `Map` for both directions. I’ll loop through the pattern and words, and check consistency in both maps. If there’s a mismatch, I return `false`. If we reach the end without issues, I return `true`."*

```javascript
var wordPattern = function(pattern, s) {
  const words = s.split(' ');
  if (pattern.length !== words.length) return false;

  const p2w = new Map();
  const w2p = new Map();

  for (let i = 0; i < pattern.length; i++) {
    const ch = pattern[i];
    const w = words[i];

    if (p2w.has(ch) && p2w.get(ch) !== w) return false;
    if (w2p.has(w) && w2p.get(w) !== ch) return false;

    p2w.set(ch, w);
    w2p.set(w, ch);
  }
  return true;
};
```

---

### 5. **Discuss Time and Space Complexity**

\*"So in terms of complexity:

* Time complexity is O(n), where n is the length of the pattern, since we just do one pass over the characters and words.
* Space complexity is O(k), where k is the number of distinct characters and words, because we store them in the hash maps."\*

---

### 6. **Mention Follow-Up Questions**

\*"Some possible follow-ups might be:

* How would this solution change if the input string `s` could have multiple spaces, or tabs, or even punctuation? I would then use a more robust splitting method, maybe regex.
* Another follow-up could be: what if we wanted to allow multiple characters mapping to the same word, but not the other way around? That would change the constraints and the logic.
* And also, we could discuss the canonicalization approach, where instead of two hash maps, we just encode both sequences into index patterns and compare them. That’s also a valid and elegant solution."\*

<br>

# 🎯 Real-World Applications｜實際應用場景

### 🛒 Example 1: Command Templates in E-commerce Systems

* **English:** In an e-commerce system, customers might use natural language templates like `"buy item item"` or `"add item to cart"`. We need to check if the user’s actual input matches the expected pattern. For instance, `"buy shoes shoes"` could be valid if the pattern is `"abba"` where `a=buy`, `b=shoes`.
* **中文：** 在電商系統裡，客戶可能會用自然語言模板下指令，例如 `"buy item item"` 或 `"add item to cart"`。我們需要檢查用戶輸入是否符合預期模式。例如 `"buy shoes shoes"` 符合 `"abba"` 的模式，其中 `a=buy`、`b=shoes`。

---

### 🗂️ Example 2: URL Routing / API Endpoints

* **English:** Many backend systems need to ensure API endpoints follow a pattern. For example, `/user/{id}/profile` should always match a fixed pattern like `"abc"`. This is similar to verifying word patterns: the path segments must follow a specific template.
* **中文：** 很多後端系統需要確保 API endpoint 遵循固定的路徑模式。例如 `/user/{id}/profile` 必須符合 `"abc"` 的模式。這和檢查 word pattern 類似：URL 的分段要符合固定範本。

---

### 📧 Example 3: Email or Message Validation

* **English:** When parsing emails or chatbot messages, we often want to validate that user input follows a defined structure. For example, `"hello [name], your order [number] is confirmed"` has placeholders, and we need to verify actual messages map consistently to that structure.
* **中文：** 在解析電子郵件或聊天機器人訊息時，常常要檢查用戶輸入是否符合既定結構。例如 `"hello [name], your order [number] is confirmed"` 有佔位符，需要驗證實際訊息能一致地對應到這個結構。

---

### 🔑 Example 4: Authentication & Role Matching

* **English:** In role-based access systems, we may want to check if a user’s role assignments match a predefined pattern, like `"admin user user admin"`. We must ensure the mapping between roles and permissions is consistent across the system.
* **中文：** 在角色存取控制系統中，我們可能要檢查用戶角色的分配是否符合預先定義的模式，例如 `"admin user user admin"`。需要確保角色與權限之間的對應在系統中保持一致。

---

### 🤖 Example 5: Natural Language Processing (NLP) Intent Parsing

* **English:** In NLP tasks, we may need to check if a sentence follows a certain semantic pattern. For example, `"book flight flight book"` should map consistently, similar to the word pattern problem. It helps in detecting templates of commands or intents.
* **中文：** 在自然語言處理任務中，我們可能要檢查一個句子是否符合某種語義模式。例如 `"book flight flight book"` 應該能一致映射，這和 word pattern 問題類似，有助於辨識指令或意圖的範本。

---

### 📌 Summary

* **English:** This problem models **bijection consistency checks**. Real-world use cases include **template matching, routing validation, input parsing, and access control**.
* **中文：** 這題的核心是**雙射一致性檢查**。在實務上常應用於 **模板比對、路由驗證、輸入解析、以及存取控制** 等場景。
