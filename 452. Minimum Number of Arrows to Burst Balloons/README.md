# 🎯 LeetCode 452 — Minimum Number of Arrows to Burst Balloons｜UMPIRE Bilingual Notes

### 📘 UMPIRE (English Version)

#### 🧠 Understand

* You’re given closed intervals `[start, end]` on a number line.
* Shooting one arrow at coordinate `x` bursts every balloon whose interval contains `x` (including when `x == start` or `x == end`).
* Goal: **minimize** the number of arrows to burst **all** balloons.
* Corner cases: empty list → `0`, single balloon → `1`, negative numbers and duplicates are fine.

#### 🧩 Match

* This is the classic **“minimum points to cover intervals”** problem.
* A standard greedy works: **sort by interval end ascending**, then always place an arrow at the **current earliest end**; keep using that arrow for every interval that starts **≤** that arrow position.

**Why greedy is optimal (intuition):**
Choosing the **earliest finishing endpoint** never hurts—you can “exchange” any other choice with this one without losing coverage and it frees up room to cover more intervals later.

#### 📝 Plan

1. If `points` is empty, return `0`.
2. Sort by `end` ascending.
3. Place the first arrow at `arrow_pos = points[0][1]`; set `arrows = 1`.
4. Scan remaining intervals:

   * If `current.start > arrow_pos` → need a **new** arrow. Increment `arrows` and set `arrow_pos = current.end`.
   * Else (overlap) → the current arrow already bursts it.
5. Return `arrows`.

#### 💻 Implement (Python 3, detailed comments)

##### ✅ Canonical Greedy: sort by end (recommended)

```python
from typing import List

class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        """
        Greedy by right endpoints (end ascending).
        Idea:
          - Place arrow at the earliest finishing time.
          - Reuse that arrow for all intervals that start <= arrow_pos (closed intervals).
          - When an interval starts AFTER arrow_pos (>), we need a new arrow at its end.

        Time  : O(n log n) for sorting
        Space : O(1) extra (ignoring sort’s stack/implementation details)
        """
        # 0) Empty input -> 0 arrows
        if not points:
            return 0

        # 1) Sort intervals by their right endpoint (end) ascending
        points.sort(key=lambda x: x[1])

        # 2) Use the first interval's end as initial arrow position
        arrows = 1
        arrow_pos = points[0][1]

        # 3) Sweep through the remaining intervals
        for start, end in points[1:]:
            # Closed intervals: if start == arrow_pos, the arrow still hits it.
            # So ONLY when start > arrow_pos (strictly), we need a new arrow.
            if start > arrow_pos:
                arrows += 1
                arrow_pos = end  # place the new arrow at this interval's end

        return arrows
```

##### 🔁 Equivalent Greedy: sort by start + shrink intersection

```python
from typing import List

class SolutionAlt:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        """
        Equivalent approach:
          - Sort by start ascending.
          - Maintain the "overlap window" right boundary (overlap_right).
          - When the next interval overlaps, shrink overlap_right = min(overlap_right, end).
          - When it doesn't (start > overlap_right), we need a new arrow and reset the window.

        Time  : O(n log n)
        Space : O(1) extra
        """
        if not points:
            return 0

        points.sort(key=lambda x: x[0])  # sort by start
        arrows = 1
        _, overlap_right = points[0]     # current overlap's right boundary

        for start, end in points[1:]:
            if start <= overlap_right:
                # Keep only the common overlap for the current arrow
                overlap_right = min(overlap_right, end)
            else:
                # No overlap -> need a new arrow and a new overlap window
                arrows += 1
                overlap_right = end

        return arrows
```

#### 🔍 Review

* Example: `[[10,16],[2,8],[1,6],[7,12]]`
  Sort by end → `[[1,6],[2,8],[7,12],[10,16]]`
  Arrow at `6` hits `[1,6]` & `[2,8]`; next `[7,12]` starts > `6` → new arrow at `12`, which also hits `[10,16]` → total `2`.

**Edge conditions & pitfalls**

* Use `>` (not `>=`) to detect “no-overlap,” because intervals are **closed**.
* The greedy choice is determined by **end**, not start.
* Large and negative coordinates are okay in Python.

#### 🚀 Evaluate

* Same pattern appears in:

  * 435\. Non-overlapping Intervals
  * 56. Merge Intervals
  * 1288. Remove Covered Intervals
  * 253. Meeting Rooms II

<br>

# 📗 UMPIRE（中文版本）

#### 🧭 理解題意

