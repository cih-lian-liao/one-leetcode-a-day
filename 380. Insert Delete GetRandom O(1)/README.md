
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

# 🎤 Full Spoken-Style Interview Answer

### 💬 1. Clarify the problem

> “Let me make sure I understand the problem correctly.
> We need to implement a data structure that behaves like a set — meaning it does not allow duplicates — and it should support three operations: `insert`, `remove`, and `getRandom`.
> All of these operations should run in average O(1) time.
> The `insert` operation adds an element if it’s not already present.
> The `remove` deletes the element if it exists.
> And `getRandom` returns a random element from the current set, and all elements must have the same probability of being returned.”

> “Also, it’s important to note that we’re not given the size of the input in advance, so we should assume it can be large — which means we really do need the constant-time guarantees.”

---

### 🔍 2. Discuss edge cases

> “There are a few edge cases we should consider:

* Trying to insert a value that already exists — in that case we should return `false`.
* Removing a value that isn’t in the set — also return `false`.
* Calling `getRandom()` on an empty set might not be valid per the problem’s assumption, so we probably don’t need to handle that, but we should still be aware of it during implementation.”

---

### 🛠️ 3. Consider brute-force and optimal approach

> “If we used just a list, we could easily support `insert` and `getRandom`, but removing an element from the middle of a list is O(n), since we need to shift elements.
> On the other hand, if we used a set or a hashmap, we could insert and delete in O(1), but we can’t index into a set or map directly, which makes `getRandom()` inefficient.”

> “So, we need a combination of both:

* A list to store the values so we can randomly access by index.
* A hashmap to map values to their index in the list so we can look up and remove in O(1).”

---

### 💻 4. Explain and implement optimal code (while talking through it)

> “Now I’ll walk through the implementation in Python. It’s also similar in JavaScript.”

```python
import random

class RandomizedSet:

    def __init__(self):
        # nums is a list of values
        self.nums = []
        # val_to_index maps value to its index in nums
        self.val_to_index = {}

    def insert(self, val: int) -> bool:
        # If already exists, return False
        if val in self.val_to_index:
            return False
        # Add to the end of the list
        self.nums.append(val)
        # Store its index in the map
        self.val_to_index[val] = len(self.nums) - 1
        return True

    def remove(self, val: int) -> bool:
        if val not in self.val_to_index:
            return False
        # Get the index of the element to remove
        index = self.val_to_index[val]
        # Get the last element
        last_val = self.nums[-1]
        # Move the last element to the spot of the element to remove
        self.nums[index] = last_val
        self.val_to_index[last_val] = index
        # Remove last element from list
        self.nums.pop()
        # Remove value from map
        del self.val_to_index[val]
        return True

    def getRandom(self) -> int:
        return random.choice(self.nums)
```

> “So during `remove`, we’re doing this trick of swapping the element we want to remove with the last element. That allows us to avoid the O(n) cost of removing from the middle of a list, since popping from the end is O(1).”

---

### ⏱️ 5. Discuss time/space complexity

> “Let’s break down the complexity for each operation:

* `insert` is O(1) because we’re adding to the end of a list and updating a hashmap.
* `remove` is O(1) because we find the index via the map, do a single swap, and then pop the last item.
* `getRandom` is O(1) because we access a random index in the list.

All operations use average constant time.

In terms of space complexity, it’s O(n), where n is the number of elements stored, since we’re keeping both a list and a hashmap.”

---

### 🧠 6. Mention follow-up questions

> “Some follow-up questions I’d consider:

* How would we handle duplicates? For that, we’d need to modify the structure to a multiset — this is actually LeetCode 381.
* How would we modify this to support `getRandomWeighted()` — where each value has a different probability weight?
* Could we extend this to support concurrent operations in a thread-safe way?
* Or, what if we had to frequently return the `min`, `max`, or median in O(1)? That might require additional data structures.”

#
#
#


# 🎯 LeetCode 380 - Insert Delete GetRandom O(1) (Spoken Interview Notes)


### 💬 1. Clarify the Problem｜釐清問題

#### 🧑‍💻 English

Let me make sure I understand the problem correctly.
We need to implement a data structure that behaves like a set — meaning it does not allow duplicates — and it should support three operations: `insert`, `remove`, and `getRandom`.
All of these operations should run in **average O(1)** time.
The `insert` operation adds an element if it’s not already present.
The `remove` deletes the element if it exists.
And `getRandom` returns a random element from the current set, and all elements must have the same probability of being returned.
We also assume the input set can grow large, so constant time complexity really matters here.

#### 🈶 中文

