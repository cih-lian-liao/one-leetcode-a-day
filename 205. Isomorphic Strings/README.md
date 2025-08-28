# 🧩 LeetCode 205 — Isomorphic Strings · UMPIRE Notes

### 🎯 U — Understand

**Problem:** Determine whether two strings `s` and `t` are *isomorphic*: there exists a one-to-one, order-preserving mapping from every character in `s` to exactly one character in `t`.
**Key rules:**

* Same length required.
* Each `s` char maps to exactly one `t` char (function).
* Different `s` chars cannot map to the same `t` char (injective).
* Equal chars in `s` must correspond to equal chars in `t` at the same relative positions (order preserved).

### 🧠 M — Match

* Category: **Hash map / bijection (one-to-one mapping)**.
* Canonical approach for interviews: **two hash maps maintained together** (`s→t` and `t→s`) to prevent both inconsistent mapping and many-to-one collisions.
* Complexity target: **O(n)** time, **O(k)** space (distinct chars ≤ n).

### 🗺️ P — Plan

1. If `len(s) != len(t)`: return `False`.
2. Create two maps: `s_to_t` and `t_to_s`.
3. Scan pairs `(cs, ct)` from `s` and `t` in lockstep:

   * If either side already has a mapping, it **must** match the current counterpart; otherwise `False`.
   * Only when **both** sides are free (no conflict) do we establish the new bidirectional mapping.
4. If the loop finishes with no violation: `True`.

### 🛠️ I — Implement

#### 🐍 Python (with detailed comments)

```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        # 0) Quick length check: different lengths cannot be isomorphic
        if len(s) != len(t):
            return False

        # Two dictionaries to enforce a bijection:
        # s_to_t: char in s -> char in t
        # t_to_s: char in t -> char in s
        s_to_t = {}
        t_to_s = {}

        # 1) Walk both strings together
        for cs, ct in zip(s, t):
            # Fetch any existing mappings
            mapped_ct = s_to_t.get(cs)  # what cs previously mapped to (if any)
            mapped_cs = t_to_s.get(ct)  # what ct was previously mapped from (if any)

            # 2) If a mapping already exists on either side, it must be consistent
            #    (same char pair as before). Any mismatch => violate bijection.
            if mapped_ct is not None and mapped_ct != ct:
                return False
            if mapped_cs is not None and mapped_cs != cs:
                return False

            # 3) Establish a new mapping only if BOTH sides are currently free.
            #    If they already match (same pair), we do nothing (idempotent).
            if mapped_ct is None and mapped_cs is None:
                s_to_t[cs] = ct
                t_to_s[ct] = cs

        # 4) All pairs processed without conflicts => isomorphic
        return True
```

#### 🌐 JavaScript (with detailed comments)

```javascript
/**
 * Determine if two strings are isomorphic using a bijection check.
 * Time: O(n), Space: O(k) where k ≤ n (distinct characters)
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
function isIsomorphic(s, t) {
  // 0) Different lengths cannot be isomorphic
  if (s.length !== t.length) return false;

  // Two maps to guarantee one-to-one mapping (bijection)
  const s2t = new Map(); // s char -> t char
  const t2s = new Map(); // t char -> s char

  // 1) Iterate over both strings in lockstep
  for (let i = 0; i < s.length; i++) {
    const cs = s[i];
    const ct = t[i];

    const mappedCt = s2t.has(cs) ? s2t.get(cs) : null; // previous target for cs (if any)
    const mappedCs = t2s.has(ct) ? t2s.get(ct) : null; // previous source for ct (if any)

    // 2) If mapping exists on either side, it must match current pair
    if (mappedCt !== null && mappedCt !== ct) return false;
    if (mappedCs !== null && mappedCs !== cs) return false;

    // 3) Only create a new mapping when both sides are free
    if (mappedCt === null && mappedCs === null) {
      s2t.set(cs, ct);
      t2s.set(ct, cs);
    }
  }

  // 4) No conflicts observed => isomorphic
  return true;
}
```

