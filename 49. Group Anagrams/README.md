# 🍇 LeetCode 49 — Group Anagrams | UMPIRE (English)

### 🧠 Understand

* **Input:** `strs: string[]` (e.g., `["eat","tea","tan","ate","nat","bat"]`)
* **Output:** `string[][]`, where each sub-array contains words that are mutual anagrams.
* **Order:** The order of groups and the order of words inside a group are *not* required to be stable unless specified.
* **Anagram definition:** Two words are anagrams iff they contain exactly the same letters with the same multiplicities (order-free).
* **Common constraints:** up to `10^4` words, each up to \~100 chars, lowercase a–z.

#### ✅ Edge cases

* Empty string(s): `["",""] → [["",""]]`
* Single item: `["a"] → [["a"]]`
* Many duplicates: `["a","a","a"] → [["a","a","a"]]`
* Very long strings: consider time cost of per-string sorting.

<br>


### 🧲 Match

* **Pattern:** Hashing by a **canonical signature** so that anagrams share the same key.
* **Two mainstream signatures:**

  1. **Sorted string** — key is `''.join(sorted(s))` (simple; `O(L log L)` per word).
  2. **26-letter frequency vector** — key is `tuple(counts)` for `'a'..'z'` (faster; `O(L)` per word).

<br>


### 🗺️ Plan

* **Plan A (Sorted key):**

  1. For each word `s`, compute `key = ''.join(sorted(s))`.
  2. Push `s` into `groups[key]`.
  3. Return `groups.values()`.

* **Plan B (26-count key):**

  1. For each `s`, build `counts[26]` by `counts[ord(ch)-ord('a')]++`.
  2. Convert to immutable key with `tuple(counts)`.
  3. Group by this key and return values.

* **Complexities:**

  * Plan A: `O(N * L log L)` time; `O(N * L)` space.
  * Plan B: `O(N * L)` time; `O(N * L)` space (keys are fixed-length).

<br>


### 🛠️ Implement

#### 🐍 Python — Plan A (sorted key, simplest)

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Map: signature -> list of original words
        groups = defaultdict(list)

        for s in strs:
            # 1) Canonical signature by sorting characters
            #    Sorting ensures anagrams map to the same key, e.g. "eat","tea","ate" -> "aet"
            key = ''.join(sorted(s))  # O(L log L)

            # 2) Append this word into its group bucket
            groups[key].append(s)

        # 3) Return the grouped anagrams (order does not matter)
        return list(groups.values())
```

#### 🐲 Python — Plan B (26-letter frequency key, faster)

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)  # key: 26-length tuple -> list of words

        for s in strs:
            # 1) Build 26-length frequency vector for 'a'..'z'
            counts = [0] * 26
            for ch in s:
                # Assumes lowercase a..z per problem statement
                idx = ord(ch) - ord('a')
                counts[idx] += 1   # O(L) total over the word

            # 2) Convert to immutable tuple so it can be used as a dict key
            key = tuple(counts)

            # 3) Bucket this word under its frequency-signature
            groups[key].append(s)

        return list(groups.values())
```

#### ⚡ JavaScript — Plan A (sorted key)

```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
function groupAnagrams(strs) {
  // Map: signature -> string[]
  const groups = new Map();

  for (const s of strs) {
    // 1) Sort characters to get a canonical signature
    //    Using spread handles Unicode code points better than split("")
    const key = [...s].sort().join(""); // O(L log L)

    // 2) Push into bucket
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(s);
  }

  // 3) Materialize values into an array of arrays
  return Array.from(groups.values());
}
```

#### 🚀 JavaScript — Plan B (26-letter frequency key)

```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
function groupAnagrams(strs) {
  const groups = new Map(); // key: serialized frequency -> string[]

  for (const s of strs) {
    // 1) Build 26-length frequency array
    const counts = new Array(26).fill(0);
    for (let i = 0; i < s.length; i++) {
      const idx = s.charCodeAt(i) - 97; // 'a' -> 97
      counts[idx] += 1;                  // O(L)
    }

    // 2) Serialize with a delimiter to avoid ambiguity, e.g., "1#11" != "11#1"
    const key = counts.join("#");

    // 3) Bucket this word
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(s);
  }

  return Array.from(groups.values());
}
```

<br>


### 🔍 Review

**Walkthrough with** `["eat","tea","tan","ate","nat","bat"]` (sorted key):

