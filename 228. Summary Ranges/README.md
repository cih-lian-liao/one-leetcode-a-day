

# 📝 LeetCode 228: Summary Ranges (English UMPIRE Notes)

### 🔍 Understand

#### Problem

You are given a **sorted array of unique integers**. Summarize the consecutive numbers into compact ranges:

* Single number → `"x"`
* Range from `a` to `b` → `"a->b"`

#### Examples

* `[-1,0,1,2,4,5,7] → ["-1->2","4->5","7"]`
* `[0,2,3,4,6,8,9] → ["0","2->4","6","8->9"]`
* `[] → []`
* `[5] → ["5"]`

---

### 🧩 Match

* Array is sorted and strictly increasing.
* We can group consecutive runs (`nums[i+1] == nums[i] + 1`).
* Two typical approaches:

  1. **Single pointer (while loop cursor)**
  2. **Two pointers (`left`, `right`)**

---

### 🛠 Plan

#### Single Pointer (while loop)

* Use `i` to scan.
* Mark `start = nums[i]`.
* Extend while consecutive.
* Append `"start->end"` or single `"x"`.

#### Two Pointers

* `left` marks the start of a range.
* `right` expands until break.
* Append range and move `left = right + 1`.

---

### 💻 Implementations

#### 🌟 Version 1: Single Pointer (while loop)

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        ans = []   # Store the final result
        i = 0      # Cursor pointer

        while i < len(nums):
            start = nums[i]  # Mark the beginning of a range

            # Extend the range while consecutive
            while i < len(nums) - 1 and nums[i] + 1 == nums[i + 1]:
                i += 1

            # nums[i] is now the end of the range
            if start != nums[i]:
                ans.append(f"{start}->{nums[i]}")
            else:
                ans.append(str(nums[i]))

            i += 1  # Move cursor forward
        return ans
```

---

#### 🌟 Version 2: Two Pointers (left/right)

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        result = []
        n = len(nums)
        left = 0  # Left pointer marks start

        for right in range(n):
            # If last element OR break in consecutiveness
            if right == n - 1 or nums[right] + 1 != nums[right + 1]:
                if left == right:
                    result.append(str(nums[left]))
                else:
                    result.append(f"{nums[left]}->{nums[right]}")
                # Reset left for next range
                left = right + 1
        return result
```

---

### 🔎 Review

* Both methods handle:

  * Empty input
  * Single element
  * All consecutive
  * All isolated numbers

---

### ⚡ Evaluate

* **Time Complexity**: `O(n)` (each element visited at most twice).
* **Space Complexity**: `O(1)` extra (excluding output).

<br>

# 🌸 中文 UMPIRE 筆記 (LeetCode 228: 區間總結)

### 🔍 理解題意

給定一個 **已排序且不重複的整數陣列**，需要將連續的區間轉換成字串：

* 單一數字 → `"x"`
* 區間 `a` 到 `b` → `"a->b"`

#### 範例

* `[-1,0,1,2,4,5,7] → ["-1->2","4->5","7"]`
* `[0,2,3,4,6,8,9] → ["0","2->4","6","8->9"]`
* `[] → []`
* `[5] → ["5"]`

---

### 🧩 思路匹配

* 陣列已經排序、嚴格遞增。
* 可以分成兩種典型解法：

  1. **單指針 while 游標法**
  2. **雙指針 left/right 法**

---

### 🛠 解題計劃

#### 單指針

* 用 `i` 掃陣列。
* `start = nums[i]` 當區間起點。
* 內層 while 延伸區間直到不連續。
* 輸出 `"start->end"` 或單一數字。

#### 雙指針

* `left` 指向區間起點。
* `right` 移動直到不連續。
* 輸出 `left..right`，然後把 `left = right + 1`。

---

### 💻 程式碼實作

#### 🌟 版本一：單指針 while 游標法

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        ans = []   # 存放結果
        i = 0      # 游標指標

        while i < len(nums):
            start = nums[i]  # 起點

            # 延伸這段區間直到不連續
            while i < len(nums) - 1 and nums[i] + 1 == nums[i + 1]:
                i += 1

            # nums[i] 是區間的終點
            if start != nums[i]:
                ans.append(f"{start}->{nums[i]}")
            else:
                ans.append(str(nums[i]))

            i += 1  # 移到下一個數字
        return ans
```

---

#### 🌟 版本二：雙指針 left/right 法

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        result = []
        n = len(nums)
        left = 0   # 區間起點

        for right in range(n):
            # 如果是最後一個元素或下一個不連續
            if right == n - 1 or nums[right] + 1 != nums[right + 1]:
                if left == right:
                    result.append(str(nums[left]))
                else:
                    result.append(f"{nums[left]}->{nums[right]}")
                # 更新 left 指向下一段起點
                left = right + 1
        return result
```

