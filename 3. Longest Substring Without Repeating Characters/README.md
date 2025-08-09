# 🐍 LeetCode 3 — Longest Substring Without Repeating Characters


### 🧠 U — Understand

#### Problem

Given a string `s`, return the **length** of the longest substring that contains **no repeated characters**. A substring must be **contiguous**.

#### Inputs & Outputs

* **Input:** `s` (ASCII/Unicode string; may include letters, digits, spaces, symbols)
* **Output:** `int` — maximum length of any substring with all distinct characters

#### Examples

* `s = "abcabcbb"` → `3` (substring `"abc"`)
* `s = "bbbbb"` → `1` (substring `"b"`)
* `s = "pwwkew"` → `3` (substring `"wke"`)
* `s = ""` → `0`

---

### 🧩 M — Match

This is a classic **sliding window** problem. Maintain a window `[left, right]` whose characters are **unique**.
Use a hash map `lastSeen: char -> latest index` to quickly detect duplicates **inside** the window. When we see a duplicate `c`, set `left = lastSeen[c] + 1` to jump past its previous position.

Why it fits:

* `right` only moves forward.
* `left` moves forward only when needed to restore uniqueness.
* Each index moves at most once → linear time.

---

### 🗺️ P — Plan

1. Initialize `left = 0`, `best = 0`, `lastSeen = {}`.
2. For each `right` from `0..len(s)-1`:

   * `c = s[right]`.
   * If `c in lastSeen` **and** `lastSeen[c] >= left`, set `left = lastSeen[c] + 1`.
   * Update `lastSeen[c] = right`.
   * Update `best = max(best, right - left + 1)`.
3. Return `best`.

---

### 💻 I — Implement (with detailed comments)

#### Python (preferred: last-seen index map)

```python
def lengthOfLongestSubstring(s: str) -> int:
    """
    Sliding window with last-seen index map.
    Time: O(n), Space: O(min(n, Σ)) where Σ is the charset size.
    """
    lastSeen = {}   # char -> most recent index
    left = 0        # left boundary (inclusive) of current window
    best = 0        # best (max) window length found so far

    # Expand the window by moving 'right' one step at a time
    for right, c in enumerate(s):
        # If 'c' has been seen and its last index lies inside the window,
        # move 'left' to one position after that to remove the duplicate.
        if c in lastSeen and lastSeen[c] >= left:
            left = lastSeen[c] + 1

        # Record the latest index for 'c'
        lastSeen[c] = right

        # Update best using the current window length
        current_len = right - left + 1
        if current_len > best:
            best = current_len

    return best
```

#### JavaScript (preferred: last-seen index map)

```javascript
/**
 * Sliding window with last-seen index map.
 * Time: O(n), Space: O(min(n, Σ))
 */
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map(); // char -> most recent index
  let left = 0;               // window left boundary (inclusive)
  let best = 0;               // best length so far

  for (let right = 0; right < s.length; right++) {
    const c = s[right];

    // If 'c' is a duplicate inside the window, jump 'left' forward
    if (lastSeen.has(c) && lastSeen.get(c) >= left) {
      left = lastSeen.get(c) + 1;
    }

    // Update last seen index for 'c'
    lastSeen.set(c, right);

    // Update the best window length
    const currentLen = right - left + 1;
    if (currentLen > best) best = currentLen;
  }

  return best;
}
```

#### Optional Variant: Set + incremental shrink (same logic, more while-ops)

```python
def lengthOfLongestSubstring_set(s: str) -> int:
    seen = set()
    left = 0
    best = 0

    for right, c in enumerate(s):
        # Shrink from the left until 'c' can be inserted without duplication
        while c in seen:
            seen.remove(s[left])
            left += 1
        seen.add(c)
        best = max(best, right - left + 1)
    return best
```

