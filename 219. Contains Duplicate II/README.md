
# 🧠 LeetCode 219 - Contains Duplicate II



## 📝 Problem Statement

Given an integer array `nums` and an integer `k`, return `true` if there are **two distinct indices** `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.



# 🧩 UMPIRE Framework (English Version)


### 🔍 U — Understand the Problem

We are given:

* An array of integers `nums`
* An integer `k`

We need to return `True` if:

* There exist **two distinct indices** `i` and `j`
* `nums[i] == nums[j]`
* The index distance is at most `k`: `abs(i - j) <= k`

---

### 🧩 M — Match the Problem Type

* Hash table problem
* Could also be solved with sliding window (Set)
* Keywords: **duplicate within distance k**

---

### 🛠️ P — Plan

#### ✅ **Approach 1: HashMap to track last seen indices**

* Create a dictionary to track the last seen index of each number
* For each number in the list:

  * If it's already in the dictionary:

    * Check if current index minus the last index is less than or equal to k
    * If so, return `True`
  * Update the dictionary with current index

#### ✅ **Approach 2: Sliding Window + Set**

* Keep a window of size `k` using a Set
* For each number:

  * If it already exists in the Set, return `True`
  * Add number to the Set
  * If Set size exceeds `k`, remove the number that falls outside the window

---

### 👨‍💻 I — Implement (Python, with detailed comments)

#### ✅ Approach 1: HashMap (Track indices)

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        last_seen = {}  # Dictionary to store the last index where each number appeared

        for i, num in enumerate(nums):
            if num in last_seen:
                # If duplicate is found within distance k
                if i - last_seen[num] <= k:
                    return True
            # Update the last seen index of the number
            last_seen[num] = i

        return False
```

---

#### ✅ Approach 2: Sliding Window + Set

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        window = set()  # Sliding window of size at most k

        for i in range(len(nums)):
            if nums[i] in window:
                return True  # Found duplicate within k distance

            window.add(nums[i])  # Add current number to window

            # Maintain window size: remove the element that is out of the k range
            if len(window) > k:
                window.remove(nums[i - k])  # Remove the oldest element

        return False
```

---

### 🔎 R — Review & Test

#### Test Cases

```python
# Case 1: True, because nums[0] == nums[3] = 1, and 3 - 0 = 3 <= k
containsNearbyDuplicate([1, 2, 3, 1], 3) → True

# Case 2: True, because nums[2] == nums[3] = 1, and 1 <= k
containsNearbyDuplicate([1, 0, 1, 1], 1) → True

# Case 3: False, duplicates exist but not within k = 2
containsNearbyDuplicate([1, 2, 3, 1, 2, 3], 2) → False
```

---

### 📈 E — Evaluate Time & Space

| Approach              | Time Complexity | Space Complexity |
| --------------------- | --------------- | ---------------- |
| HashMap (Track Index) | O(n)            | O(n)             |
| Sliding Window (Set)  | O(n)            | O(min(n, k))     |

<br>

# 📚 UMPIRE 框架（中文版本）



### 🔍 U — 了解題目

給定一個整數陣列 `nums` 和一個整數 `k`，找出是否存在兩個索引 `i` 和 `j`：

* `nums[i] == nums[j]`（值相等）
* `abs(i - j) <= k`（索引距離不超過 k）

---

### 🧩 M — 匹配題型

* 常見 Hash 題型：記錄值與其索引的對應關係
* 可選 Sliding Window 優化空間

---

### 🛠️ P — 解題計劃

#### ✅ 方法一：使用字典記錄每個值的最後出現位置

* 若值重複出現，檢查當前索引與前一次出現位置是否小於等於 `k`

#### ✅ 方法二：使用 Set + 滑動視窗

* 建立固定大小為 `k` 的 Set 視窗
* 若值已在 Set 中 → 回傳 True
* 否則加入，並移除最舊的元素

---

### 👨‍💻 I — 實作（Python + 詳細註解）

#### ✅ 方法一：HashMap 實作

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        last_seen = {}

        for i, num in enumerate(nums):
            if num in last_seen:
                # 如果目前索引與上次出現的索引差距小於等於 k
                if i - last_seen[num] <= k:
                    return True
            # 更新這個數字的最後出現位置
            last_seen[num] = i

        return False
```

---

#### ✅ 方法二：滑動視窗 + Set 實作

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        window = set()

        for i in range(len(nums)):
            if nums[i] in window:
                return True  # 發現重複元素在 k 距離內

            window.add(nums[i])

            # 若超出視窗大小，移除最舊的元素
            if len(window) > k:
                window.remove(nums[i - k])

        return False
```

---

### 🔎 R — 檢查與測試案例

```python
containsNearbyDuplicate([1, 2, 3, 1], 3)  # True
containsNearbyDuplicate([1, 0, 1, 1], 1)  # True
containsNearbyDuplicate([1, 2, 3, 1, 2, 3], 2)  # False
```

---

### 📈 E — 複雜度分析

| 解法             | 時間複雜度 | 空間複雜度 |
| -------------- | ----- | ----- |
| HashMap        | O(n)  | O(n)  |
| Sliding Window | O(n)  | O(k)  |

---

## ✨ Bonus: 附加筆記｜寫這題時要注意什麼？

* ⚠️ **Set 不記錄索引**，如果你需要知道 `i, j` 的位置，不能用 Set！
* ✅ 如果 `k` 非常大（像 10⁵），使用 HashMap 會更保險。
* 🧠 使用 Sliding Window 時記得「多餘的舊資料」要移出視窗（否則會 false positive）
* 🔍 記得考慮邊界狀況：

  * 空陣列
  * 所有值皆不同
  * 所有值皆相同但距離超過 k

| 解法名稱                       | 使用資料結構            | 時間複雜度 | 空間複雜度        | 優點           | 缺點               |
| -------------------------- | ----------------- | ----- | ------------ | ------------ | ---------------- |
| ✅ **HashMap (記錄索引)**       | `dict` / `object` | O(n)  | O(n)         | 精準追蹤索引，容易理解  | 空間使用多            |
| ✅ **Sliding Window + Set** | `set`             | O(n)  | O(min(n, k)) | 節省空間，適合小 `k` | 只能處理連續範圍，不保留原始索引 |


<br>