---

### 🔎 檢查

* ✅ 空陣列 → `[]`
* ✅ 單一元素 → `["x"]`
* ✅ 全部連續 → `["a->b"]`
* ✅ 全部不連續 → 每個獨立輸出

---

### ⚡ 複雜度分析

* **時間**：`O(n)`
* **空間**：`O(1)` 額外空間

<br>

# 📌 附加筆記

* **單指針法**：程式碼更短，適合熟悉 while loop 的人。
* **雙指針法**：邏輯直觀，適合面試時清楚表達「起點 → 終點 → 收段 → 繼續下一段」。
* 都必須注意 **flush 最後一段**，否則最後區間會漏掉。
* 記得測試邊界案例：`[]`、`[5]`、`[1,2,3]`、`[1,3,5]`。

<br>

# 🎤 Full Spoken-Style Interview Answer (with while loop version)

### 1. Clarify the problem and read examples/constraints

**What you can say:**

> Let me first make sure I understand the problem.
> We are given a sorted array of unique integers. I need to summarize the consecutive numbers into compact string ranges.
> For example, if the input is `[-1,0,1,2,4,5,7]`, the output should be `["-1->2","4->5","7"]`.
> Another example, `[0,2,3,4,6,8,9]` should give `["0","2->4","6","8->9"]`.
> If the array is empty, then the result should be just an empty list.
> Okay, that makes sense.

---

### 2. Discuss edge cases

**What you can say:**

> I also want to think about edge cases.
>
> * If the array is empty, return an empty list.
> * If the array has only one element, like `[5]`, the result is `["5"]`.
> * If all numbers are consecutive, like `[1,2,3,4]`, then the result is just one range `["1->4"]`.
> * If none are consecutive, like `[1,3,5]`, then each becomes its own single number string.
>   These cases make sure my logic works in all scenarios.

---

### 3. Consider brute-force and optimal approach

**What you can say:**

> A brute-force solution would be: for each number, scan forward until the sequence breaks, then output it. But even that is still O(n).
> Since the array is sorted, we can solve it in one single pass, which is optimal.
> The approach is:
>
> * Use a pointer `i`.
> * Mark the start of a range.
> * Move forward while numbers are consecutive.
> * Once it breaks, record the range and continue.
>   That way, every number is processed once, and it’s very efficient.

---

### 4. Explain and implement optimal code (while loop version)

**What you can say while coding (spoken walkthrough):**

> Okay, let me write the code using a while loop and a single pointer.

```python
from typing import List

class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        result = []     # Store final ranges
        i = 0           # Pointer that scans through the array

        while i < len(nums):
            start = nums[i]  # Mark the beginning of the current range

            # Extend the range while next numbers are consecutive
            while i < len(nums) - 1 and nums[i] + 1 == nums[i + 1]:
                i += 1

            # Now nums[i] is the end of the current range
            if start != nums[i]:
                result.append(f"{start}->{nums[i]}")
            else:
                result.append(str(nums[i]))

            # Move to the next element after finishing this range
            i += 1

        return result
```

**What you can say (explaining the code):**

> Here’s how this works.
> I use a variable `i` to scan through the array. At the start of each range, I store `start = nums[i]`.
> Then I use an inner while loop to extend the sequence as long as the numbers are consecutive.
> When the inner loop ends, I know `nums[i]` is the last number of this range.
> If start equals end, I just append it as a single number. Otherwise, I append `"start->end"`.
> Finally, I move `i` forward by one to continue with the next potential range.

---

### 5. Discuss time and space complexity

**What you can say:**

> The time complexity is O(n). Here’s why: every element is processed exactly once. When I look at an element, either it extends the current consecutive range inside the inner loop, or it becomes the end of a range when the loop breaks. In either case, the pointer i always moves forward and never goes backward. That means each number is visited only once throughout the algorithm. So overall, the complexity is linear in the size of the input array, O(n).

> The space complexity is O(1) extra, aside from the output list.

---

### 6. Mention follow-up questions

**What you can say:**

> Some possible follow-up questions could be:
>
> * What if the input array is not sorted? Then I would sort it first, which makes the complexity O(n log n).
> * What if duplicates are allowed? Then we need to decide whether duplicates should be merged into ranges or kept separate.
> * How would we handle streaming data, where numbers arrive one by one? In that case, we could maintain an open range and flush it whenever a gap occurs.
