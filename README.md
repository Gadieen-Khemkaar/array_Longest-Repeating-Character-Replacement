# 🔠 Longest Repeating Character Replacement (LeetCode #424)

## 🧩 Problem Statement

You are given a string `s` consisting of uppercase English letters and an integer `k`.

You can change (replace) at most `k` characters in the string so that all the characters in the resulting substring are the same.
Return the **length of the longest possible substring** that can be obtained after performing at most `k` replacements.

### Example

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace one 'B' in "AABA" to get "AAAA".
```

---

## 💡 Approach

We use a **sliding window** technique with two pointers (`l` and `r`):

1. Expand the window by moving `r` (the right pointer) one step at a time.
2. Keep track of the **frequency** of each character in the window using a `count` array.
3. Compute `maxCount`, the count of the **most frequent** character in the window.
4. If the number of characters to replace `(window size - maxCount)` becomes greater than `k`, shrink the window from the left (`l++`).
5. Track the maximum valid window length (`result`).

This ensures we always maintain a valid window where at most `k` replacements are needed.

---

## 🧠 Intuition

At any point, the number of characters to change in the current window is:

```
window_size - most_frequent_char_count
```

If this number exceeds `k`, the window is invalid — we move the left pointer `l` to shrink it.
Otherwise, the window is valid, and we update our result.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int l = 0, r = 0;
        vector<int> count(26, 0);
        int result = 0;
        int maxCount = 0;

        while (r < s.size()) {
            count[s[r] - 'A']++;
            maxCount = max(maxCount, count[s[r] - 'A']);

            while ((r - l + 1) - maxCount > k) {
                count[s[l] - 'A']--;
                l++;
            }

            result = max(result, r - l + 1);
            r++;
        }

        return result;
    }
};
```

---

## 🕒 Time Complexity

* **O(n)** — Each character is processed at most twice (once when expanding, once when shrinking the window).

## 💾 Space Complexity

* **O(1)** — Only 26 English uppercase letters, so frequency array size is constant.

---

## ✅ Example Walkthrough

`s = "AABABBA"`, `k = 1`

| Step | Window | Max Char | Window Size | Replacements Needed | Valid?     |
| ---- | ------ | -------- | ----------- | ------------------- | ---------- |
| 1    | A      | A(1)     | 1           | 0                   | ✅          |
| 2    | AA     | A(2)     | 2           | 0                   | ✅          |
| 3    | AAB    | A(2)     | 3           | 1                   | ✅          |
| 4    | AABA   | A(3)     | 4           | 1                   | ✅          |
| 5    | AABAB  | A(3)     | 5           | 2                   | ❌ (shrink) |
| 6    | ABAB   | A(2)     | 4           | 2                   | ❌ (shrink) |
| 7    | BAB    | B(2)     | 3           | 1                   | ✅          |

✅ **Result = 4**

---

## 🧩 Key Concepts

* Sliding Window
* Frequency Counting
* Two Pointers
* Greedy Window Expansion

---

**Author:** Gadieen Khemkaar
---
**Language:** C++
**Category:** Sliding Window / String Manipulation
**Difficulty:** Medium