```javascript
function lengthOfLongestSubstring_set(s) {
  const seen = new Set();
  let left = 0, best = 0;

  for (let right = 0; right < s.length; right++) {
    const c = s[right];
    // Remove leftmost chars until 'c' becomes unique in the set
    while (seen.has(c)) {
      seen.delete(s[left]);
      left++;
    }
    seen.add(c);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

---

### 🧪 R — Review

Walk-through for `"abcabcbb"`:

* Start `left=0, best=0`
* `right=0,'a'` → window `"a"` → best=1
* `right=1,'b'` → `"ab"` → best=2
* `right=2,'c'` → `"abc"` → best=3
* `right=3,'a'` dup (last at 0 ≥ left=0) → `left=1` → `"bca"` → best=3
* `right=4,'b'` dup (1 ≥ 1) → `left=2` → `"cab"` → best=3
* `right=5,'c'` dup (2 ≥ 2) → `left=3` → `"abc"` → best=3
* `right=6,'b'` dup (4 ≥ 3) → `left=5` → `"cb"` → best=3
* `right=7,'b'` dup (6 ≥ 5) → `left=7` → `"b"` → best=3
  Return `3`.

Quick checks:

* `""` → 0
* `"bbbbb"` → 1
* `"pwwkew"` → 3 (`"wke"`)

---

### 📈 E — Evaluate

* **Time Complexity:** O(n) — each index enters/leaves the window at most once.
* **Space Complexity:** O(min(n, Σ)) — stores at most one entry per distinct character in the window.

Why the “lastSeen map” is often preferred:

* Directly **jumping** `left` reduces extra loop iterations vs. the `while`-shrink variant.
* Easier to extend (e.g., also return the substring, allow up to *k* repeats, character replacements).

---

### 🧊 Notes & Edge Cases

* Empty string → 0
* All characters identical → 1
* All unique → `len(s)`
* Spaces/symbols are ordinary characters in this problem.
* Unicode detail:

  * Python iterates Unicode code points.
  * JavaScript indexes UTF‑16 code units; some emojis/rare chars use two code units. Algorithm still works for this problem’s definition. If you need **user-perceived** characters (grapheme clusters), use `Intl.Segmenter` or a library.

#
#
#

# 🧧 無重複字元的最長子字串


### 🧠 U — 理解

#### 題意

給定字串 `s`，求**不含重複字元**的**最長子字串**之**長度**（子字串需**連續**）。

#### 輸入與輸出

* **輸入：** 任意字串（可能包含字母、數字、空白、符號、Unicode）
* **輸出：** 整數（最長不重複子字串的長度）

#### 範例

* `"abcabcbb"` → `3`（`"abc"`)
* `"bbbbb"` → `1`（`"b"`)
* `"pwwkew"` → `3`（`"wke"`)
* `""` → `0`

---

### 🧩 M — 對應

這是典型的**滑動視窗**：維持 `[left, right]` 視窗，視窗內字元需互不重複。
建立 `lastSeen`（雜湊表）記錄每個字元**最近出現的索引**。當右端遇到重複且仍在視窗內，就把 `left` **一次跳**到「該字元前次位置 + 1」。

---

### 🗺️ P — 步驟

1. 設 `left = 0`, `best = 0`, `lastSeen = {}`。
2. 逐一處理 `right`：

   * `c = s[right]`
   * 若 `c` 在 `lastSeen` 中且 `lastSeen[c] >= left`，則 `left = lastSeen[c] + 1`
   * 更新 `lastSeen[c] = right`
   * 更新 `best = max(best, right - left + 1)`
3. 回傳 `best`。

---

### 💻 I — 實作（含詳細註解）

#### Python（索引表版本，較精簡與可擴充）

```python
def lengthOfLongestSubstring(s: str) -> int:
    """
    滑動視窗 + 字元最近出現索引表
    時間 O(n)，空間 O(min(n, Σ))
    """
    lastSeen = {}   # 字元 -> 最近出現的索引
    left = 0        # 視窗左界（含）
    best = 0        # 目前為止的最大長度

    for right, c in enumerate(s):
        # 若 c 曾出現且其位置仍在視窗內，將 left 跳到舊 c 的右側
        if c in lastSeen and lastSeen[c] >= left:
            left = lastSeen[c] + 1

        # 更新 c 的最近出現位置
        lastSeen[c] = right

        # 計算視窗長度並更新最佳解
        best = max(best, right - left + 1)

    return best