### 🔍 R — Review

* Edge cases: empty strings → `True`; single char pairs → `True`.
* Conflict examples: `"ab"` vs `"aa"` → `False` (many-to-one); `"foo"` vs `"bar"` → `False` (inconsistent).
* Unicode-safe: uses dictionaries/Maps; not limited to ASCII.

### ⚖️ E — Evaluate

* **Time:** O(n) single pass.
* **Space:** O(k) for distinct characters.
* **Why this approach:** Clear semantics (bijection), strong interview narration, robust for all character sets.

#
#
#

# 📘 中文版 UMPIRE 筆記

### 💡 U — 理解

判斷兩字串 `s` 與 `t` 是否「同構」：存在一個**一對一**且**保序**的映射，將 `s` 的每個字元替換後剛好得到 `t`。
規則：

* 長度需相同。
* `s` 的每個字元對應到 `t` **唯一**字元（函數）。
* `s` 中不同字元不可對應到 `t` 的同一字元（單射）。
* 相同字元在對應位置的關係要一致（保序）。

### 🧭 M — 辨識

* 題型：**哈希映射 / 雙射**。
* 面試主推：同時維護 `s→t` 與 `t→s`，一次掃描即可擋住「不一致」與「多對一」衝突。
* 複雜度：**O(n)** 時間，**O(k)** 空間（k ≤ n）。

### 📝 P — 計畫

1. 長度不同 → `False`。
2. 建立兩張表：`s_to_t` 與 `t_to_s`。
3. 同步掃描 `(cs, ct)`：

   * 若任一邊已有映射，需與當前對應一致；否則 `False`。
   * 僅在兩邊皆為空閒時，才建立新的雙向映射。
4. 全部檢查通過 → `True`。

### 🔧 I — 實作

#### 🐼 Python（含詳細註解）

```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        # 0) 長度不同一定不可能同構
        if len(s) != len(t):
            return False

        # 兩張映射表：確保「一對一」
        s_to_t = {}  # s 的字元 -> t 的字元
        t_to_s = {}  # t 的字元 -> s 的字元

        # 1) 一次同步遍歷
        for cs, ct in zip(s, t):
            # 取出既有的對應（若有）
            mapped_ct = s_to_t.get(cs)  # cs 之前對到的目標字元
            mapped_cs = t_to_s.get(ct)  # ct 之前對到的來源字元

            # 2) 若已有對應，必須與目前 pair 一致
            if mapped_ct is not None and mapped_ct != ct:
                return False
            if mapped_cs is not None and mapped_cs != cs:
                return False

            # 3) 僅在兩側都沒被占用時，才建立新對應
            if mapped_ct is None and mapped_cs is None:
                s_to_t[cs] = ct
                t_to_s[ct] = cs

        # 4) 沒有衝突即為同構
        return True
```

#### 🪄 JavaScript（含詳細註解）

```javascript
/**
 * 檢查兩字串是否同構（雙向映射）
 * 時間 O(n)，空間 O(k)（k 為不同字元數）
 */
function isIsomorphic(s, t) {
  // 0) 長度不同 => 一定不可能
  if (s.length !== t.length) return false;

  const s2t = new Map(); // s -> t
  const t2s = new Map(); // t -> s

  // 1) 同步掃描
  for (let i = 0; i < s.length; i++) {
    const cs = s[i];
    const ct = t[i];

    const mappedCt = s2t.has(cs) ? s2t.get(cs) : null; // cs 既有目標
    const mappedCs = t2s.has(ct) ? t2s.get(ct) : null; // ct 既有來源

    // 2) 若既有對應，必須與目前 pair 一致
    if (mappedCt !== null && mappedCt !== ct) return false;
    if (mappedCs !== null && mappedCs !== cs) return false;

    // 3) 兩側都空閒才建立新對應
    if (mappedCt === null && mappedCs === null) {
      s2t.set(cs, ct);
      t2s.set(ct, cs);
    }
  }

  // 4) 全程無衝突 => 同構
  return true;
}
```

