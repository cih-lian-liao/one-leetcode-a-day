
# 🎯 LeetCode 125: Valid Palindrome

> Given a string `s`, return `true` if it is a palindrome, or `false` otherwise. Only consider alphanumeric characters and ignore cases.


#### 🧠 UMPIRE Method (English Version)

### 🧩 U - Understand

#### Problem:

Check if the input string `s` is a valid palindrome.
Ignore:

* Cases (e.g., `'A'` == `'a'`)
* Non-alphanumeric characters (e.g., punctuation, spaces)

#### Input:

* A string `s` of length `n`

#### Output:

* Boolean: `True` if it's a palindrome, `False` otherwise

---

### 🧮 M - Match

This problem is a classic **two-pointer** pattern.
We move two pointers from both ends toward the center, comparing characters while skipping non-alphanumeric characters.

---

### 🧷 P - Plan

1. Initialize two pointers: `left = 0`, `right = len(s) - 1`
2. While `left < right`:

   * Skip non-alphanumeric characters using a helper or `isalnum()`
   * Compare lowercase of `s[left]` and `s[right]`
   * If unequal → return `False`
   * Else → move `left++`, `right--`
3. If loop completes → return `True`

---

### 🐍 I - Implement (Python)

```python
def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1

    while left < right:
        # Skip non-alphanumeric characters from the left
        while left < right and not s[left].isalnum():
            left += 1

        # Skip non-alphanumeric characters from the right
        while left < right and not s[right].isalnum():
            right -= 1

        # Compare lowercase characters
        if s[left].lower() != s[right].lower():
            return False

        # Move inward
        left += 1
        right -= 1

    return True
```

---

### 📜 I - Implement (JavaScript)

```javascript
function isPalindrome(s) {
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    // Skip non-alphanumeric characters from the left
    while (left < right && !isAlphaNumeric(s[left])) {
      left++;
    }

    // Skip non-alphanumeric characters from the right
    while (left < right && !isAlphaNumeric(s[right])) {
      right--;
    }

    // Compare lowercase characters
    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }

    // Move inward
    left++;
    right--;
  }

  return true;
}

// Helper function to check alphanumeric character
function isAlphaNumeric(char) {
  return /^[a-z0-9]$/i.test(char);
}
```

---

### 🧪 R - Review

Test cases:

* `"A man, a plan, a canal: Panama"` → `True`
* `"race a car"` → `False`
* `" "` → `True`
* `"0P"` → `False`

---

### 🧾 E - Evaluate

**Time Complexity:** `O(n)` — each character is checked at most once
**Space Complexity:** `O(1)` — constant extra space if no new string is created

#
#

#### 📘 UMPIRE 方法（中文版）

### 🧩 U - 理解題目

#### 題目說明：

給定一個字串 `s`，判斷是否為回文，只考慮英數字元，並忽略大小寫。

#### 輸入：

* 一個長度為 `n` 的字串

#### 輸出：

* 布林值：如果是回文，回傳 `True`，否則回傳 `False`

---

### 🧮 M - 類型對應

這是一題典型的 **雙指針** 題目，從兩端開始比對英數字元是否一致。

---

### 🧷 P - 解題計畫

1. 設定兩個指針 `left = 0`, `right = len(s) - 1`
2. 當 `left < right`：

   * 跳過 `left` 和 `right` 不是英數的情況
   * 比較 `s[left]` 和 `s[right]` 的小寫形式是否相同
   * 不相等時回傳 `False`
   * 否則兩指針往中間靠攏
3. 最後回傳 `True`

---

### 🐍 I - Python 實作

```python
def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1

    while left < right:
        # 當左指針不是英數字元，往右移動
        while left < right and not s[left].isalnum():
            left += 1

        # 當右指針不是英數字元，往左移動
        while left < right and not s[right].isalnum():
            right -= 1

        # 比較兩邊字元的小寫形式是否相等
        if s[left].lower() != s[right].lower():
            return False

        # 雙指針往內縮
        left += 1
        right -= 1

    return True
```

---

### 📜 I - JavaScript 實作

```javascript
function isPalindrome(s) {
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    // 左邊不是英數則跳過
    while (left < right && !isAlphaNumeric(s[left])) {
      left++;
    }

    // 右邊不是英數則跳過
    while (left < right && !isAlphaNumeric(s[right])) {
      right--;
    }

    // 比較轉小寫後的字元是否一致
    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }

    // 雙指針往中間靠攏
    left++;
    right--;
  }

  return true;
}

// 自訂函式：判斷是否為英數字元
function isAlphaNumeric(char) {
  return /^[a-z0-9]$/i.test(char);
}
```

---

### 🧪 R - 測試案例

* `"A man, a plan, a canal: Panama"` ➜ `True`
* `"race a car"` ➜ `False`
* `" "` ➜ `True`
* `"0P"` ➜ `False`

---

### 🧾 E - 時間與空間複雜度分析

* **時間複雜度：** `O(n)`（每個字元最多處理一次）
* **空間複雜度：** `O(1)`（使用雙指針，無額外資料結構）

#
#
#


# 🎤 Full Spoken-Style Interview Answer



### ✅ 1. Clarify the problem

> “Okay, so just to make sure I understand the problem correctly:
> We're given a string `s`, and we need to determine whether it's a valid palindrome.
> A palindrome reads the same forward and backward.
> However, for this problem, we’re only supposed to consider **alphanumeric characters**, and we should **ignore cases**.
> So, spaces, punctuation, and other symbols should be skipped during comparison.”

---

### 🔍 2. Discuss edge cases

> “Let me quickly think through some edge cases:

