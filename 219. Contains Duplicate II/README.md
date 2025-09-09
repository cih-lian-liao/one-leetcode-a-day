
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

# 🎤 Full Spoken-Style Interview Answer

**(Interviewer gives you the problem: "Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.")**

### 1\. Clarify the Problem and Read Examples

**(You):** "Awesome, thank you for the problem. Let me make sure I understand it correctly. I'm given an array of numbers, let's call it `nums`, and an integer `k`. My goal is to find out if there are any two identical numbers in the array whose indices are at most `k` distance apart. If such a pair exists, I should return `true`; otherwise, I return `false`.

Let me walk through one of the examples to solidify my understanding.

  * If `nums` is `[1, 2, 3, 1]` and `k` is `3`.
  * I see the number `1` appears twice. The first `1` is at index `0`, and the second `1` is at index `3`.
  * The absolute difference between their indices is `abs(3 - 0)`, which is `3`.
  * The condition is `abs(i - j) <= k`, so here, `3 <= 3`. This is true.
  * So, for this case, I should return `true`.

And for the second example, `nums = [1, 2, 3, 1, 2, 3]` and `k = 2`.

  * Again, let's look at the number `1`. It's at index `0` and index `3`.
  * The difference in indices is `abs(3 - 0) = 3`.
  * In this case, `3` is *not* less than or equal to `k=2`. So this pair doesn't satisfy the condition.
  * Let's check the number `2`. It's at index `1` and index `4`. The difference is `abs(4 - 1) = 3`, which is also greater than `k`.
  * The same goes for `3`. The difference is `abs(5 - 2) = 3`, also greater than `k`.
  * Since no pair of identical numbers meets the distance requirement, the output should be `false`.

Okay, that seems clear. The constraints on `nums.length` and `k` will also be important for figuring out the efficiency I need to aim for."

### 2\. Discuss Edge Cases

**(You):** "Before I jump into solutions, I'd like to consider a few edge cases.

  * **Empty `nums` array:** If the input array `nums` is empty, there can't be any duplicates, so I should return `false`. The same would apply if the array has only one element.
  * **`k` is zero:** If `k` is `0`, the condition becomes `abs(i - j) <= 0`. Since `i` and `j` must be distinct indices, `abs(i - j)` will always be greater than 0. So, no pair can ever satisfy the condition. In this case, I should always return `false`.
  * **`k` is negative:** The problem description doesn't specify, but `abs(i - j)` is always non-negative. If `k` were negative, the condition could never be met, so I'd return `false`. I'll assume `k` is non-negative based on the problem type.
  * **No duplicates at all:** If the array contains all unique elements, like `[1, 2, 3, 4]`, then no `nums[i]` will ever equal `nums[j]`, so it should return `false`.

Thinking about these helps me ensure my final code is robust."

### 3\. Consider Brute-Force and Optimal Approach

**(You):** "Okay, so my first thought for a solution—the brute-force approach—would be to just check every possible pair of elements in the array.

I could use a nested loop. The outer loop would go from `i = 0` to the end of the array. The inner loop would go from `j = i + 1` to the end. Inside the inner loop, I'd check two conditions:

1.  Is `nums[i]` equal to `nums[j]`?
2.  Is `abs(i - j)` less than or equal to `k`?

If both are true for any pair, I can immediately return `true`. If I finish both loops without finding such a pair, I'd return `false`.

This would work, but the time complexity would be $O(n^2)$ because of the nested loops. Given that `n` can be up to $10^5$, an $O(n^2)$ solution would likely be too slow and time out. So, I need a more optimal approach.

To optimize, I need to avoid re-checking things. The bottleneck is finding the duplicate and its index efficiently. This makes me think of using a **hash map** (or a dictionary in Python). A hash map provides average $O(1)$ time complexity for lookups and insertions.

My idea is this: I can iterate through the array once, from left to right. I'll use a hash map to store the numbers I've already seen, where the **key is the number** itself, and the **value is its most recent index**.

As I iterate with index `i` and number `num`:

