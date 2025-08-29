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

# 🎤 Full Spoken-Style Interview Answer 

### 1) Clarify the problem & read examples/constraints

**What I’d say:**

> “I’ll restate the problem to make sure I understand it. I’m given an array of strings, and I need to group the anagrams together. Two strings are anagrams if they have exactly the same letters with the same counts, just in a different order.
>
> For example, if the input is `["eat","tea","tan","ate","nat","bat"]`, one valid output is `[["eat","tea","ate"],["tan","nat"],["bat"]]`. The order of the groups and the order **inside** each group don’t matter unless you specify otherwise.
>
> I’ll assume all strings are lowercase English letters `'a'..'z'`, lengths up to maybe 100, and the number of words can be up to, say, `10^4`. Please let me know if there are any constraints I should be aware of beyond this. If not, I’ll proceed with those typical constraints.”

---

### 2) Discuss edge cases

**What I’d say:**

> “Edge cases I’ll keep in mind:
> • Empty strings: `["",""]` should group together.
> • Single element: `["a"]` returns `[["a"]]`.
> • Lots of duplicates: `["a","a","a"]` stays together.
> • Very long strings: I should be mindful of sorting costs per string.
> • Potential non-letters or mixed case—if that existed I’d normalize (e.g., lowercase and strip non-letters), but for this problem I’ll assume clean lowercase input.”

---

### 3) Consider brute-force and optimal approach

**What I’d say:**

> “A brute-force way would be: for each string, compare it against every other string that hasn’t been grouped yet, and check if they’re anagrams. A direct anagram check can be `O(L)` if I count letters, or `O(L log L)` if I sort each pair. But doing that for all pairs is `O(N^2 * L)`, which is too slow when `N` is large.
>
> A **much better** approach is to use a **hash map** where the key is a **canonical signature** of the word—an identifier that’s identical for all anagrams. There are two common signatures:
>
> • **Sorted string**: sort the characters; anagrams map to the same sorted result. Per word cost `O(L log L)`.
> • **26-letter frequency vector**: count a..z and use that 26-length count as the key; per word cost `O(L)`.
>
> Both are valid. If performance matters for long strings, the frequency vector is strictly better (`O(N·L)` overall). For simplicity under time pressure, the sorted key is very fast to code and is usually accepted. I’ll code the **frequency-vector version** as the optimal one, and I can also show the sorted-key version if you’d like.”

---

### 4) Explain & implement the optimal code (talk while typing)

**What I’d say while coding (Python, frequency-vector key):**

> “I’ll write a function `groupAnagrams` that returns a list of groups. I’ll create a dictionary from signature to list of words. For each word, I’ll build a 26-length count array, convert it to a tuple to make it hashable, and then append the word to that bucket. Finally I’ll return the dictionary values.”

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # groups maps: frequency signature (tuple of 26 ints) -> list of original words
        groups = defaultdict(list)

        for s in strs:
            # Build a 26-length frequency vector for 'a'..'z'.
            # This is O(L) per word where L is the length of s.
            freq = [0] * 26
            for ch in s:
                # Assuming lowercase 'a'..'z'.
                # Map 'a'->0, 'b'->1, ..., 'z'->25
                freq[ord(ch) - ord('a')] += 1

            # Convert list to tuple so it's immutable and hashable as a dict key.
            signature = tuple(freq)

            # Append the original string to its anagram bucket.
            groups[signature].append(s)

        # The values of the dict are the grouped anagrams.
        return list(groups.values())
```

**How I’d quickly walk through the example out loud:**

> “For `eat`, the counts are a:1, e:1, t:1 → same signature as `tea` and `ate`, so they land in the same bucket. For `tan` and `nat`, counts match and they share a bucket. `bat` lands in its own bucket. At the end I return all the buckets as a list of lists.”

**(Optional) If interviewer asks for the simpler sorted-key version:**

> “Here’s the concise sorted-key solution. It’s a tiny bit slower on long strings (`O(L log L)` per word) but often totally fine.”

```python
from collections import defaultdict
from typing import List

class SolutionSortedKey:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)
        for s in strs:
            # Canonical signature by sorting characters.
            # "eat","tea","ate" -> "aet"
            key = ''.join(sorted(s))  # O(L log L)
            groups[key].append(s)
        return list(groups.values())