```

#### JavaScript（索引表版本）

```javascript
/**
 * 滑動視窗 + 最近出現索引表
 * 時間 O(n)，空間 O(min(n, Σ))
 */
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map(); // 字元 -> 最近出現的索引
  let left = 0;
  let best = 0;

  for (let right = 0; right < s.length; right++) {
    const c = s[right];

    // 若 c 先前出現且仍在視窗內，left 一次跳到舊 c 的右邊
    if (lastSeen.has(c) && lastSeen.get(c) >= left) {
      left = lastSeen.get(c) + 1;
    }

    // 更新最近出現位置
    lastSeen.set(c, right);

    // 更新最佳長度
    const len = right - left + 1;
    if (len > best) best = len;
  }

  return best;
}
```

#### 可選替代：用 Set 逐步收縮

```python
def lengthOfLongestSubstring_set(s: str) -> int:
    seen = set()
    left = 0
    best = 0

    for right, c in enumerate(s):
        # 視窗內已有 c，就從左側逐一移除直到不重複
        while c in seen:
            seen.remove(s[left])
            left += 1
        seen.add(c)
        best = max(best, right - left + 1)
    return best
```

```javascript
function lengthOfLongestSubstring_set(s) {
  const seen = new Set();
  let left = 0, best = 0;

  for (let right = 0; right < s.length; right++) {
    const c = s[right];
    while (seen.has(c)) {
      seen.delete(s[left]);
      left++;
    }
    seen.add(c);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

---

### 🧪 R — 檢驗

以 `"abcabcbb"` 為例：
遇到重複字元時，把 `left` 跳到「該字元前次位置 + 1」，確保視窗內皆不重複；最長長度為 `3`。

---

### 📈 E — 複雜度

* **時間：** O(n)
* **空間：** O(min(n, Σ))

索引表版本能直接跳動 `left`，通常比 while 收縮更省操作，也更容易延伸到變形題。

---

### 🧊 邊界與注意事項

* 空字串 → `0`；全部相同 → `1`；全部不重複 → `len(s)`
* 空白、符號均視為一般字元
* Unicode：Python 以 Unicode 碼點、JS 以 UTF‑16 碼元為單位；少數字元（如某些 emoji）佔兩個碼元。若需以「視覺字元」計數，需額外斷詞工具；本題用以上方法即可。

#
#

## 🎤 Full Spoken-Style Interview Answer


**1️⃣ Clarify the problem and read the examples / constraints**

*"Okay, so let me restate the problem in my own words to make sure I fully understand it.*
We’re given a string `s`, and we want to find the **length** of the longest substring that contains **no repeating characters**. A substring means it has to be contiguous — no skipping characters.

For example:

* If `s = "abcabcbb"`, the longest substring without repeating characters is `"abc"`, which has length 3.
* If `s = "bbbbb"`, then the answer is 1 because the longest substring without repeating characters is just `"b"`.
* And if `s = "pwwkew"`, the answer should be 3 because `"wke"` is the longest valid substring.

*Are there any constraints on the input size or the character set?*
I’m going to assume the input could be fairly large — maybe up to tens of thousands of characters — so we should aim for an efficient solution, ideally linear time."

---

**2️⃣ Discuss edge cases**

*"Let me think about edge cases:*

* An empty string: the result should be 0.
* A string where all characters are the same: result is 1.
* A string where all characters are unique: result is just the length of the string.
* Strings with spaces or symbols — I’ll treat them as normal characters.
* Strings with mixed case — if case sensitivity matters, `'A'` and `'a'` should be considered different characters unless told otherwise."

---

**3️⃣ Consider brute-force and optimal approach**

\*"The brute-force way would be:

* Enumerate all possible substrings, check if each one has all unique characters, and keep track of the maximum length.
  That would be O(n³) if I generate substrings and check uniqueness with a set every time. That’s too slow for large inputs.

The better approach is to use a **sliding window**:

* We’ll have two pointers: `left` and `right`, representing the current window.
* As we move `right` forward through the string, we keep track of the last index where we saw each character.
* If we see a character that’s already inside our current window, we move `left` to just past its previous position.
* We keep track of the max window size we’ve seen so far.

This way, each character is processed at most twice — once when it enters the window and once when it leaves — so the time complexity is O(n)."

---

**4️⃣ Explain and implement optimal code**

*"Alright, I’ll start coding in Python first, and I’ll talk through it as I go."*

```python
def lengthOfLongestSubstring(s: str) -> int:
    # lastSeen maps each character to the last index it appeared at
    lastSeen = {}
    left = 0    # left pointer of the window
    best = 0    # length of the best window so far

    for right, c in enumerate(s):
        # If 'c' was seen before and its last occurrence is inside the window
        if c in lastSeen and lastSeen[c] >= left:
            # Move 'left' to one position after the previous occurrence
            left = lastSeen[c] + 1

        # Update the last seen index for this character
        lastSeen[c] = right

        # Calculate current window length and update best
        current_len = right - left + 1
        if current_len > best:
            best = current_len

    return best
```

\*"Here’s what’s happening:

* `lastSeen` helps us jump the `left` pointer directly, avoiding unnecessary loops.
* At each step, the current window is from `left` to `right`.
* If the current character was seen inside the window, we skip past its old position.
* Otherwise, we just expand the window.
* We keep track of the max length in `best`."\*

---

*"Now I’ll quickly write the JavaScript version."*

```javascript
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map();
  let left = 0;
  let best = 0;

  for (let right = 0; right < s.length; right++) {
    const c = s[right];

    if (lastSeen.has(c) && lastSeen.get(c) >= left) {
      left = lastSeen.get(c) + 1;
    }

    lastSeen.set(c, right);

    const currentLen = right - left + 1;
    if (currentLen > best) best = currentLen;
  }

  return best;
}
```

---

**5️⃣ Discuss time/space complexity**

*"Time complexity is O(n) because both `left` and `right` pointers only move forward, and each character’s index is updated at most once.
Space complexity is O(min(n, Σ)) where Σ is the character set size, because the `lastSeen` dictionary stores at most one entry per distinct character in the current window."*

---

**6️⃣ Mention follow-up questions**

*"Possible follow-up questions the interviewer might ask:*

* How would you also return the substring itself, not just the length?
* How would you modify this to allow at most *k* repeated characters?
* How would you handle Unicode grapheme clusters, like emojis that take up two code units in JavaScript?
* How would you solve it if the input is a stream of characters rather than a string in memory?"\*

#
#
## 🎯 Real-World Applications｜實際應用場景


### 1. **User Session Tracking**｜**使用者工作階段追蹤**

**EN:**
When tracking user activity in a session (e.g., keystrokes, page views, or API calls), you might want to detect the longest continuous period without repeating a certain action or event. The sliding window technique from this problem can efficiently identify such periods.

**ZH:**
在追蹤使用者工作階段（例如鍵盤輸入、瀏覽頁面或 API 請求）時，可能需要找出**最長的連續時間區段**，其中沒有重複發生某個特定動作或事件。這題使用的滑動視窗技巧，可以高效地找到這樣的區段。

---

### 2. **Data Stream Processing**｜**資料流處理**

**EN:**
In systems that process continuous data streams (like network packets or IoT sensor readings), you might need to find the longest span without duplicate identifiers or events. This is especially useful for anomaly detection or ensuring uniqueness in a given time window.

**ZH:**
在處理連續資料流（如網路封包或 IoT 感測器讀數）的系統中，可能需要找出最長的一段資料，期間沒有重複的識別碼或事件。這對異常偵測、或在特定時間視窗中確保唯一性特別有用。

---

### 3. **Input Validation and Formatting**｜**輸入驗證與格式化**

**EN:**
When validating form input — for example, passwords or usernames — some rules may forbid consecutive repeats or require a sequence of unique characters. This algorithm can be adapted to check the maximum unique-character run in the input.

**ZH:**
在表單輸入驗證中，例如密碼或使用者名稱，有些規則會禁止連續重複字元，或要求一定長度的唯一字元序列。這個演算法可以改寫來檢查輸入中最長的唯一字元序列是否符合規範。

---

### 4. **Text and Log Analysis**｜**文字與日誌分析**

**EN:**
In text analytics or log file processing, you may need to extract the longest substring of unique characters to study patterns, clean data, or compress repeated sequences for storage optimization.

**ZH:**
在文字分析或日誌檔處理中，可能需要找出最長的不重複字元子字串，用來分析模式、清理資料，或將重複序列壓縮以優化儲存。

---

### 5. **Game Development**｜**遊戲開發**

**EN:**
In game mechanics involving sequences of moves, you may want to reward the player for making the longest streak of non-repetitive moves or actions. This logic can be used to evaluate and score such sequences.

**ZH:**
在遊戲機制中，如果遊戲包含動作或移動的連續記錄，可能需要計算玩家**最長的不重複動作連續串**，以給予額外獎勵或評分。這題的邏輯可以直接應用到這種情境。