### ✅ R — 檢查

* 邊界：空字串→`True`；單字元→`True`。
* 反例：`"ab"` 對 `"aa"` → `False`（多對一）；`"foo"` 對 `"bar"` → `False`（不一致）。
* 字元集：此法對 Unicode 安全（不限 ASCII）。

### 📏 E — 評估

* **時間**：O(n)。
* **空間**：O(k)。
* **為何選它**：語意清楚、面試好講、對各種字元都穩定。

---

### 📎 附加筆記（寫題注意事項）

* **一定要做長度檢查**：長度不同，直接 `False`。
* **雙向同檢**：只檢 `s→t` 會放過「多對一」；務必同步檢 `t→s`。
* **先檢查再賦值**：保持「先驗證、後寫入」的節奏，避免半更新造成閱讀負擔。
* **只在雙側空閒時建立新映射**：若已存在且一致，無需覆寫（語義更乾淨）。
* **Unicode 友善**：用字典/Map，不要硬用固定長度陣列（ASCII）以免遇到 emoji/擴展字元失效。
* **談複雜度時的措辭**：時間 O(n)、空間 O(k)，說明 `k` 是不同字元數，最壞不超過 `n`。
* **講解示例**：`"paper"` ↔ `"title"` 是好例子（`p→t, a→i, e→l, r→e`），反例用 `"ab"` ↔ `"aa"` 說明多對一衝突。

#
#
#

## 🎤 Full Spoken-Style Interview Answer

### 1) 🔍 Clarify the problem & restate examples/constraints (spoken)

> “I want to restate the problem to make sure we’re aligned. We’re given two strings `s` and `t`. They’re **isomorphic** if I can replace each character in `s` with a **unique** character so that I get `t`, while preserving order.
> Concretely, it’s a **one-to-one** mapping: each `s` char maps to exactly one `t` char, and no two different `s` chars map to the same `t` char. Also, when the i-th and j-th characters of `s` are equal, then the i-th and j-th characters of `t` must also be equal.
>
> Examples the problem usually gives:
>
> * `s = "egg", t = "add"` → true (`e→a`, `g→d`)
> * `s = "foo", t = "bar"` → false (the two `o`’s can’t map to both `a` and `r`)
> * `s = "paper", t = "title"` → true (`p→t, a→i, e→l, r→e`)
>
> Constraints-wise, lengths can be up to `n` (I’ll treat it as large enough that an **O(n)** solution is expected). If lengths differ, it’s immediately false.”

---

### 2) 🧊 Edge cases (spoken)

> “Edge cases I’ll consider:
>
> * Empty strings: `""` vs `""` should be true.
> * Single character pairs: always true if lengths are equal.
> * Immediate mismatch in length: false.
> * ‘Many-to-one’ traps: `s="ab", t="aa"` must be false.
> * Repeated patterns must align: positions of equal letters should stay equal after mapping.
> * Character set: may include any Unicode, so I’ll avoid ASCII-only tricks unless specified.”

---

### 3) 🧪 Brute force vs. optimal approach (spoken)

> “A brute-force way would be to **guess** mappings and backtrack whenever we hit contradictions. That explodes combinatorially and isn’t practical.
>
> A cleaner near-brute approach is to keep a **single** map `s→t`. That catches ‘one-to-many’ mistakes (e.g., `a→x` and later trying `a→y`), but it **doesn’t** catch ‘many-to-one’ (`a→x`, `b→x`).
>
> The **optimal interview approach** is to maintain **two maps at once**: `s→t` and `t→s`. For every pair `(cs, ct)`, if either side already has a mapping, it must match the current pair; otherwise we return false. If both sides are free, we create the new mapping in both directions. That gives me a single pass, **O(n)** time, and **O(k)** space where `k` is the number of distinct characters. It’s also Unicode-safe because I use normal hash maps.
>
> There’s also a nice alternative: normalize both strings into their **first-occurrence patterns** (like `paper → 0,1,0,2,3`) and compare the sequences. That’s also O(n), just a different style. But for interviews I prefer the two-map approach because it explains the ‘bijection’ idea very clearly.”

