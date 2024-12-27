
# LeetCode 929: Unique Email Addresses

### Problem Statement

Given a list of email addresses, determine how many unique email addresses exist. You can achieve this by normalizing the email addresses based on the following rules:

1. Ignore dots (`.`) in the local name (everything before `@`).
2. Ignore everything after a plus (`+`) in the local name.
3. The domain name (everything after `@`) remains unchanged.

Return the count of unique email addresses.

---

##### UMPIRE Method: (U)nderstand | (M)atch | (P)lan | (I)mplement | (R)eview | (E)valuate

### 1. Understand

- **Inputs**:
  - A list of email addresses: `emails` (1 ≤ `emails.length` ≤ 100, 1 ≤ `emails[i].length` ≤ 100).
- **Outputs**:
  - An integer representing the count of unique email addresses.
- **Constraints**:
  - The input list contains valid email addresses.
  - Each email address consists of a local name and domain name separated by `@`.

- **Assumptions**:
  - The local name is case-sensitive and can include `.` or `+`.
  - The domain name is case-sensitive and remains unchanged.

---

### 2. Match

- **Problem Type**:
  - This problem involves **string manipulation** and **set-based uniqueness checks**.
- **Data Structures**:
  - Use a **set** to store unique email addresses after normalization.

---

### 3. Plan

1. Initialize an empty set `unique_emails` to store normalized email addresses.
2. For each email in the input list:
   - Split the email into `local` and `domain` using the `@` separator.
   - Normalize the `local` part:
     - Ignore everything after the first `+`.
     - Remove all dots (`.`).
   - Recombine the normalized `local` and `domain` to form the full email.
   - Add the normalized email to `unique_emails`.
3. Return the size of the set as the result.

**Edge Cases**:
- Emails with no dots or plus signs.
- Emails where the local part is empty after normalization.
- All emails are duplicates after normalization.

**Time Complexity**: O(n * m), where `n` is the number of emails and `m` is the average length of each email.  
**Space Complexity**: O(n * m), as we store up to `n` unique normalized emails in the set.

---

### 4. Implement

```python
def numUniqueEmails(emails):
    """
    Count the number of unique email addresses after normalization.
    
    Parameters:
    - emails: List[str] - A list of email addresses
    
    Returns:
    - int - Count of unique normalized email addresses
    """
    # Step 1: Initialize a set for unique emails
    unique_emails = set()

    # Step 2: Process each email
    for email in emails:
        # Split into local and domain
        local, domain = email.split('@')

        # Normalize the local part
        local = local.split('+')[0]  # Ignore everything after '+'
        local = local.replace('.', '')  # Remove dots

        # Combine normalized local with domain
        normalized_email = f"{local}@{domain}"

        # Add to the set
        unique_emails.add(normalized_email)

    # Step 3: Return the count of unique emails
    return len(unique_emails)
```

**Step-by-Step Implementation Notes**:
1. **Initialize Set**: Use a set to ensure uniqueness.
2. **Split Email**: Use `split('@')` to separate `local` and `domain`.
3. **Normalize Local**:
   - Remove everything after `+` using `split('+')[0]`.
   - Remove dots using `replace('.', '')`.
4. **Recombine and Add to Set**: Form the normalized email and store it in the set.
5. **Return Set Length**: The length of the set is the count of unique emails.

---

### 5. Review

#### Dry Run with Example Input:
```python
emails = [
    "test.email+alex@gmail.com",
    "test.e.mail+bob.cathy@gmail.com",
    "testemail+david@lee.tcode.com"
]
print(numUniqueEmails(emails))  # Expected Output: 2
```

- **Processing `test.email+alex@gmail.com`**:
  - Local: `test.email+alex` → `test.email` → `testemail`.
  - Domain: `gmail.com`.
  - Normalized: `testemail@gmail.com`.
- **Processing `test.e.mail+bob.cathy@gmail.com`**:
  - Local: `test.e.mail+bob.cathy` → `test.e.mail` → `testemail`.
  - Domain: `gmail.com`.
  - Normalized: `testemail@gmail.com`.
- **Processing `testemail+david@lee.tcode.com`**:
  - Local: `testemail+david` → `testemail`.
  - Domain: `lee.tcode.com`.
  - Normalized: `testemail@lee.tcode.com`.

Unique emails: `{"testemail@gmail.com", "testemail@lee.tcode.com"}`.  
Output: `2`.

#### Edge Case:
```python
emails = ["a@leetcode.com", "b@leetcode.com", "c@leetcode.com"]
print(numUniqueEmails(emails))  # Expected Output: 3
```

---

### 6. Evaluate

**Time Complexity**: O(n * m)  
- Parsing and normalizing each email takes O(m), repeated for `n` emails.

**Space Complexity**: O(n * m)  
- The set stores up to `n` unique emails, each of average length `m`.

---

### Additional Notes

#### Why This Problem is Important
- It demonstrates the use of string manipulation and set-based uniqueness checks, which are common in industry applications like data preprocessing and validation.

#### Prerequisites for Practicing This Problem
- Understanding of string operations (`split`, `replace`).
- Familiarity with sets in Python for uniqueness checks.

#### Industry Relevance
- Useful in real-world applications for processing and normalizing data, such as email verification, CRM systems, and data deduplication.

#### Follow-up Practice Problems
1. [LeetCode 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)
2. [LeetCode 804: Unique Morse Code Words](https://leetcode.com/problems/unique-morse-code-words/)
3. [LeetCode 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)
