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