---

### 4) ✍️ Explain & implement the optimal code (spoken + code)

> “I’ll code the two-map solution. Steps I’ll follow while coding:
>
> 1. If lengths differ, return false.
> 2. Create two maps: `s→t` and `t→s`.
> 3. Walk the strings in lockstep. For each `(cs, ct)`, check if either map already has a binding:
>
>    * If `cs` was seen before, it must map to `ct`.
>    * If `ct` was seen before, it must map back to `cs`.
>    * If both sides are clear, I’ll insert both directions.
> 4. If I finish the loop with no conflicts, return true.”

#### 🐍 Python

```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        # 0) Quick reject: different lengths => cannot be isomorphic
        if len(s) != len(t):
            return False

        # 1) Prepare two hash maps to enforce a bijection
        #    - s_to_t: map each char in s to a unique char in t
        #    - t_to_s: map each char in t back to a unique char in s
        s_to_t = {}
        t_to_s = {}

        # 2) Scan both strings in lockstep
        for cs, ct in zip(s, t):
            mapped_ct = s_to_t.get(cs)  # prior target of cs, if any
            mapped_cs = t_to_s.get(ct)  # prior source of ct, if any

            # 3) If either mapping exists, it must be consistent
            if mapped_ct is not None and mapped_ct != ct:
                return False  # cs previously mapped to a different char
            if mapped_cs is not None and mapped_cs != cs:
                return False  # ct previously mapped from a different char

            # 4) Only establish a new mapping when both sides are free
            if mapped_ct is None and mapped_cs is None:
                s_to_t[cs] = ct
                t_to_s[ct] = cs

        # 5) All pairs processed with no contradictions => isomorphic
        return True
```

#### 🌐 JavaScript

```javascript
/**
 * Check if two strings are isomorphic using a two-way bijection.
 * Time: O(n) — single pass
 * Space: O(k) — k distinct characters (k ≤ n)
 */
function isIsomorphic(s, t) {
  // 0) If lengths differ, it's impossible
  if (s.length !== t.length) return false;

  // 1) Two maps to enforce a one-to-one relation
  const s2t = new Map(); // s char -> t char
  const t2s = new Map(); // t char -> s char

  // 2) Walk through both strings together
  for (let i = 0; i < s.length; i++) {
    const cs = s[i];
    const ct = t[i];

    const mappedCt = s2t.has(cs) ? s2t.get(cs) : null;
    const mappedCs = t2s.has(ct) ? t2s.get(ct) : null;

    // 3) Existing mappings must be consistent with the current pair
    if (mappedCt !== null && mappedCt !== ct) return false;
    if (mappedCs !== null && mappedCs !== cs) return false;

    // 4) Only create new bindings when both directions are still free
    if (mappedCt === null && mappedCs === null) {
      s2t.set(cs, ct);
      t2s.set(ct, cs);
    }
  }

  // 5) No violations detected => isomorphic
  return true;
}
```

> “Quick dry-run with `s="paper"` and `t="title"`:
>
> * see `p→t`, `a→i`; when `p` appears again it maps to `t` consistently; then `e→l`, `r→e`. No conflicts, so return true.
>   A failure case like `s="ab", t="aa"`: first we set `a→a`. Next we try `b→a`, but `a` is already taken on the `t` side, so we fail immediately.”

---

### 5) ⏱️ Time & space complexity (spoken)

> “I traverse once, so **time O(n)**. Each character participates in at most one insertion and a couple of lookups, all O(1) average.
> I store at most one mapping per distinct character, so **space O(k)** where `k ≤ n`. In the worst case `k` equals `n`.”

