
# 🎯 LeetCode 380 - Insert Delete GetRandom O(1) (English UMPIRE)

### 🌟 Understand

We need to design a data structure that supports **average O(1)** operations for:

1. `insert(val)` – Insert a value if not present. Return `true` if it was added.
2. `remove(val)` – Remove a value if present. Return `true` if it was removed.
3. `getRandom()` – Return a random element from the set. All elements must have the **same probability**.

The data structure must **behave like a set** (no duplicates), and support **fast random access**.

---

### 🧩 Match

This is a hybrid data structure problem:

* We need **fast lookup** → use a `HashMap`.
* We need **random access** → use a `List`.

Using only a `set()` or `HashMap` won't let us randomly access elements in O(1).
Using only a `list` makes removals O(n).
Thus, the combination of `list` + `map` is ideal.

---

### 🧠 Plan

We use:

* `nums` (list) to store the values.
* `val_to_index` (map) to track each value’s index in the list.

Operations:

* `insert(val)`:

  * If `val` is in map → return `False`.
  * Otherwise, append to list and store index in map → return `True`.
* `remove(val)`:

  * If `val` not in map → return `False`.
  * Swap `val` with the last element, update map, then pop the list and delete from map.
* `getRandom()`:

  * Use `random.choice(nums)` (Python) or `Math.random()` index (JavaScript).

---

### ⌨️ Implement

#### 🐍 Python

```python
import random

class RandomizedSet:

    def __init__(self):
        # List to store inserted values
        self.nums = []
        # Map to store value:index for fast lookup
        self.val_to_index = {}

    def insert(self, val: int) -> bool:
        if val in self.val_to_index:
            return False
        self.nums.append(val)  # Add to list
        self.val_to_index[val] = len(self.nums) - 1  # Store index
        return True

    def remove(self, val: int) -> bool:
        if val not in self.val_to_index:
            return False
        index = self.val_to_index[val]
        last_val = self.nums[-1]

        # Move last element to the index to be removed
        self.nums[index] = last_val
        self.val_to_index[last_val] = index

        # Remove the last element
        self.nums.pop()
        del self.val_to_index[val]
        return True

    def getRandom(self) -> int:
        return random.choice(self.nums)
```

---

#### 🌐 JavaScript

```javascript
var RandomizedSet = function() {
    this.nums = []; // List to store values
    this.valToIndex = new Map(); // Map for quick index lookup
};

RandomizedSet.prototype.insert = function(val) {
    if (this.valToIndex.has(val)) return false;
    this.nums.push(val); // Add to list
    this.valToIndex.set(val, this.nums.length - 1); // Store index
    return true;
};

RandomizedSet.prototype.remove = function(val) {
    if (!this.valToIndex.has(val)) return false;
    const index = this.valToIndex.get(val);
    const last = this.nums[this.nums.length - 1];

    // Swap with last element
    this.nums[index] = last;
    this.valToIndex.set(last, index);

    // Remove last
    this.nums.pop();
    this.valToIndex.delete(val);
    return true;
};

RandomizedSet.prototype.getRandom = function() {
    const randomIndex = Math.floor(Math.random() * this.nums.length);
    return this.nums[randomIndex];
};
```

---

### 🔍 Review

* All operations are **O(1)** on average.
* Using map avoids costly `.indexOf()` or `.remove()` operations.
* `getRandom()` is efficient because list allows random index access.

---

### 🧪 Evaluate

Edge Cases:

* Insert duplicates → should return `false`
* Remove non-existent element → should return `false`
* `getRandom()` after many inserts/removes → still evenly distributed

#
#
#

# 🐉 LeetCode 380 - 插入、刪除並隨機取得元素 (中文 UMPIRE)

### 🌟 理解問題

你需要設計一個資料結構，支援下列操作並且平均時間複雜度為 O(1)：

1. `insert(val)`：如果值尚未存在，插入集合中並回傳 `true`，否則回傳 `false`。
2. `remove(val)`：如果值存在於集合中，刪除並回傳 `true`，否則回傳 `false`。
3. `getRandom()`：隨機從集合中回傳一個元素，每個元素機率必須相同。

這個結構需類似 set（不能重複），且能夠快速隨機選取元素。

---

### 🧩 類型比對

這是一道資料結構設計題：

* 需要 O(1) 查找 → 使用 HashMap（字典）
* 需要 O(1) 隨機取得 → 使用 List（陣列）

單獨使用 set 或 list 都無法同時達成這兩個目標，必須結合兩者。

---

### 🧠 解法規劃

使用：

* `nums`（list）：儲存元素
* `val_to_index`（map）：記錄每個元素在 `nums` 中的位置

操作說明：

* `insert(val)`：

  * 若存在於 map → 回傳 `False`
  * 否則 append 到 list，並記錄 index → 回傳 `True`
* `remove(val)`：

  * 若不存在 → 回傳 `False`
  * 將要移除的元素與最後一個元素交換，更新 map，pop list，刪除 map → 回傳 `True`
* `getRandom()`：

  * 隨機選一個 list 中的 index 回傳

---

### ⌨️ 實作

#### 🐍 Python

```python
import random

class RandomizedSet:
    def __init__(self):
        self.nums = []  # 儲存值的列表
        self.val_to_index = {}  # 儲存值對應的 index

    def insert(self, val: int) -> bool:
        if val in self.val_to_index:
            return False
        self.nums.append(val)  # 加入列表尾端
        self.val_to_index[val] = len(self.nums) - 1  # 儲存對應索引
        return True

    def remove(self, val: int) -> bool:
        if val not in self.val_to_index:
            return False
        index = self.val_to_index[val]
        last_val = self.nums[-1]

        self.nums[index] = last_val  # 把最後一個元素搬過來
        self.val_to_index[last_val] = index  # 更新 map 中對應索引

        self.nums.pop()  # 移除最後一個元素
        del self.val_to_index[val]  # 刪除原來的 val
        return True

    def getRandom(self) -> int:
        return random.choice(self.nums)  # 從 list 中隨機選一個
```

---

#### 🌐 JavaScript

```javascript
var RandomizedSet = function() {
    this.nums = []; // 存儲元素的陣列
    this.valToIndex = new Map(); // 對應值的 index
};

RandomizedSet.prototype.insert = function(val) {
    if (this.valToIndex.has(val)) return false;
    this.nums.push(val); // 加入 list
    this.valToIndex.set(val, this.nums.length - 1); // 記錄 index
    return true;
};

RandomizedSet.prototype.remove = function(val) {
    if (!this.valToIndex.has(val)) return false;
    const index = this.valToIndex.get(val);
    const last = this.nums[this.nums.length - 1];

    this.nums[index] = last; // 將最後一個元素搬到要刪的位置
    this.valToIndex.set(last, index); // 更新索引

    this.nums.pop(); // 刪除最後一個
    this.valToIndex.delete(val); // 從 map 移除 val
    return true;
};

RandomizedSet.prototype.getRandom = function() {
    const randomIndex = Math.floor(Math.random() * this.nums.length);
    return this.nums[randomIndex]; // 回傳隨機元素
};
```

---

### 🔍 複習與優化

* 所有操作都可在 O(1) 完成
* 移除時的「交換再 pop」技巧是關鍵
* `getRandom()` 利用 list 的 index 隨機存取效率極高

---

### 🧪 邊界測試

* 插入重複值 → 應該回傳 `false`
* 移除不存在值 → 應該回傳 `false`
* `getRandom()` 在大量操作後仍需平均分布

#
#
#
