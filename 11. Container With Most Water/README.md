
# 🐳 LeetCode 11. Container With Most Water

#### 🦄 UMPIRE Method (English Version)

### 🔍 U — Understand

We are given an array `height[]` representing vertical lines on the x-axis. Each index is the x-coordinate, and the value is the height of a vertical line.
We need to find **two lines** that, together with the x-axis, form a container that can store the **most water**.

* **Input:** An integer array `height[]`
* **Output:** Maximum area of water the container can hold

Formula:

```
area = min(height[left], height[right]) * (right - left)
```

---

### 🐙 M — Match

* Problem Type: **Two Pointer / Greedy**
* Key Pattern: Move inward from both ends to maximize area by shrinking the width and possibly increasing the height.
* Known strategy: **Always move the shorter line**, because that’s the bottleneck.

---

### 🦋 P — Plan

1. Initialize two pointers `left = 0`, `right = n - 1`
2. Calculate area using the shorter height and current width
3. Keep track of the `max_area`
4. Move the pointer pointing to the shorter height:

   * If `height[left] < height[right]`, move `left++`
   * Else, move `right--`
5. Repeat until `left >= right`
6. Return the `max_area`

---

### 🌊 I — Implement

#### 🐍 Python Code

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left = 0                          # Start pointer at the beginning
        right = len(height) - 1          # End pointer at the end
        max_area = 0                     # Track the maximum area

        while left < right:
            # Get the height and width for current container
            h = min(height[left], height[right])
            w = right - left
            area = h * w                 # Compute the area

            # Update maximum area if larger is found
            max_area = max(max_area, area)

            # Move the shorter line inward
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

#### 🌐 JavaScript Code

```javascript
var maxArea = function(height) {
    let left = 0;                      // Start from the leftmost line
    let right = height.length - 1;     // Start from the rightmost line
    let maxArea = 0;                   // Track maximum area

    while (left < right) {
        const h = Math.min(height[left], height[right]); // Shorter height
        const w = right - left;                          // Width
        const area = h * w;                              // Calculate area

        maxArea = Math.max(maxArea, area);               // Update if better

        // Move the shorter side inward
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxArea;
};
```



### 🐢 R — Review

#### ✅ Sample Test Cases:

```python
assert Solution().maxArea([1,8,6,2,5,4,8,3,7]) == 49
assert Solution().maxArea([1,1]) == 1
assert Solution().maxArea([4,3,2,1,4]) == 16
```

```javascript
console.log(maxArea([1,8,6,2,5,4,8,3,7])); // 49
console.log(maxArea([1,1]));              // 1
console.log(maxArea([4,3,2,1,4]));        // 16
```

---

### 🐬 E — Evaluate

* **Time Complexity:** `O(n)` — Each pointer moves only once.
* **Space Complexity:** `O(1)` — Only a few variables are used.

#
#

#### 🐳 UMPIRE 方法（中文版）

### 🔍 U — Understand｜理解題目

給你一個整數陣列 `height[]`，代表直立於 x 軸的豎線，每條線的座標是 `(i, height[i])`。
請找出兩條線，與 x 軸構成一個容器，**容量最大**。

* **輸入：** 整數陣列 `height[]`
* **輸出：** 可容納的最大水量

計算公式：

```
面積 = min(height[left], height[right]) × (right - left)
```

---

### 🐙 M — Match｜套用類型

* 題型：**雙指針 Two Pointers** + **貪心 Greedy**
* 核心技巧：從兩端夾擊，並移動**較短的那一側**

---

### 🦋 P — Plan｜解題計畫

1. 設置左右指針 `left = 0`，`right = n - 1`
2. 計算面積，保留最大值
3. 若左邊高度較小，`left += 1`；否則 `right -= 1`
4. 重複步驟直到 `left >= right`
5. 回傳最大面積 `max_area`

---

### 🌊 I — Implement｜實作程式

#### 🐍 Python 範例

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left = 0
        right = len(height) - 1
        max_area = 0

        while left < right:
            h = min(height[left], height[right])
            w = right - left
            area = h * w
            max_area = max(max_area, area)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

#### 🌐 JavaScript 範例

```javascript
var maxArea = function(height) {
    let left = 0;
    let right = height.length - 1;
    let maxArea = 0;

    while (left < right) {
        const h = Math.min(height[left], height[right]);
        const w = right - left;
        const area = h * w;

        maxArea = Math.max(maxArea, area);

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxArea;
};
```



### 🐢 R — Review｜測試驗證

#### 🧪 測試案例