* `"eat" -> "aet"` → `{"aet": ["eat"]}`
* `"tea" -> "aet"` → `{"aet": ["eat","tea"]}`
* `"tan" -> "ant"` → `{"aet": [...], "ant": ["tan"]}`
* `"ate" -> "aet"` → `{"aet": ["eat","tea","ate"], "ant": ["tan"]}`
* `"nat" -> "ant"` → `{"aet": [...], "ant": ["tan","nat"]}`
* `"bat" -> "abt"` → `{"aet": [...], "ant": [...], "abt": ["bat"]}`

One valid output: `[["eat","tea","ate"],["tan","nat"],["bat"]]` (order agnostic).

<br>


### 📈 Evaluate

* **Correctness:** Anagrams share identical sorted forms or identical per-letter counts; both signatures are complete invariants.
* **Time/Space:**

  * Plan A: `O(N * L log L)` / `O(N * L)`
  * Plan B: `O(N * L)` / `O(N * L)`
* **Trade-offs:** Sorted key is easiest to write; count key scales better for long words or large inputs.

<br>


# 🍑 LeetCode 49 — 字母異位詞分組 | UMPIRE（中文）

### 🧩 理解（Understand）

* **輸入：** `strs: string[]`（如 `["eat","tea","tan","ate","nat","bat"]`）
* **輸出：** `string[][]`，每個子陣列是一組異位詞。
* **順序：** 群組與群組內的順序通常不要求固定。
* **異位詞定義：** 兩字串含**相同字母與相同次數**（順序無關）。
* **常見限制：** 最多 `10^4` 個字，單字長度至多約 100，皆為小寫英文字母。

#### ✅ 邊界案例

* 空字串：`["",""] → [["",""]]`
* 單一元素：`["a"] → [["a"]]`
* 大量重複：`["a","a","a"] → [["a","a","a"]]`
* 極長字串：注意排序成本。

<br>


### 🔗 匹配（Match）

* **核心模式：** 使用**標準化簽名**做雜湊分組，讓異位詞共享同一把 key。
* **兩大簽名法：**

  1. **排序字串鍵**：`''.join(sorted(s))`（直觀；每字 `O(L log L)`）。
  2. **26 維計數鍵**：`tuple(counts)`（較快；每字 `O(L)`）。

<br>


### 📝 規劃（Plan）

* **方案 A（排序鍵）：**

  1. 對每個 `s` 計算 `key = ''.join(sorted(s))`。
  2. 將 `s` 放入 `groups[key]`。
  3. 回傳 `groups.values()`。

* **方案 B（26 計數鍵）：**

  1. 對每個 `s` 建立 `counts[26]`，`counts[ord(ch)-ord('a')]++`。
  2. 用 `tuple(counts)` 轉成不可變鍵。
  3. 依此鍵分組並回傳值。

* **複雜度：**

  * 方案 A：時間 `O(N * L log L)`；空間 `O(N * L)`。
  * 方案 B：時間 `O(N * L)`；空間 `O(N * L)`（鍵長固定）。

<br>


### 🧪 實作（Implement）

#### 🐼 Python — 方案 A（排序鍵，最易上手）

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # 簽名（排序字串） -> 該組的原字串清單
        groups = defaultdict(list)

        for s in strs:
            # 1) 以排序字元作為標準化簽名
            #    例如 "eat","tea","ate" 皆會變成 "aet"
            key = ''.join(sorted(s))  # 每字 O(L log L)

            # 2) 丟入對應的桶子
            groups[key].append(s)

        # 3) 回傳所有桶子內容（順序不重要）
        return list(groups.values())
```

#### 🐧 Python — 方案 B（26 計數鍵，效能更佳）

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)  # key: 26 長度 tuple -> 該組字串

        for s in strs:
            # 1) 建立 a..z 的次數陣列
            counts = [0] * 26
            for ch in s:
                idx = ord(ch) - ord('a')  # 題目假設輸入為小寫
                counts[idx] += 1          # 每字 O(L)

            # 2) 轉為不可變 tuple，才能當作 dict 的鍵
            key = tuple(counts)

            # 3) 依簽名分組
            groups[key].append(s)

        return list(groups.values())
```

#### 🦊 JavaScript — 方案 A（排序鍵）