---

### 6) 🚀 Follow-ups & variations (spoken)

> “A few follow-ups I’d be ready for:
>
> * **Unicode vs. ASCII**: my solution uses hash maps, so it’s Unicode-safe. If inputs were guaranteed ASCII, a micro-optimization is to use two fixed arrays of size 256 to track last-seen indices, giving O(1) extra space—but I would explicitly call out the limitation.
> * **Alternative style**: normalize both strings to first-occurrence patterns and compare; also O(n), just a different expression of the same invariant.
> * **Return the mapping**: if the interviewer asked for the actual mapping, I’d collect `s→t` during the pass and return it alongside the boolean.
> * **Streaming validation**: this algorithm already validates online as we scan; we return early on the first conflict.
> * **Multiple queries**: if we had to check many pairs against the same `s`, we could precompute `s`’s pattern and reuse it to compare against each `t`’s pattern.”

---

> “That’s my approach. I like this method in interviews because it directly encodes the **bijection** requirement, it reads cleanly, and it hits the expected **O(n)** time with simple data structures.”

#
#
#

## 🎯 Real-World Applications｜實際應用場景

### 🛡️ Data Transformation Validation｜資料轉換驗證

**EN:**
Isomorphic string validation is useful in verifying that one structure can be transformed into another without ambiguity, especially in **ETL pipelines**, **data anonymization**, or **data migration** tasks. For example, when transforming user identifiers in logs or anonymized datasets, we may want to ensure that the mapping from original IDs to pseudonyms is **one-to-one** and **consistent**.

**ZH：**
同構字串的概念可用來驗證兩個結構是否可以無歧義地對應，這在 **ETL 數據處理流程**、**資料匿名化** 或 **資料遷移** 中非常實用。例如：在日誌檔案中將使用者 ID 匿名化時，需確保從原始 ID 到匿名代碼的對應是 **一對一** 且 **一致** 的，避免多個 ID 被錯誤地指向同一個代碼。

---

### 💬 Text Encoding & Decoding｜文字編碼與解碼一致性

**EN:**
In **natural language processing (NLP)** or **language translation systems**, we might encode text into token sequences and later decode them. Ensuring the encoded form preserves structure — like repeating words, punctuation, etc. — can be modeled similarly to isomorphic validation, especially in **testing round-trip translation** consistency.

**ZH：**
在 **自然語言處理（NLP）** 或 **機器翻譯系統** 中，將文字編碼為符號序列再解碼回原文，是一個常見流程。為確保這種「來回轉換」保持結構（例如：重複詞、標點位置），就可以套用同構驗證的邏輯，檢查兩個版本的字串是否具備一致的相對關係。

---

### 🎭 Pattern Matching in Templates｜模板模式匹配

**EN:**
Isomorphic string comparison is used in **template engines** or **pattern matching systems** to verify whether an input string conforms to a specific format. For example, determining whether the string `"abcabc"` fits the pattern `"xyxxyx"` — both follow the same structure and could be validated using an isomorphic check.

**ZH：**
在 **模板引擎** 或 **樣式匹配系統** 中，同構字串比較常被用來判斷輸入是否符合某種模式。例如：`"abcabc"` 是否符合 `"xyxxyx"` 的格式 —— 若其結構一致（字元的重複關係相同），就可以視為匹配成功，這正好是同構驗證的應用。

---

### 🧬 Genome Mapping & Symbolic Encoding｜基因映射與符號編碼

**EN:**
In **bioinformatics**, when mapping genome sequences or symbolic patterns (like proteins or DNA strands), it’s important to ensure the encoding across organisms or experiments is consistent. Isomorphism checks help detect if two encoded strands **maintain the same structure** without mapping multiple symbols to one target.

**ZH：**
在 **生物資訊學** 中，處理基因序列或符號模式（如蛋白質、DNA 鏈）時，常需比對不同樣本是否具備**相同結構**。同構比對可幫助我們檢查：兩條序列是否有對應關係且不會出現「多對一」錯誤，有助於基因資料的對齊與轉譯分析。