```python
assert Solution().maxArea([1,8,6,2,5,4,8,3,7]) == 49
assert Solution().maxArea([1,1]) == 1
assert Solution().maxArea([4,3,2,1,4]) == 16
```

```javascript
console.log(maxArea([1,8,6,2,5,4,8,3,7])); // 49
console.log(maxArea([1,1]));              // 1
console.log(maxArea([4,3,2,1,4]));        // 16
```

---

### 🐬 E — Evaluate｜效能評估

* **時間複雜度：** `O(n)`
* **空間複雜度：** `O(1)`

#
#
#


# 🎤 Full Spoken-Style Interview Answer

### 1️⃣ Clarify the Problem

> "Sure! So we are given an array of non-negative integers where each element represents the height of a vertical line on the x-axis.
> We want to choose two lines such that they, along with the x-axis, form a container that holds the **maximum amount of water**."

> "The container's volume is determined by the **shorter** of the two lines, multiplied by the **distance** between them.
> So the goal is to find the pair of lines that maximizes this area."

> "Just to clarify, the input size could go up to 10⁵, and the values are guaranteed to be non-negative integers.
> And we can assume the array has at least two elements, right?"

---

### 2️⃣ Discuss Edge Cases

> "Let me think about a few edge cases."

* "If the array only has two elements, then there's only one possible container, so we just return that area."
* "If all heights are the same, like `[3,3,3,3]`, the widest pair will yield the maximum area."
* "If the tallest bars are close together, they may not be the best choice, because **width matters** too."

> "Also, we don't need to handle negative heights, since the problem guarantees non-negative integers."

---

### 3️⃣ Consider Brute-Force and Optimal Approach

> "The brute-force solution would be to check all possible pairs of lines — so two nested loops over the array and calculate the area for each pair."

```python
# Pseudocode
max_area = 0
for i in range(n):
    for j in range(i+1, n):
        area = min(height[i], height[j]) * (j - i)
        max_area = max(max_area, area)
```

> "But this would take O(n²) time, which is too slow for large inputs."

---

> "Now for the optimal solution: we can use a **two-pointer approach**.
> We place one pointer at the start and one at the end. At each step, we calculate the area and move the pointer pointing to the **shorter height**, because that’s the limiting factor."

> "This way, we explore all possible widest containers while gradually trying higher walls as we shrink the width."

---

### 4️⃣ Explain and Implement Optimal Code (with spoken thought process)

> "Okay, let me walk through the code as I write it."

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left = 0                          # Pointer starting from the left
        right = len(height) - 1          # Pointer starting from the right
        max_area = 0                     # Variable to track the max area

        while left < right:
            # Calculate current height and width
            h = min(height[left], height[right])
            w = right - left
            area = h * w

            # Update the maximum area found so far
            max_area = max(max_area, area)

            # Move the pointer pointing to the shorter height
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

> "So to explain:

* I'm using two pointers: `left` at index 0, `right` at the end.
* At each step, I calculate the **area** using the shorter of the two lines and multiply by the distance.
* I compare and update the `max_area`.
* Then I move the shorter side inward, since moving the taller one won’t increase the area."

---

### 5️⃣ Discuss Time and Space Complexity

> "This solution has a **time complexity of O(n)**, since both pointers move inward at most n steps total."

> "The **space complexity is O(1)** because we only use a few variables, no extra data structures."

---

### 6️⃣ Mention Follow-Up Questions

> "Some follow-up questions an interviewer might ask could include:"

* "Can you return the indices of the two lines forming the max container, not just the area?"
* "How would you solve this problem if the input was a stream of heights, and you could only keep a window of k lines at a time?"
* "How would the algorithm change if we had to return multiple pairs that all result in the same max area?"


#
#

### Google / Amazon 面試中這題的經典 follow-up 問題之一：

> **“Can you return the indices instead of the area?”**
> 「你可以回傳構成最大容器的兩個 index 嗎？」



### 🔍 要求改動

原本我們只回傳最大容器的面積（`max_area`），現在要求回傳的是：

* **構成最大容器的兩個 index**，例如 `[left_index, right_index]`

---

### ✅ 解題邏輯調整（不改核心邏輯）

* 只要在原本的 `max_area` 更新條件時，**順便記錄當時的 `left` 和 `right`** 指標即可。


#### 🐍 Python 實作（回傳 index）

```python
class Solution:
    def maxAreaIndices(self, height: List[int]) -> List[int]:
        left = 0
        right = len(height) - 1
        max_area = 0
        result = [0, 0]  # 用來記錄最大面積時的 index

        while left < right:
            h = min(height[left], height[right])
            w = right - left
            area = h * w

            if area > max_area:
                max_area = area
                result = [left, right]  # 更新 index

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return result
```