```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
function groupAnagrams(strs) {
  const groups = new Map(); // 簽名 -> 陣列

  for (const s of strs) {
    // 1) 排序字元成為標準化簽名
    const key = [...s].sort().join(""); // 每字 O(L log L)

    // 2) 加入桶子
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(s);
  }

  return Array.from(groups.values());
}
```

#### 🐯 JavaScript — 方案 B（26 計數鍵）

```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
function groupAnagrams(strs) {
  const groups = new Map(); // key: 計數序列化字串 -> 陣列

  for (const s of strs) {
    // 1) 統計 a..z 次數
    const counts = new Array(26).fill(0);
    for (let i = 0; i < s.length; i++) {
      const idx = s.charCodeAt(i) - 97; // 'a' 的 ASCII 為 97
      counts[idx] += 1;                  // 每字 O(L)
    }

    // 2) 用分隔符序列化，避免歧義
    const key = counts.join("#");

    // 3) 依鍵分組
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(s);
  }

  return Array.from(groups.values());
}
```

<br>


### 🪞 檢查（Review）

以 `["eat","tea","tan","ate","nat","bat"]` 為例（排序鍵）：

* `"eat" -> "aet"`：`{"aet": ["eat"]}`
* `"tea" -> "aet"`：`{"aet": ["eat","tea"]}`
* `"tan" -> "ant"`：`{"aet": [...], "ant": ["tan"]}`
* `"ate" -> "aet"`：`{"aet": ["eat","tea","ate"], "ant": ["tan"]}`
* `"nat" -> "ant"`：`{"aet": [...], "ant": ["tan","nat"]}`
* `"bat" -> "abt"`：`{"aet": [...], "ant": [...], "abt": ["bat"]}`

可接受輸出：`[["eat","tea","ate"],["tan","nat"],["bat"]]`（順序無要求）。

<br>


### 🧮 評估（Evaluate）

* **正確性：** 排序或計數皆為完整簽名；同簽名即同組異位詞。
* **時間/空間：**

  * 方案 A：`O(N * L log L)` / `O(N * L)`
  * 方案 B：`O(N * L)` / `O(N * L)`
* **取捨：** 方案 A 最直觀、碼量少；方案 B 較快、適合長字串或大資料量。

<br>

# 📌 Additional Notes — Writing This Problem Well

### 🐞 Pitfalls & Gotchas

* **Don’t use a mutable list as a dict key（Python）**：`list` 不可雜湊；請用 `tuple(counts)`。
* **String serialization ambiguity（JS）**：序列化計數時請加分隔符（如 `'#'`），避免 `"111"` 的歧義。
* **Input normalization**：若題目可能含大寫或非字母，先正規化（`lower()`、過濾 `isalpha()`）再建簽名；或用 `Counter(s)` 並將 `sorted(counter.items())` 變成 `tuple` 當鍵。
* **Performance choice**：長字串/大 N → 優先用計數鍵；一般情境用排序鍵即可。
* **Deterministic output**：若面試官要求組內排序，最後對每組 `sort()`（額外 `O(total * log groupSize)`）。

### 🌱 Variations & Follow-ups

* **Case-insensitive / ignore spaces & punctuation**：先正規化字串再簽名。
* **Unicode / multilingual**：使用字元→次數的 `Counter/Map`，簽名為 `tuple(sorted(counter.items()))`。
* **Streaming / huge input**：依簽名前綴分桶（外部存儲），最後合併。
* **Parallelization**：簽名計算可並行，最後 reduce by key。

### 🧫 Mini Test Cases

* `[""] → [[""]]`
* `["",""] → [["",""]]`
* `["a"] → [["a"]]`
* `["ab","ba","abc","cab","bca","cba"] → [["ab","ba"],["abc","cab","bca","cba"]]`
* `["eat","Tea","ate!"]` after normalization (lower + letters only) → `[["eat","tea","ate"]]`

### 🗣️ Quick Interview Script

> “I’ll hash each word by a canonical signature so anagrams share the same key.
> The simplest signature is the sorted characters (`O(L log L)` per word).
> If performance matters for long words, I’ll use a 26-length frequency vector and convert it to an immutable key (`tuple` in Python) for `O(L)` per word.
> Then I group words in a hashmap keyed by the signature and return the values.
> Complexity is `O(N·L log L)` or `O(N·L)` depending on the signature; space is `O(N·L)`.
> If needed, I’ll normalize case or strip non-letters, and sort each group for deterministic output.”

<br>