* 給你多個**閉區間**氣球 `[start, end]`。
* 在任意座標 `x` 射出一箭，凡是區間包含 `x` 的氣球都被射爆（包含 `x == start` 或 `x == end`）。
* 目標：用**最少**箭把**所有**氣球射爆。
* 邊界：空陣列→`0`；單一氣球→`1`；可有負數與重複端點。

#### 🏷️ 類比問題

* 屬於「**用最少的點覆蓋所有區間**」的經典題。
* 標準解法是**按右端點升序**的**貪心**：每次把箭放在**當前最早結束的右端點**，盡量覆蓋後續能重疊的區間。

**為什麼貪心正確（直覺）**：
把箭放在「最早結束點」不吃虧，可以把任何其他解法的箭“交換”到這裡而不變差，同時保留最多的未來選擇空間。

#### 📐 計畫

1. 若 `points` 為空→回傳 `0`。
2. 依 `end` 由小到大排序。
3. 第一支箭放在 `arrow_pos = points[0][1]`，`arrows = 1`。
4. 走訪後續區間：

   * 若 `start > arrow_pos` → 需要**新箭**，`arrows += 1` 並把 `arrow_pos = end`。
   * 否則（有重疊）→ 這支箭也能射爆，不用動作。
5. 回傳 `arrows`。

#### 🛠️ 程式實作（Python 3，含中文註解）

##### 🥇 主解：按右端點排序的貪心

```python
from typing import List

class SolutionCN:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        """
        觀念：
          - 將所有區間依「右端點」由小到大排序。
          - 將箭放在當前可處理集合的「最右端」(最早結束點)。
          - 後續若遇到 start > arrow_pos (嚴格大於，因為閉區間)，代表需要新箭。

        時間複雜度：O(n log n)（排序）
        空間複雜度：O(1) 額外
        """
        # 0) 空輸入
        if not points:
            return 0

        # 1) 依右端點排序
        points.sort(key=lambda p: p[1])

        # 2) 第一支箭放在第一個區間的右端
        arrows = 1
        arrow_pos = points[0][1]

        # 3) 逐一檢查後續區間
        for start, end in points[1:]:
            # 閉區間 => start == arrow_pos 仍視為能射中，所以用 ">" 判定不重疊
            if start > arrow_pos:
                arrows += 1
                arrow_pos = end

        return arrows
```

##### 🥈 等價法：按左端點排序 + 維護重疊右界

```python
from typing import List

class SolutionCNAlt:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        """
        觀念：
          - 先按左端點排序。
          - 維護「當前所有能由同一支箭射爆的共同交集右界」overlap_right。
          - 若新區間與目前交集重疊 -> overlap_right = min(overlap_right, end)
          - 若不重疊 (start > overlap_right) -> 需要新箭，並重設 overlap_right = end
        """
        if not points:
            return 0

        points.sort(key=lambda p: p[0])
        arrows = 1
        _, overlap_right = points[0]

        for start, end in points[1:]:
            if start <= overlap_right:
                overlap_right = min(overlap_right, end)
            else:
                arrows += 1
                overlap_right = end

        return arrows
```

#### 🧪 檢查

* 範例：`[[10,16],[2,8],[1,6],[7,12]]`
  依 `end` 排序 → `[[1,6],[2,8],[7,12],[10,16]]`
  `arrow_pos=6` 可射 `[1,6]` 與 `[2,8]`；遇到 `[7,12]` 因 `7 > 6` → 新箭放 `12`，也能射 `[10,16]` → 共 `2`。

**容易忽略**

* 判斷不重疊必須用 `>`（不是 `>=`），因為是**閉區間**。
* 關鍵在「依右端點排序」；若用左端點排序，記得要**縮小重疊右界**。

#### 🌟 反思

* 相關題型：
  435（刪最少使不重疊）、56（合併區間）、1288（移除被覆蓋區間）、253（最少會議室數）。

---

### ✍️ Extra Notes (EN)

#### ⚠️ Common pitfalls

* Using `>=` for non-overlap is **wrong** for closed intervals; must use **`>`**.
* Sorting by start and forgetting to **shrink the overlap** (via `min`) breaks the greedy.
* Accidentally writing `lamba` instead of `lambda`, or mis-typing the sort key.
* In interviews, if you forget `lambda x: x[1]`, use `from operator import itemgetter; key=itemgetter(1)` or define a tiny helper `def end(p): return p[1]`.

#### 📊 Complexity & correctness sketch