```

---

### 5) Discuss time/space complexity

**What I’d say:**

> “For the frequency-vector solution:
> • Time complexity is **O(N · L)**: each word is processed once and we count characters in linear time.
> • Space complexity is **O(N · L)** overall because we store all the input strings in the output structure; the keys themselves are fixed size (26 integers) per *distinct* group but that’s relatively small compared to the total characters stored.
>
> For the sorted-key approach:
> • Time is **O(N · L log L)** due to sorting each word.
> • Space is **O(N · L)** as well.”

---

### 6) Mention follow-up questions

**What I’d say:**

> “A few natural follow-ups:
>
> 1. **Deterministic order**: If you want each group sorted, I can sort each bucket at the end. If you want the buckets in a specific order, we can sort by, say, the first word or by group size.
> 2. **Mixed case or punctuation**: If inputs may contain capitals or non-letters, I’d normalize—lowercase and optionally filter to letters—before building the signature.
> 3. **Unicode / multilingual**: For arbitrary character sets, I’d use a `collections.Counter` of characters and make the key a `tuple(sorted(counter.items()))`. That’s still `O(L)` per word but with a slightly larger constant.
> 4. **Streaming / huge data**: If memory is tight, we could hash to temporary files by signature prefix and merge later (external grouping).
> 5. **Parallelization**: Signature building is embarrassingly parallel; a map-reduce pattern works well—map words to signatures, then reduce by key.”

---

### Bonus: “What I literally say while typing” (condensed)

> “I’ll use a hash map from a canonical signature to the list of words. For optimal time I’ll build a 26-length frequency vector per word, convert it to a tuple, and use that as the key. Anagrams share the same letter counts, so they end up in the same bucket. Time is `O(N·L)`, space is `O(N·L)`. If you prefer simpler code, I can switch to sorting each word and using the sorted string as the key; that’s `O(N·L log L)` but shorter to write. I’ll go with the frequency vector now and then dry-run the provided example to confirm correctness.”

<br>


# 🎯 Real-World Applications｜實際應用場景


### 🗃️ Grouping Log Entries by Structure

#### 分組具有相同結構的日誌訊息

**EN:**
In large-scale systems, log entries often have different parameter values but follow the same template (like: `"User 123 logged in"` vs `"User 456 logged in"`).
We can normalize the logs (by removing user IDs, timestamps, etc.) and group them by the template (signature). This is similar to grouping by anagram keys.

**中文：**
在大型系統中，日誌訊息雖然參數不同，但結構很類似（例如 `"User 123 logged in"` 跟 `"User 456 logged in"`）。
我們可以去除可變資訊（像使用者 ID、時間戳）後做簽名化，將具有相同「結構」的日誌分組，這就像是異位詞分組的概念。

---

### 🔍 Detecting Plagiarism or Code Similarity

#### 偵測抄襲或程式碼結構相似性

**EN:**
In education or software engineering, you might want to detect if two pieces of code or two documents have the same underlying structure.
You can strip out variable names or formatting and group submissions with identical normalized structure.

**中文：**
在教育或開發中，我們常想檢查兩份程式碼或文件是否「本質上」一樣。
可以將變數名稱或排版清除，保留邏輯結構後，再以簽名方式分組，就像找異位詞那樣找到結構雷同的程式。

---

### 🧬 Grouping DNA Sequences with Same Composition

#### 將具有相同組成的 DNA 序列歸類

**EN:**
In bioinformatics, two DNA strands with the same base composition but different orders can be functionally relevant.
We can use the same technique: count base occurrences (`A`, `T`, `G`, `C`) and group sequences with identical counts.

**中文：**
在生物資訊學中，有些 DNA 序列雖然順序不同，但因為含有一樣的 A/T/G/C 數量，具有類似功能。
可以用類似異位詞的「計數法」來分組這些序列，找出潛在關聯。

---

### 💬 Grouping Synonyms or Translations with Same Letters

#### 將同字母但順序不同的詞彙或翻譯分在一起

**EN:**
In natural language processing, some tools might group translations or misspellings based on character-level similarity—anagram-like logic helps cluster words that are likely variants of each other.

**中文：**
在自然語言處理中，像拼字錯誤或同義詞，有時字母相似但順序不同。我們可以用異位詞分組法，將這些「可能相關」的字彙歸在一起，提升分析準確度。

---

### 📦 Inventory or Recipe Matching

#### 庫存或食譜材料組合比對

**EN:**
In supply chain or cooking apps, you may need to match combinations of ingredients or components, regardless of order.
Using a frequency-count signature of item quantities allows grouping interchangeable sets.

**中文：**
在供應鏈或食譜應用中，材料的順序不重要，只要組成相同就可以互換。
利用類似異位詞的計數簽名，就能把這些材料組合正確歸為同一類。

---

### 🔁 Cache De-duplication or Memoization

#### 快取去重或函式記憶化（Memoization）

**EN:**
When using memoization, sometimes the order of inputs doesn’t matter. For example, a function that takes a list of items as a set.
We can sort or normalize the input to avoid recomputing the same logic, just like using a sorted string key in anagram grouping.

**中文：**
在使用快取或函式記憶化時，有些輸入順序不重要（如一組項目集合）。
可以先排序輸入並做為 key，這樣就能避免重複計算，這和異位詞的排序簽名法是一樣的核心技巧。