1.  I'll check if `num` is already in my hash map.
2.  If it is, that means I've found a duplicate\! I can then get its last seen index, let's call it `last_index`, from the map.
3.  Then, I just need to check if the distance `i - last_index` is less than or equal to `k`. Since I'm iterating from left to right, `i` will always be greater than `last_index`, so I don't need the absolute value function.
4.  If the distance is within `k`, I've found my pair, and I can return `true` immediately.
5.  If `num` is not in the map, or if the distance was too large, I then need to update the map with the current number and its index: `map[num] = i`. This ensures I always have the *most recent* index for any duplicate I might find later.

This approach seems much better. It processes each element once, and each hash map operation is on average $O(1)$. This brings the total time complexity down to $O(n)$. The space complexity would be $O(n)$ in the worst case, where all elements are unique and I have to store every single one in the hash map."

### 4\. Explain and Implement Optimal Code

**(You):** "This hash map approach sounds solid. I'll go ahead and implement that. I'll write it in Python.

Okay, I'm starting to write the code now."

```python
# I'll define the class and method as required by the problem.
class Solution:
    def containsNearbyDuplicate(self, nums: list[int], k: int) -> bool:
        # First, I'll create my hash map. I'll call it 'last_seen_index'
        # to make it clear what it's storing: the last index we saw a number at.
        last_seen_index = {}  # Key: number, Value: index

        # Now, I'll iterate through the 'nums' array. I need both the index and
        # the value, so I'll use enumerate(), which is a clean way to do that in Python.
        for i, num in enumerate(nums):
            
            # For each number, the first thing I do is check if we've seen it before.
            # I can do this by checking if 'num' is a key in our hash map.
            if num in last_seen_index:
                
                # If it is, we've found a duplicate. Now we must check the second condition:
                # is the distance between the indices less than or equal to k?
                # The previous index is stored in last_seen_index[num].
                # The current index is 'i'.
                if i - last_seen_index[num] <= k:
                    # If the condition is met, we're done! We can just return True.
                    return True
            
            # If the number wasn't in the map, OR if the distance check failed,
            # we need to update the map with the current index. This is crucial because
            # we always want to store the most recent index to get the smallest possible
            # distance for future checks.
            last_seen_index[num] = i
            
        # If I get through the entire loop without returning True, it means no
        # such pair was found. So, at the end, I can safely return False.
        return False

```

**(You):** "I think this code correctly implements the logic we discussed. It handles the core requirements efficiently."

### 5\. Discuss Time/Space Complexity

**(You):** "Now, let's quickly analyze the time and space complexity of this implementation.

  * **Time Complexity:** I'm iterating through the `nums` array exactly once. The loop runs `n` times, where `n` is the number of elements in `nums`. Inside the loop, all operations—the hash map lookup (`in`), retrieval (`[]`), and update (`=`)—are, on average, $O(1)$. Therefore, the total time complexity is **$O(n)$**.

  * **Space Complexity:** The hash map `last_seen_index` stores entries for the numbers encountered. In the worst-case scenario, if all the numbers in the `nums` array are distinct, the hash map will grow to the size of the input array. Therefore, the space complexity is **$O(n)$**.

This is a significant improvement over the $O(n^2)$ brute-force solution, especially for large inputs."

### 6\. Mention Follow-up Questions

**(You):** "This was a great problem. If we wanted to explore it further, a couple of follow-up questions come to mind.

  * **Space Optimization:** I could be asked about optimizing space. The space complexity is $O(n)$, but what if `k` is much smaller than `n`? We could use a **sliding window approach with a set**. We would maintain a set of the last `k` elements. As we iterate, we'd try to add the current element to the set. If it's already there, we return `true`. If not, we add it. To maintain the window size, if the set's size exceeds `k`, we remove the element that just fell out of the window (`nums[i-k]`). This would make the space complexity $O(k)$ (or more precisely, $O(\\min(n, k))$), which is better if `k` is small.

  * **What if we need to return the indices?** The problem could be modified to return the actual indices `i` and `j` of the first pair that satisfies the condition. My current code could be easily adapted for this by just returning `(last_seen_index[num], i)` instead of `True`.

  * **What if the input is a stream?** If the numbers were coming in as a stream, we wouldn't know the full length `n` in advance. My hash map solution would still work perfectly fine in this scenario, as it processes one element at a time without needing to know what's coming next. The sliding window approach would also work well.

