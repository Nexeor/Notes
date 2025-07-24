2025-07-24 13:48

Status: #Leaf

Tags: #LeetCode #DynamicProgramming 

# LeetCode 139 - Word Break
**LeetCode**: https://leetcode.com/problems/word-break/description/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/1D_Dynamic/word_break.py
## Problem
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**
- **Input:** `s = "leetcode", wordDict = ["leet","code"]`
- **Output:** `true`
- **Explanation:** `Return true because "leetcode" can be segmented as "leet code".`
**Example 2:**
- **Input:** `s = "applepenapple", wordDict = ["apple","pen"]`
- **Output:** `true`
- **Explanation:** Return `true` because `applepenapple` can be segmented as `apple pen apple`
	- Note that you are allowed to reuse a dictionary word.
**Example 3:**
- **Input:** `s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]`
- **Output:** `false`
**Constraints:**
- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are **unique**.
## Solution
This is a classic dynamic programming problem. We start at the beginning of the string, and pick words from `word_dict` to move farther down the string, then repeat the decision. In this way we try every possible word that matches our current position. However, this involves a lot of repeated work as we may try the same combinations of words several times. We can solve this by *memoizing* our results as we go:
1) Setup: Create empty cache dict
2) Recursive local function: `build_word` that takes `cur_word`
	1) If `cur_word` is cached, return that
	2) If `cur_word` is equivalent to `s`, then we have succeeded, return `true`
	3) Iterate over each `word` in the dict:
		1) If `word + cur_word == s`, then recursively call `build_word`, passing it `cur_word + word` 
		2) If the recursive call returns `true`, cache that result and return `true`
	4) We didn't find another path forward, cache and return `false`
3) Return `build_word("")`
**Analysis:**
- **TC: O(n * m * t)** - We iterate over each character in each string in `wordDict` (**m * t**) once for each character in `s` (**n**)
- **SC: O(n)** - Linear sized cache
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        cache = {}
        def build_word(cur_word):
            if cur_word in cache:
                return cache[cur_word]
            if cur_word == s:
                return True
            
            for word in wordDict:
                if len(cur_word) + len(word) <= len(s) 
	            and word == s[len(cur_word):len(cur_word) + len(word)]:
                    if build_word(cur_word + word):
                        cache[cur_word] = True
                        return True
            
            cache[cur_word] = False
            return False
        
        return build_word("")
```
## References