* Both solutions run in **O(n log n)** time (sorting dominates) and **O(1)** extra space.
* **Exchange argument**: if an optimal solution places an arrow anywhere inside an overlapping set, you can move it to the set’s **earliest end** without losing coverage—thus the greedy choice is safe.

#### 🎙️ Interview tips

* Say up front: “I’ll sort intervals by end ascending and greedily place arrows at these ends.”
* Call out closed-interval detail: “I’ll use `>` to detect no-overlap.”
* If time permits, mention the **start-sorted variant** to show breadth.

#### 🔗 Related problems to practice

* 435, 56, 1288, 253 — all share interval-greedy DNA.

---

### 🧾 附加筆記（中文）

#### 🚫 易錯點

* **閉區間**判斷：必用 `start > arrow_pos` 判斷「需要新箭」。
* 只換排序鍵卻不改邏輯會壞：左排序一定要「縮小重疊右界」。
* `lambda` 容易打錯；備案：`itemgetter(1)` 或小函式 `def end(p): return p[1]`。

#### 🧮 複雜度與正確性

* 時間 **O(n log n)**、額外空間 **O(1)**。
* 正確性憑藉「交換論證」：把箭移到**最早結束點**不會讓覆蓋變差。

#### 💡 面試小撇步

* 先講思路再寫碼，邊寫邊強調「閉區間」細節。
* 若卡在 `lambda`，**口頭說明**“取右端點排序”，再用 `itemgetter(1)` 實作。
* 寫完用一個能卡住 `>=` 的例子驗證（如 `[[1,2],[2,3]]` 應得 `1`）。

#### 🪜 延伸練習

* 做 435 / 56 / 1288 / 253，觀察哪些條件（排序鍵、比較運算子）是共通的。

<br>

## 🎤 Full Spoken-Style Interview Answer