These are just a few ways the problem could be extended. Overall, I feel confident in the hash map solution I've provided."

<br>


Of course! Understanding the real-world applications of an algorithm makes learning it much more meaningful. Here are some examples of how the core logic of "Contains Duplicate II" is used in the tech industry, presented in a bilingual note format.

---

# 🎯 Real-World Applications｜實際應用場景

The core idea of this problem—finding duplicates within a specific window (`k`)—is fundamental in systems that process sequential data and need to detect recent or localized patterns.

### ### 1. Fraud and Anomaly Detection (金融風控與異常檢測)

* **English:** In financial systems, this logic can be used to detect fraudulent activities. For example, if the same credit card number (`nums[i] == nums[j]`) is used for multiple transactions within a very short sequence of global transactions (`abs(i - j) <= k`), it could be a flag for fraud. The "window" `k` represents a high-risk transaction period.
* **中文:** 在金融系統中，這個邏輯可以用於偵測詐欺行為。例如，如果同一張信用卡號碼 (`nums[i] == nums[j]`) 在全球交易序列中，於極短的範圍內 (`abs(i - j) <= k`) 出現多次，系統就可能將其標記為潛在的盜刷行為。這裡的 `k` 代表一個高風險的交易時間窗口。

### ### 2. Spam and Bot Detection (垃圾訊息與機器人檢測)

* **English:** Social media or comment systems can use this to identify spam. If a user posts the exact same comment (`nums[i]`) multiple times within their last `k` posts or comments (`abs(i - j) <= k`), their behavior can be flagged as bot-like or spammy. This helps maintain platform quality.
* **中文:** 社群媒體或留言系統可以利用此算法來識別垃圾訊息。如果一個用戶在最近的 `k` 則貼文或留言中 (`abs(i - j) <= k`)，重複發布了完全相同的內容 (`nums[i]`)，系統可以將其行為標記為機器人或垃圾訊息，以維護平台內容品質。

### ### 3. Network Security & Monitoring (網路安全與監控)

* **English:** In network monitoring, systems analyze streams of data packets. If a duplicate data packet (identified by a sequence number, `nums[i]`) arrives within a certain time window or packet count (`k`) of a previous identical packet, it could indicate a network loop, a misconfiguration, or a replay attack.
* **中文:** 在網路監控中，系統會分析連續的數據封包流。如果一個重複的數據封包（由其序列號 `nums[i]` 標識）在前一個相同封包之後的 `k` 個封包（或時間窗口）內再次到達，這可能意味著網路迴圈、設備配置錯誤或是正在遭受「重放攻擊」。

### ### 4. Data Deduplication in Streaming (串流資料去重)

* **English:** When processing real-time data streams (e.g., IoT sensor data, user activity logs), you might want to ignore duplicate events that occur in quick succession. This algorithm allows a system to efficiently check if an incoming event (`nums[i]`) is a repeat of one of the last `k` events processed, preventing redundant processing or storage.
* **中文:** 在處理即時數據流（例如：物聯網感測器數據、用戶行為日誌）時，我們常常需要忽略短時間內連續發生的重複事件。這個算法讓系統可以高效地檢查新傳入的事件 (`nums[i]`) 是否與最近處理的 `k` 個事件之一重複，從而避免多餘的運算或儲存。

### ### 5. Plagiarism Detection (抄襲檢測)

* **English:** In a simplified text analysis model, you could check for plagiarism or unoriginal writing by seeing if the same unique phrase (represented as a hash or number, `nums[i]`) appears multiple times within a certain number of words (`k`). Frequent nearby duplicates might indicate poorly written or copied content.
* **中文:** 在一個簡化的文本分析模型中，可以透過檢查一個獨特的片語（表示為一個雜湊值或數字 `nums[i]`）是否在文章的某個 `k` 字詞範圍內多次出現，來判斷內容的原創性。如果短距離內有大量重複的片語，可能代表內容是抄襲或寫作品質不佳。
