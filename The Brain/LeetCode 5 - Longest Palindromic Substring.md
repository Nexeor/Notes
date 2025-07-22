2025-07-16 14:45

Status: #Leaf

Tags: #LeetCode #Algorithms #DynamicProgramming 

# LeetCode 5 - Longest Palindromic Substring
**LeetCode:** https://leetcode.com/problems/longest-palindromic-substring/description/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/longest_palindromic_substring.py
## Problem
Given a string `s`, return the longest substring of `s` that is a _palindrome_.
- A **palindrome** is a string that reads the same forward and backward.
- If there are multiple palindromic substrings that have the same length, return any one of them.

**Example 1:**
- **Input**: `s = "ababd"`
- **Output:** `"bab"`
- **Explanation:** Both "aba" and "bab" are valid answers
**Example 2:**
- **Input:** `s = "abbc"`
- **Output:** `bb`
**Constraints:**
- `1 <= nums.length <= 1000`
- `s` contains only digits and English letters
## Solution
As we iterate over the array, we treat each element as the "center", and then attempt to expand from there. We always test for odd-length palindromes (as the center element can be unique) and test for even-length palindromes when the next elem is the same, therefore forming a possible even-length palindrome. After "expanding" each palindrome, if it has a greater length than the last max, record it as the new max. 
1) Setup: Create empty `max_palindrome` string
2) Iterate over the array
	1) If `s[i] = s[t] + 1` test for an even length array
	2) Test for odd length array
	3) If odd or even is greater than previous max, save the expanded palindrome as the new `max_palindrome`
3) Return the `max_palindrome`

**Analysis:**
- **TC: O(n^2)** - We iterate over each elem, and (at the worst case), will look at each elem other elem once while expanding the array
- **SC: O(1)** - Constant space `max_palindrome`
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        max_palindrome = ""
        for i, char in enumerate(s):
	        # Test for even-length palindrome
            if i < len(s) - 1 and s[i + 1] == char:
                even = self.find_radius(s, i, i + 1)
                if len(even) > len(max_palindrome):
	                max_palindrome = even 
            # Test for odd-length palindrome
            odd = self.find_radius(s, i, i)
            if len(odd) > len(max_palindrome):
	            max_palindrome = odd
        return max_palindrome

    def find_radius(self, s, center_lb, center_rb):
        radius = 0
        while (
		    center_lb - (radius + 1) >= 0 and 
		    center_rb + (radius + 1) < len(s) and 
		    s[center_lb - (radius + 1)] == s[center_rb + (radius + 1)]
		): 
            radius += 1
            
        return s[center_lb - radius:center_rb + radius + 1]
```
## References
