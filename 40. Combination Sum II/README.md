
# 🧩 LeetCode 40 – Combination Sum II | UMPIRE Notes

### 🧠 Understand  
We are given a list of **positive integers** `candidates`, which **may contain duplicates**, and an integer `target`.  
Our goal is to find all **unique combinations** where:
- The numbers sum up to `target`.
- Each number in `candidates` can be **used only once**.
- Combinations with the same elements but in different order are considered the same.

---

### 🔍 Match  
This is a **backtracking** problem with two special constraints:
- Each number can only be used once.
- Duplicate combinations must be skipped.

To manage this:
- We **sort** the array to bring duplicates together.
- We **skip** duplicates during iteration using:  
  `if i > start and candidates[i] == candidates[i - 1]: continue`

---

### 📝 Plan  
- Sort the candidates list.
- Use a recursive helper function with:
  - `start`: the current index
  - `path`: the current combination
  - Either `total` or `remaining`
- Base cases:
  - If `total == target` or `remaining == 0`, we add a copy of `path` to results.
  - If `total > target` or `remaining < 0`, we stop recursion.
- In the loop:
  - Skip duplicates in the same recursion layer.
  - Pick the current number.
  - Recurse with next index (`i + 1`) since each number can only be used once.
  - Backtrack (remove last added number).

---

### 🔧 Implement

#### 🐍 Python Version 1 – Using `total`
```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()  # 🔶 Sort to detect duplicates

    def backtrack(start, path, total):
        if total == target:
            res.append(path[:])  # 🎯 Found valid combination
            return
        if total > target:
            return  # ❌ Prune the search

        for i in range(start, len(candidates)):
            # ⛔ Skip duplicates in the same recursive level
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            path.append(candidates[i])  # ✅ Choose current number
            backtrack(i + 1, path, total + candidates[i])  # Move to next index
            path.pop()  # ⏪ Undo the choice (backtrack)

    backtrack(0, [], 0)
    return res
````

#### 🐍 Python Version 2 – Using `remaining`

```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()

    def backtrack(start, path, remaining):
        if remaining == 0:
            res.append(path[:])
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            if candidates[i] > remaining:
                break  # 🚫 No need to continue further
            path.append(candidates[i])
            backtrack(i + 1, path, remaining - candidates[i])
            path.pop()

    backtrack(0, [], target)
    return res
