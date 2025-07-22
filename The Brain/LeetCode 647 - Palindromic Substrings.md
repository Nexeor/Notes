2025-07-16 14:45

Status: #Leaf

Tags: #LeetCode #Algorithms #DynamicProgramming 

# LeetCode 647 - Palindromic Substrings
**LeetCode:** https://leetcode.com/problems/palindromic-substrings/description/
**Github**:https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/palindromic_substrings.py
## Problem
Given a string `s`, return _the number of **palindromic substrings** in it_.
- A string is a **palindrome** when it reads the same backward as forward.
- A **substring** is a contiguous sequence of characters within the string.

**Example 1:**
- **Input**: `s = "abc"`
- **Output:** `3`
- **Explanation:** "a", "b", and "c" all form their own substrings
**Example 2:**
- **Input:** `s = "aaa"`
- **Output:** 6
- **Explanation:** Substrings with the same chars are still distinct `aaa = [a, a, a, aa, aa, aaa]`
**Constraints:**
- `1 <= nums.length <= 1000`
- `s` contains only digits and English letters
## Solution
This is a minor variation on the [[LeetCode 5 - Longest Palindromic Substring]] problem. However, rather than tracking the longest substring we encounter, we just count the number of substrings we find.
1) Setup: Create `num_palindromes = 0`
2) Iterate over the array
	1) If `s[i] = s[t] + 1` test for an even length palindromes. Increase `num_palindromes`.
	2) Test for odd length palindromes. Increase `num_palindromes`.
3) Return `num_palindromes`

**Analysis:**
- **TC: O(n^2)** - We iterate over each elem, and (at the worst case), will look at each other elem once while expanding the palindrome
- **SC: O(1)** - Constant space `num_palindromes`
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        num_palindromes = 0
        for i, char in enumerate(s):
           	# Test for even-length palindromes
            if i < len(s) - 1 and s[i + 1] == char:
                num_palindromes += self.find_radius(s, i, i + 1)
            # Test for odd-length palindromes
            num_palindromes += self.find_radius(s, i, i)
        return num_palindromes

    def find_radius(self, s, center_lb, center_rb):
        radius = 0
        while (
		    center_lb - (radius + 1) >= 0 and 
		    center_rb + (radius + 1) < len(s) and 
		    s[center_lb - (radius + 1)] == s[center_rb + (radius + 1)]
		): 
            radius += 1
            
        return radius + 1
```
## References