#### 🌐 JavaScript 實作（回傳 index）

```javascript
var maxAreaIndices = function(height) {
    let left = 0;
    let right = height.length - 1;
    let maxArea = 0;
    let result = [0, 0]; // To store the indices

    while (left < right) {
        const h = Math.min(height[left], height[right]);
        const w = right - left;
        const area = h * w;

        if (area > maxArea) {
            maxArea = area;
            result = [left, right]; // Update indices
        }

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return result;
};
```

---

### 🧪 測試範例

```python
Solution().maxAreaIndices([1,8,6,2,5,4,8,3,7])
# Output: [1, 8]
# 原因：height[1]=8, height[8]=7 → area = min(8,7) * (8-1) = 7*7 = 49
```

```javascript
console.log(maxAreaIndices([1,8,6,2,5,4,8,3,7]));
// Output: [1, 8]
```

---

### 📌 小提醒（口說時可補充）

> "I didn't change the core logic at all.
> I just added a result variable to store the indices when the current area is larger than the previous max.
> This way I still maintain O(n) time and O(1) space."

#
#
#



## 🎯 Real-World Applications｜實際應用場景



### 🧱 UI/UX Layout Design Optimization｜使用者介面區塊最佳化

**EN:**
In frontend development or UI layout engines, developers may need to determine the largest rectangular area between vertical elements (like divs or panels) that can fit content or ads.
A logic similar to this problem helps determine the most "spacious" pair.

**中文：**
在前端開發或 UI 佈局系統中，工程師可能需要找出兩個垂直元素（如 `div` 或面板）之間能擺放最多內容或廣告的最大空間。
這題的邏輯可以應用在 **動態計算最大有效內容區塊**。

---

### 🗺️ Image Processing: Object Boundary Analysis｜影像處理：物件邊界分析

**EN:**
In image processing or computer vision, we often detect vertical lines or objects and need to compute the space between them.
This algorithm helps in calculating the **maximum region bounded by detected vertical structures** (e.g., tree lines, walls, columns in 2D scans).

**中文：**
在影像處理與電腦視覺中，常會偵測到一系列垂直物體（如牆、柱子），需要計算它們之間形成的最大空間。
這題的邏輯就能應用來找出**最大區域或容積的邊界對**。

---

### 🏭 Logistics & Warehouse Slot Optimization｜倉儲與物流的空間最佳化

**EN:**
In warehouse management, finding two storage columns with the greatest distance and usable height helps **maximize volume** for shipping containers or robot arms.
This is useful in robotic logistics, such as Amazon Robotics or warehouse slotting algorithms.

**中文：**
在倉儲管理中，找出**最適合放大型物品的兩個立體空間邊界**（像是貨架柱）可以幫助最大化儲存體積。
這種邏輯在 **Amazon Robotics 倉儲機器人**、自動化貨架系統中有實際應用價值。

---

### 📊 Visualization & Graph Layout Optimization｜資料視覺化與圖形佈局最佳化

**EN:**
When designing graphs or dashboards, we sometimes want to find the two most distant elements with high values to visually emphasize trends or gaps.
This two-pointer strategy helps in **visual density calculation** or optimal spacing.

**中文：**
在資料視覺化或儀表板設計中，我們可能希望找出 **兩個最有代表性的高點**，來凸顯趨勢或差異。
這題的解法可應用於 **視覺密度估算** 或圖形元件最佳分佈計算。

---

### 🚧 Real-Time Sensor Data Boundary Tracking｜即時感測資料邊界偵測

**EN:**
In IoT or automotive scenarios, sensors may report height or distance at regular intervals.
To quickly detect the widest safe path (like for autonomous vehicles), this algorithm helps find **widest navigable space** bounded by two sensor readings.

**中文：**
在物聯網或自駕車情境中，感測器會定期傳回高度/距離資訊。
這類演算法可用來即時計算 **最寬的可通行區間**，幫助自駕系統選擇最佳通道。

---

### ✨ 小結｜Summary

這題雖然看起來是抽象的數學題，但實際上常常出現在：

| 應用領域           | 應用目的           |
| -------------- | -------------- |
| 前端 / UI Layout | 元件之間最大可用空間     |
| 影像處理           | 偵測最大區域的邊界物件    |
| 倉儲物流           | 貨架之間的最大可用儲存容量  |
| 自駕感測           | 偵測最寬可通行區間      |
| 資料視覺化          | 找出視覺最關鍵的兩個資料節點 |

