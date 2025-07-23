2025-07-23 02:00

Status: #Leaf

Tags: #LeetCode #Hashmaps

# LeetCode 242 - Valid Anagram
**LeetCode**: https://leetcode.com/problems/valid-anagram/description/
**GitHub**:
## Problem
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise

**Example 1:**
- **Input:** `s = "anagram", t = "nagaram"`
- **Output:** `true`
**Example 2:**
- **Input:** `s = "rat", t = "car"`
- **Output:** `false`
**Constraints:**
- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.
## Solution
Using a hashmap, we can count the number of characters in each string. Then, if both hashes are the same, they are an anagram.
1) Setup: Create two empty hashmaps
2) If the strings are not equal length, they cannot be an anagram, return `false`
3) Iterate over both strings:
	1) Add their characters to the hashmaps
4) Return `true` if the hashmaps are equal, `false` otherwise
**Analysis:**
- **TC: O(n)** - Iterate over each character once
- **SC: O(n)** - Store the char counts for both strings
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        a = {}
        b = {}
        for i in range(0, len(s)):
            a[s[i]] = 1 + a.get(s[i], 0)
            b[t[i]] = 1 + b.get(t[i], 0)
        
        return a == b
```
## References