```



#### 🌐 JavaScript Version 1 – Using `total`

```javascript
function combinationSum2(candidates, target) {
    const res = [];
    candidates.sort((a, b) => a - b);  // 🔶 Sort the array

    function backtrack(start, path, total) {
        if (total === target) {
            res.push([...path]);  // 🎯 Found a valid combination
            return;
        }
        if (total > target) return;  // ❌ Prune

        for (let i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] === candidates[i - 1]) continue;  // ⛔ Skip duplicates
            path.push(candidates[i]);  // ✅ Choose
            backtrack(i + 1, path, total + candidates[i]);  // i+1 because we cannot reuse
            path.pop();  // ⏪ Undo the choice
        }
    }

    backtrack(0, [], 0);
    return res;
}
```

#### 🌐 JavaScript Version 2 – Using `remaining`

```javascript
function combinationSum2(candidates, target) {
    const res = [];
    candidates.sort((a, b) => a - b);

    function backtrack(start, path, remaining) {
        if (remaining === 0) {
            res.push([...path]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] === candidates[i - 1]) continue;
            if (candidates[i] > remaining) break;
            path.push(candidates[i]);
            backtrack(i + 1, path, remaining - candidates[i]);
            path.pop();
        }
    }

    backtrack(0, [], target);
    return res;
}
```

---

### 🔍 Review

After implementing the solution, I would test it on multiple cases:

* `[2,3,6,7], target = 7` → `[[7], [2,2,3]]`
* `[10,1,2,7,6,1,5], target = 8` → `[[1,1,6],[1,2,5],[1,7],[2,6]]`
* `[1,1,1,1], target = 2` → `[[1,1]]`
* `[], target = 3` → `[]`
* Large inputs with many duplicates

Also verify:

* No reused numbers
* No duplicate combinations
* Order of result doesn't matter

---

### 📈 Evaluate

**Time Complexity**:

* Worst: O(2^n), exponential due to all combinations
* Pruning and skipping duplicates help reduce actual runtime

**Space Complexity**:

* O(n) recursion stack + result storage

#
#
#

### 🌸 中文版本 – UMPIRE 面試筆記

### 🧠 Understand（理解問題）

給你一個整數陣列 `candidates`（可能有重複），以及一個整數 `target`。
你要找出所有不重複的組合，使得組合中的數字加總為 `target`。
**每個數字只能使用一次**，組合不能出現重複（例如 `[1,2,5]` 和 `[2,1,5]` 被視為同一組）。

---

### 🔍 Match（匹配問題）

這是典型的 **回溯法 backtracking** 題型，有兩個重點：

* 每個元素只能用一次。
* 避免重複組合。

為此我們會：

* 先 `sort()` 陣列，讓重複元素靠在一起。
* 在每層遞迴中，使用：

  ```python
  if i > start and candidates[i] == candidates[i - 1]:
      continue
  ```

  來跳過重複。

---

### 📝 Plan（規劃）

* 對輸入排序。

* 定義一個 backtrack 函數，參數包含：

  * `start`：目前遞迴的起點。
  * `path`：目前組合。
  * `total` 或 `remaining`：目前總和或剩餘目標。

* 遞迴中：

  * 如果 total == target → 加入結果。
  * 如果 total > target → 剪枝。
  * 遍歷選項：

    * 跳過同層重複。
    * 選擇當前數字。
    * 遞迴進入下一層（使用 i+1，因為不能重複用）。
    * 回溯（pop 最後一個數字）

---

### 🔧 Implement（實作）

參見英文段落中程式碼
（Python 與 JavaScript 兩版本、兩種寫法都有完整註解）

---

### 🔍 Review（檢查與測試）

測試以下情況確認正確性：

* `[2,3,6,7], target = 7` → `[[7], [2,2,3]]`
* `[10,1,2,7,6,1,5], target = 8` → `[[1,1,6],[1,2,5],[1,7],[2,6]]`
* `[1,1,1,1], target = 2` → `[[1,1]]`
* 空陣列 → `[]`
* 多重複元素的大型輸入

也需確認：

* 每個數字只用一次。
* 沒有重複組合。
* 組合順序不影響結果。

---

### 📈 Evaluate（評估）

**時間複雜度**：

* 最差為 O(2^n)，因為是組合型問題。
* 實作中透過剪枝與跳過重複改善效能。

**空間複雜度**：

* O(n)：遞迴深度與 path 長度。
* 加上儲存結果所需空間。

  #
  #
  #

# 💬 Spoken-style Explanation for remaining-based Backtracking – LeetCode 40

### 🎯 1. Clarify the problem

> “Sure! So we are given a list of integers called `candidates`, and an integer `target`. Our goal is to find all **unique combinations** of numbers from `candidates` that sum up to the `target`.
> Each number in the list can **only be used once** in a combination, and we need to **avoid duplicate combinations**, even if the numbers appear in different orders.
> For example, if two 1s exist in the input, we need to make sure we only count unique combinations and don’t return `[1,2,5]` twice.”

---

### ⚠️ 2. Discuss edge cases

> “Some edge cases I’d consider are:

* If `candidates` is empty, we should return an empty list.
* If all numbers are greater than the target, no valid combinations exist.
* If `candidates` has many duplicates, like `[1,1,1,1]`, we need to handle them carefully to avoid duplicate combinations.
* If the target is 0, we should probably return an empty list or only include empty combinations if allowed—though in this problem, that shouldn't happen since `1 <= target`.”

---

### 🧠 3. Consider brute-force and optimal approach

> “A brute-force approach would be to try every combination using backtracking, and then store them in a set or use sorting to filter out duplicates.
> However, that’s not efficient.
> Instead, an optimal approach involves **sorting the input first**, and during backtracking, **skipping duplicate values** at the same recursion level.
> This way, we can avoid exploring duplicate paths and make the search more efficient.”

---

### 👨‍💻 4. Explain and implement optimal code

> “Let me walk you through my backtracking solution.
> I’ll sort the input first to group duplicates together. Then I’ll define a helper function `backtrack(start, path, remaining)`:

* `start` tells us where to begin in the candidate list (so we don’t reuse numbers).
* `path` is our current combination.
* `remaining` is how much we still need to reach the target.

During the loop, we’ll skip duplicates by checking `if i > start and candidates[i] == candidates[i - 1]`.
This ensures we only use one occurrence of the same value at the same recursion level.”

#### 🐍 Python Code (spoken while typing)

```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()

    def backtrack(start, path, remaining):
        if remaining == 0:
            res.append(path[:])
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i - 1]:
                continue  # skip duplicates
            if candidates[i] > remaining:
                break  # prune
            path.append(candidates[i])
            backtrack(i + 1, path, remaining - candidates[i])
            path.pop()  # backtrack

    backtrack(0, [], target)
    return res