* If the string is empty or contains only spaces or non-alphanumeric characters like `",."`, that should return `true`, since an empty or non-letter/number sequence technically counts as a palindrome.
* If the string has only one character, like `'a'` or `'9'`, it's also trivially a palindrome.
* If the string has mixed casing, like `'Aba'`, we should treat it as `'aba'`, which means it still counts as a palindrome.”

---

### 💡 3. Consider brute-force and optimal approach

> “A brute-force approach would be:

1. Clean the string by removing all non-alphanumeric characters and converting everything to lowercase.
2. Then simply reverse the string and check if it’s equal to the original cleaned version.

That works, but it creates a new string and uses extra space.

An **optimal approach** would be to use the **two-pointer technique**:

* Place one pointer at the beginning of the string, and one at the end.
* Move both toward the center, skipping non-alphanumeric characters and comparing the lowercase versions of the characters.
* If all comparisons match, it's a palindrome.”

---

### 💻 4. Explain and implement optimal code

> “Alright, let me go ahead and write the optimal two-pointer implementation.
> I'll write it in Python first, and then I can explain it line by line.”

```python
def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1

    while left < right:
        # Skip characters that are not letters or numbers
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1

        # Compare lowercase versions
        if s[left].lower() != s[right].lower():
            return False

        # Move both pointers inward
        left += 1
        right -= 1

    return True
```

> “So here’s what this code does:

* We initialize two pointers: `left` and `right`.
* While they haven’t crossed each other, we check:

  * If either character isn’t alphanumeric, we skip it.
  * If both are valid, we compare their lowercase versions.
  * If they don’t match, we return `False`.
* If we finish the loop without mismatches, we return `True`.

This avoids building a new string and handles everything in-place, which is both clean and efficient.”

> “Would you like me to write this in JavaScript as well?”

```javascript
function isPalindrome(s) {
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    while (left < right && !isAlphaNumeric(s[left])) {
      left++;
    }
    while (left < right && !isAlphaNumeric(s[right])) {
      right--;
    }

    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }

    left++;
    right--;
  }

  return true;
}

function isAlphaNumeric(char) {
  return /^[a-z0-9]$/i.test(char);
}
```

> “The JavaScript version follows the same logic, but since JavaScript doesn’t have an `isalnum()` method, we use a regex helper function to check for alphanumeric characters.”

---

### ⏱️ 5. Discuss time/space complexity

> “Let’s talk about time and space complexity:

* **Time complexity** is O(n), because each character is checked at most once.
* **Space complexity** is O(1), since we’re not allocating extra memory or new strings—just using two pointers.”

---

### 💬 6. Mention follow-up questions

> “Some follow-up questions that might come up are:

* Can you do this in a different language, or write unit tests for this function?
* What if the input is a stream of characters and you can’t store the full string?
* How would you handle Unicode or non-English alphabets?
* What if you're only allowed to use constant space, and not even helper functions?”

> “I’d be happy to walk through any of those next if you'd like.”

#
#
#


## 🎯 Real-World Applications｜實際應用場景


### 💬 Text Validation in Messaging Apps｜通訊軟體中的文字驗證

**EN:**
Messaging platforms like WhatsApp or Slack might use similar logic to clean up and analyze user messages — for instance, checking if the message is just noise or meaningless symbols.

**ZH：**
像 WhatsApp 或 Slack 這樣的通訊軟體，會使用類似的字串處理方式來分析使用者輸入的訊息，例如判斷訊息是否只是噪音或符號，是否為空白訊息。

---

### 🔐 Input Sanitization for Security｜輸入清理與資安

**EN:**
Before storing or processing user input (e.g., usernames, passwords, comments), many systems clean up special characters and normalize cases to prevent injection attacks or ensure consistent comparison.

**ZH：**
在儲存或處理使用者輸入之前（如帳號、密碼、留言），系統常會先清理特殊符號、統一大小寫，以避免注入攻擊，或保證資料比對的一致性。

---

### 🔄 Data Normalization in Search Engines｜搜尋引擎的資料正規化

**EN:**
Search engines often ignore punctuation and case when comparing queries to documents. Techniques used in this palindrome problem (like ignoring non-alphanumerics and case-insensitive comparison) are foundational for implementing search indexing and text normalization.

**ZH：**
搜尋引擎在比對查詢與文件時通常會忽略標點與大小寫。本題中用到的處理技巧（忽略非英數字元與大小寫）是建構搜尋索引與文字正規化的重要基礎。

---

### 🧠 Natural Language Processing (NLP) Preprocessing｜自然語言處理的預處理

**EN:**
In NLP tasks, text preprocessing often involves removing punctuation and converting to lowercase before feeding data into models. This problem helps reinforce those fundamentals.

**ZH：**
在自然語言處理任務中，常見的預處理動作就包括去除標點符號、轉換為小寫再送入模型。本題是這類基礎技能的絕佳練習。

---

### 🎮 String-Based Game Logic｜遊戲邏輯中的字串判斷

**EN:**
Some games or puzzles (e.g., word palindromes, hidden messages) rely on checking whether strings are palindromes after sanitization. Validating strings in this way is essential for implementing correct game mechanics.

**ZH：**
某些遊戲或益智題（如字串回文、密碼破解）會依賴回文判斷來運作。像這樣清理與比對字串的技巧，在設計正確的遊戲邏輯時不可或缺。

---

### 🧾 Summary｜總結

| 應用場景      | 技術重點         |
| --------- | ------------ |
| 通訊軟體驗證輸入  | 移除噪音字元、正規化比對 |
| 資安與輸入清理   | 避免惡意輸入、轉換大小寫 |
| 搜尋引擎比對查詢  | 忽略標點、統一格式    |
| NLP 文本處理  | 預處理步驟與資料乾淨度  |
| 遊戲邏輯與謎題設計 | 比對清理後的字串特徵   |


