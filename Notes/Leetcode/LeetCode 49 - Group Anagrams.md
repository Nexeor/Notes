2025-07-23 02:35

Status: #Leaf

Tags: #LeetCode #Hashmaps

# Group Anagrams
**LeetCode**: https://leetcode.com/problems/group-anagrams/description/
**GitHub**:
## Problem
Given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

**Example 1:**
- **Input:** `strs = ["eat","tea","tan","ate","nat","bat"]`
- **Output:** `[["bat"],["nat","tan"],["ate","eat","tea"]]`
**Example 2:**
- **Input:** `strs = [""]`
- **Output:** `[[""]]`
**Example 3:**
- **Input:** `strs = ["a"]`
- **Output:** `[["a"]]`
**Constraints:**
- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters
## Solution
We can use a similar idea to [[LeetCode 242 - Valid Anagram]], but on a larger scale. We will get the char frequency of each word. Using the frequency map as a key and a list of words as the value, save each word to the hashmap, therefore grouping anagrams together.
1) Setup: Create empty hashmap
2) Iterate over `strs`
	1) Calculate the char frequency of the string
	2) Add the string to the hashmap, using the char frequency as the key
3) Return the grouped anagrams
**Analysis:**
- **TC: O(m * n)** - Iterate over each character of each string
- **SC: O(m)** - Store once for each string
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        hashmap = defaultdict(list)

        for word in strs:
            char_arr = [0] * 26
            for char in word:
                char_arr[ord(char) - ord("a")] += 1

            hashmap[tuple(char_arr)].append(word)

        return list(hashmap.values())
```
## References