*(Imagine the interviewer has just given you the problem. You've read it silently for about 30-60 seconds.)*

### **1. Clarify the Problem and Examples**

**(You - to the interviewer):**

"Okay, thank you for the problem. I've had a moment to read it. Just to make sure I'm on the same page, let me quickly restate my understanding."

*(You turn slightly away from the monitor, as if thinking, and then look back.)*

"So, we're given a list of integer pairs, where each pair `[x_start, x_end]` represents the horizontal width of a balloon. Our goal is to find the *minimum* number of arrows we need to shoot to burst all of them. An arrow shot vertically from position `x` on the x-axis will burst any balloon whose range includes `x`."

"Let me just walk through one of the examples to confirm my understanding. If we have `points = [[10,16], [2,8], [1,6], [7,12]]`..."

*(You might gesture with your hands as if drawing the intervals in the air.)*

"...we have four balloons. I can see that an arrow shot at, say, `x = 6` would burst both the `[1,6]` and `[2,8]` balloons, because 6 is within both of their ranges. Then, for the remaining balloons `[7,12]` and `[10,16]`, an arrow at `x = 11` or `x=12` could burst both of them. So, in total, that would be two arrows. The goal is to find this minimum number, which is 2. Does that sound correct?"

*(Interviewer will likely confirm. Now you have established a clear understanding.)*

### **2. Discuss Edge Cases**

**(You):**

"Great. Before I think about the algorithm, I'd like to quickly consider a few edge cases to make sure my solution will be robust."

  * "First, what if the input list, `points`, is empty? In that case, we don't have any balloons, so we wouldn't need any arrows. The function should return `0`."
  * "Second, what if there's only one balloon, like `[[1, 5]]`? We would clearly need just one arrow, so the output should be `1`."
  * "The constraints also mention that the coordinates can be large and negative, so my solution should handle that. Also, a balloon's start and end could be the same, like `[3, 3]`, which is essentially a point. My logic should work for that as well."

"Thinking about these cases helps frame the problem better."

### **3. Consider Brute-Force and Optimal Approach**

**(You):**

"Okay, now for the approach. A brute-force solution here seems difficult. We can't just test every possible x-coordinate to shoot from, because the coordinates can be very large, and it's not a discrete set of points. Trying all combinations of arrows would be far too slow."

"This problem feels like it's about making a series of smart, optimal choices. That immediately makes me think of a **Greedy Algorithm**."

*(This is a key moment where you state your high-level strategy.)*

"The greedy idea would be to use each arrow as effectively as possible. With every single arrow, we want to burst the maximum number of balloons we can."

"So, how do we make that greedy choice? To process the balloons in a consistent way, sorting them first seems like the right move. The big question is: **what do we sort by?** The start points or the end points?"

*(Pause for a second, as if considering the options.)*

"Let's think about that. If we sort by the start points, we might deal with a very long balloon first, like `[0, 1000]`. If we shoot an arrow for that one, we don't really know the best place to shoot it. But... if we sort by the **end points**, the strategy becomes much clearer."

"By sorting by the end points, we are always dealing with the balloon that is about to 'escape' first. This is the most urgent balloon to take care of. Once we identify this first-ending balloon, we have to shoot an arrow to burst it. To maximize the utility of this one arrow, where should we shoot it? The best place would be at its very end point, `x_end`. Why? Because that position is the right-most possible point that can still burst this urgent balloon, which gives us the best chance to also hit other balloons that might overlap with it."

"So, my greedy strategy is:

1.  Sort all balloons by their end points.
2.  Take the first balloon, and shoot an arrow at its end point. This counts as one arrow.
3.  Then, iterate through the rest of the balloons. For each balloon, if it can be burst by our last arrow, we just move on. If it can't, it means we need a new arrow. We then shoot this new arrow at the *new* balloon's end point and repeat the process."

"This feels like a solid and efficient approach."

### **4. Explain and Implement Optimal Code**

**(You - as you start typing):**

"Alright, with that plan, I'll start coding now and I'll explain my thought process as I go."

```python
# (You start typing)
class Solution:
    def findMinArrowShots(self, points: list[list[int]]) -> int:
        # "First, I'll handle the edge case we talked about. 
        # If the points list is empty, we just return 0."
        if not points:
            return 0

        # "Next, the core of my greedy strategy: I'm sorting the points list. 
        # I'll use a lambda function as the key, to sort based on the second element, 
        # which is the end point of each balloon."
        points.sort(key=lambda x: x[1])

        # "Now, I'll initialize my variables. I'll need a counter for the arrows.
        # Since we know the list is not empty at this point, we will need at least one arrow.
        # So I'll start `arrows` at 1."
        arrows = 1
        
        # "I also need to keep track of where my last arrow was shot. 
        # For the first arrow, I'll shoot it at the end of the very first balloon in my sorted list.
        # I'll call this `arrow_pos`."
        arrow_pos = points[0][1]

        # "Okay, now I'll loop through the rest of the balloons, starting from the second one."
        # (You can use a more descriptive variable name in the loop for clarity)
        for balloon in points[1:]:
            # "Inside the loop, for each balloon, I need to make a decision.
            # Can my last arrow burst this balloon?
            # I can check this by looking at the balloon's start point.
            # If the balloon's start point is GREATER than my arrow's position..."
            if balloon[0] > arrow_pos:
                # "...it means this balloon starts after my arrow, so it can't be burst.
                # In this case, I need a new arrow."
                arrows += 1
                
                # "And since I'm using a new arrow, I need to update its position.
                # The best place for this new arrow is at the end of the CURRENT balloon."
                arrow_pos = balloon[1]
            
            # "If the balloon's start point is NOT greater than arrow_pos, 
            # it means it's overlapping, and my last arrow already takes care of it. 
            # So I don't need to do anything, and I can just continue to the next balloon."

        # "Finally, after the loop has finished, the `arrows` counter will hold the minimum number needed.
        # So I'll just return it."
        return arrows
```

### **5. Discuss Time/Space Complexity**

**(You - after finishing the code):**

"Okay, the code is complete. Now I'd like to quickly discuss its complexity."

  * "For **Time Complexity**, the most expensive operation here is the initial sort. Sorting a list of N balloons will take **O(N log N)** time. After the sort, I have a single loop that goes through all the balloons once, which is an O(N) operation. Therefore, the total time complexity is dominated by the sorting step, which is **O(N log N)**."

  * "For **Space Complexity**, I am modifying the list in-place with `.sort()`. The space used by the sorting algorithm itself in Python (Timsort) is O(N) in the worst case and O(log N) on average for its internal stack. I'm not creating any other data structures that scale with the input size. So, the space complexity would be **O(N)** in the worst case."

### **6. Mention Follow-up Questions**

**(You):**

"This greedy approach seems to be quite efficient and correctly solves the problem. If we had more time, some interesting follow-up questions could be:"

  * "What if the arrows had a blast radius, meaning they could burst balloons that are close but not directly on the line of fire?"
  * "Or, what if we had a limited number of arrows, and we wanted to burst the maximum number of balloons possible? That might change the approach towards something like dynamic programming."

"But for the current problem, I believe this solution is complete and optimal. Do you have any questions for me?"