```

> “So here, we use `path.pop()` to undo the last decision so we can try the next candidate.
> Also, we break the loop early if the current number is larger than the remaining target.”

#### 🌐 JavaScript Version

```javascript
function combinationSum2(candidates, target) {
    const res = [];
    candidates.sort((a, b) => a - b);

    function backtrack(start, path, remaining) {
        if (remaining === 0) {
            res.push([...path]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] === candidates[i - 1]) continue;
            if (candidates[i] > remaining) break;

            path.push(candidates[i]);
            backtrack(i + 1, path, remaining - candidates[i]);
            path.pop();
        }
    }

    backtrack(0, [], target);
    return res;
}
```

---

### 📈 5. Discuss time and space complexity

> “Now let’s talk about the complexity.
> The time complexity in the worst case is **O(2^n)**, since we’re exploring subsets of the input.
> However, due to pruning and duplicate-skipping, it performs better in practice.
> The space complexity is **O(n)** for the recursion stack and the temporary path list, plus the space for storing the result combinations.”

---

### 💬 6. Mention follow-up questions

> “Some potential follow-up questions might include:

* How would we modify the code to return the number of combinations instead of the combinations themselves?
* What if each number could be used unlimited times? (That’s actually `Combination Sum I`, LeetCode 39.)
* Could we do it without recursion, using iteration and a stack?
* What if we wanted to limit the length of each combination?”

#
#
#

# 💬 Spoken-style Explanation for `total`-based Backtracking – LeetCode 40


### 🎯 1. Clarify the problem

> "Sure! We're given a list of integers called `candidates` and an integer `target`.
> Our goal is to find all **unique combinations** of numbers from the list that add up to exactly `target`.
> Each number in `candidates` can only be used **once per combination**, and we must **avoid duplicate combinations**, meaning `[1,2,5]` is the same as `[2,1,5]` and should only appear once.
> Also, the input may contain duplicate numbers, so we have to be very careful not to return repeated combinations."

---

### ⚠️ 2. Discuss edge cases

> "Here are some edge cases to watch for:

* If `candidates` is an empty list, then there are no combinations.
* If all numbers are larger than `target`, we can't form any combination.
* If `candidates` contains many duplicates like `[1,1,1,1]`, we must skip over the extras to avoid duplicate answers.
* If the `target` is smaller than the smallest number in `candidates`, again we return an empty list."

---

### 🧠 3. Consider brute-force and optimal approach

> "We could brute-force every possible subset and check whether their sum equals `target`, but that's super inefficient and leads to lots of duplicate combinations.
> A better approach is **backtracking** with **pruning** and **duplicate skipping**.
> By sorting the array first, we can group duplicates together. Then during recursion, if we see the same number as the previous one and it's not the first in that layer, we skip it.
> Also, we can stop early if our running `total` exceeds the `target`."

---

### 👨‍💻 4. Explain and implement optimal code (total version)

> "Let me walk you through the code.
> We'll define a helper function `backtrack(start, path, total)` where:

* `start` is the index where we begin searching.
* `path` stores the current combination.
* `total` keeps track of the sum so far.

We’ll prune branches where `total > target`, and skip duplicate values at the same recursion depth."

#### 🐍 Python (with inline explanation)

```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()  # Step 1: Sort for duplicate skipping

    def backtrack(start, path, total):
        if total == target:
            res.append(path[:])  # Found valid combination
            return
        if total > target:
            return  # Prune: no need to explore further

        for i in range(start, len(candidates)):
            # Skip duplicates in the same recursion level
            if i > start and candidates[i] == candidates[i - 1]:
                continue

            path.append(candidates[i])  # Choose current number
            # Move to next index to avoid reusing the same element
            backtrack(i + 1, path, total + candidates[i])
            path.pop()  # Backtrack: undo the choice

    backtrack(0, [], 0)  # Start backtracking
    return res
```

> "Here, we're keeping track of the total sum instead of calculating it from scratch.
> If the sum exceeds the target, we stop early.
> And for skipping duplicates, we only skip if `i > start and candidates[i] == candidates[i - 1]`, which ensures that we don't use the same value at the same tree level."

---

### 📈 5. Discuss time and space complexity

> "Time complexity is roughly O(2^n) in the worst case because we explore subsets.
> However, pruning and duplicate checks reduce the number of calls significantly.
> Space complexity is O(n) for the recursion stack and the current path, plus the result list."

---

### 💬 6. Mention follow-up questions

> "Some good follow-ups might include:

* How would this change if each number could be reused multiple times?
* How would we do this iteratively?
* What if the list was extremely large, and we needed to stream partial results?"