---

### 🧠 AI Model Input Pattern Checking｜AI 模型輸入模式檢查

**EN:**
Some **AI systems** enforce input pattern constraints — for example, in prompt engineering for LLMs or symbolic AI — to ensure templates are followed. Verifying that new inputs follow the same token pattern as a baseline input can be done using isomorphic comparison.

**ZH：**
某些 **AI 系統**，尤其是大型語言模型（LLM）或符號推理系統，會要求輸入符合特定模板。透過同構比對，可以檢查新的輸入是否與原始輸入保持「結構一致」，對於 prompt engineering 或輸入驗證特別重要。

---

### 🧮 Summary｜總結

| Use Case                        | 中文用途說明      |
| ------------------------------- | ----------- |
| Data transformation consistency | 資料轉換一致性驗證   |
| NLP round-trip encoding         | 自然語言編碼/解碼驗證 |
| Pattern matching                | 模板與樣式比對     |
| Bioinformatics                  | 基因或符號資料對齊   |
| AI prompt validation            | AI 模板輸入格式驗證 |


#
#
#



### ✅ 核心邏輯檢查

```python
for cs, ct in zip(s, t):
    if cs in s_to_t and s_to_t[cs] != ct:
        return False
    if ct in t_to_s and t_to_s[ct] != cs:
        return False
    s_to_t[cs] = ct
    t_to_s[ct] = cs
```

這段的意義是：

* 若 `cs` 有出現過，它的對應字元必須是目前的 `ct`，否則返回 `False`（防止一對多錯誤）
* 若 `ct` 有出現過，它的來源字元必須是目前的 `cs`，否則返回 `False`（防止多對一錯誤）
* 如果雙邊都沒衝突，更新對應表

非常正確，符合同構定義。

---

### 🔍 小建議：風格上的優化（非必要）

你可以選擇像前面我們提到的這樣「**先檢查，再賦值**」，讓邏輯更清晰一點點（尤其是面試中容易講解）：

```python
for cs, ct in zip(s, t):
    if cs in s_to_t:
        if s_to_t[cs] != ct:
            return False
    elif ct in t_to_s:
        return False
    else:
        s_to_t[cs] = ct
        t_to_s[ct] = cs
```

或者，使用 `get()`：

```python
for cs, ct in zip(s, t):
    mapped_ct = s_to_t.get(cs)
    mapped_cs = t_to_s.get(ct)
    if mapped_ct is not None and mapped_ct != ct:
        return False
    if mapped_cs is not None and mapped_cs != cs:
        return False
    if mapped_ct is None and mapped_cs is None:
        s_to_t[cs] = ct
        t_to_s[ct] = cs
```

這些寫法的行為和你的一樣，只是在**語義上更清楚「驗證無衝突 → 再建立對應」**，不會提早寫入資料，有助於日後維護和面試表達。

---

### 🧪 測試案例建議

你可以手動用以下幾組來測一下：

| s       | t       | Expected | Why                     |
| ------- | ------- | -------- | ----------------------- |
| "egg"   | "add"   | True     | `e→a, g→d`              |
| "foo"   | "bar"   | False    | `o` 想同時對應 `a` 與 `r`     |
| "paper" | "title" | True     | `p→t, a→i, e→l, r→e`    |
| "ab"    | "aa"    | False    | `b→a` 不行，`a` 已經給了 `a→a` |
| ""      | ""      | True     | 空字串同構                   |
| "a"     | "a"     | True     | 單字元同構                   |

---

### ✅ 結論

* ✅ 你這份代碼是 **正確的解法**
* 💡 若你想更強調「先檢查再寫入」的流程，可以改寫得更語義清楚（非必要）
* 🧠 建議你練習幾組特殊測資，幫助你更理解這段程式的反應與限制