我想先確認我對題目的理解是否正確。
我們要設計一個像是 set 的資料結構（不允許重複元素），而且支援三個操作：`insert`、`remove`、`getRandom`。
這三個操作的平均時間複雜度都必須是 O(1)。
`insert`：如果元素尚未存在，則插入；否則忽略。
`remove`：如果元素存在則移除。
`getRandom`：從目前的元素集合中隨機回傳一個元素，每個元素被選到的機率要一樣。
因為集合可能很大，所以 O(1) 的效率非常重要。

---

### 🔍 2. Discuss Edge Cases｜討論邊界情況

#### 🧪 English

Some edge cases to consider:

* Inserting an element that already exists should return `false`.
* Removing an element not in the set should also return `false`.
* `getRandom()` should only be called when the set has at least one element — we’ll assume that’s guaranteed by the problem.

#### 🧫 中文

我們要注意以下邊界情況：

* 如果插入一個已經存在的元素，應該回傳 `false`。
* 如果移除一個不存在的元素，也應該回傳 `false`。
* `getRandom()` 應該只在集合非空時才會被呼叫 — 題目預設應該會保證這一點。

---

### 🛠️ 3. Brute-force and Optimal Approach｜暴力與最佳解法思路

#### 🧱 English

A brute-force way might use a list.
Inserting and `getRandom` are easy with a list, but removing a value is O(n) because we’d have to search for it and then shift elements.
Using a set or map would help with fast lookup and removal, but we can’t randomly access an element in O(1).

So, to achieve average O(1) for all operations, we need to combine:

* A list to allow O(1) random access by index.
* A hashmap to map values to their index in the list for O(1) insertion and removal.

#### 🧮 中文

一種暴力的方式是只用 list。
用 list 插入值與隨機選取都很簡單，但如果要刪除一個特定值，我們需要搜尋與移動元素，會變成 O(n)。
如果只用 set 或 map 雖然可以快速查找或刪除元素，但無法在 O(1) 時間內隨機選取元素。

所以為了讓三種操作平均時間都為 O(1)，我們需要結合：

* 一個 list，提供 O(1) 的隨機索引訪問。
* 一個 map，將每個值對應到 list 中的索引，以支援 O(1) 查找與刪除。

---

### 💻 4. Explain and Implement Optimal Code｜講解與實作最佳解法

#### 🐍 Python Version

```python
import random

class RandomizedSet:

    def __init__(self):
        self.nums = []  # List of values
        self.val_to_index = {}  # Map from value to its index in nums

    def insert(self, val: int) -> bool:
        if val in self.val_to_index:
            return False
        self.nums.append(val)  # Add to end of list
        self.val_to_index[val] = len(self.nums) - 1  # Store index
        return True

    def remove(self, val: int) -> bool:
        if val not in self.val_to_index:
            return False
        index = self.val_to_index[val]
        last_val = self.nums[-1]

        # Move last element to the index of the element to remove
        self.nums[index] = last_val
        self.val_to_index[last_val] = index

        # Remove last
        self.nums.pop()
        del self.val_to_index[val]
        return True

    def getRandom(self) -> int:
        return random.choice(self.nums)  # Return a random element
```

---

#### 🌐 JavaScript Version

```javascript
var RandomizedSet = function() {
    this.nums = []; // List of values
    this.valToIndex = new Map(); // Map value → index
};

RandomizedSet.prototype.insert = function(val) {
    if (this.valToIndex.has(val)) return false;
    this.nums.push(val);
    this.valToIndex.set(val, this.nums.length - 1);
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

### ⏱️ 5. Time and Space Complexity｜時間與空間複雜度

#### ⌛ English

* `insert`: O(1) — Appending to the list and updating the map are both constant time.
* `remove`: O(1) — We use a trick to swap and pop to avoid shifting.
* `getRandom`: O(1) — Random index access in a list is constant time.
* Space complexity is O(n) because we store both the list and the map.

#### 🧾 中文

* `insert`：O(1) — 加到 list 尾端與更新 map 都是常數時間。
* `remove`：O(1) — 透過交換與 pop 技巧，避免了移動元素。
* `getRandom`：O(1) — list 的隨機索引存取是 O(1)。
* 空間複雜度為 O(n)，因為我們同時儲存了 list 和 map。

---

### 🧠 6. Follow-up Questions｜延伸思考

#### ❓ English

Some potential follow-up questions might be:

* What if we want to allow duplicate values? (That’s LeetCode 381)
* What if we want to do `getRandomWeighted()` with different probabilities?
* How would we make this data structure thread-safe for concurrent access?
* Can we extend this to also support `getMin()` or `getMax()` in O(1)?

#### 💭 中文

延伸問題可能包括：

* 如果我們想支援重複元素該怎麼辦？（這是 LeetCode 381）
* 如果我們想要加權隨機選取（`getRandomWeighted()`）呢？
* 如果這個結構要在多執行緒下安全運作要怎麼設計？
* 如果想加上 `getMin()` 或 `getMax()`，也要 O(1) 怎麼辦？

#
#
#
