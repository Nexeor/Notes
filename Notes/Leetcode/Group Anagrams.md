2024-11-19 17:25

Status:

Tags:

# Group Anagrams
### Problem
Given an array of strings strs, group the anagrams together. You can return the answer in any order.

Example 1:
- Input: strs = ["eat","tea","tan","ate","nat","bat"]
- Output: ["bat"],["nat","tan"],["ate","eat","tea"]
- Explanation:
	- There is no string in strs that can be rearranged to form "bat".
	- The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
	- The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.
Example 2:
- Input: strs = [""]
- Output: [[""]]
Example 3:
- Input: strs = ["a"]
- Output: [["a"]]
Constraints:
- 1 <= strs.length <= 104
- 0 <= strs[i].length <= 100
- strs[i] consists of lowercase English letters.
### Solution
#### Solution 1: Sorting with Hash Map
While we iterate over strs we sort each string and use it as a key in our dict. All anagrams will then be sorted into this dict accordingly, with their sorted version as the key and each anagram in the list. 
```python
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
	# Create empty hash for tracking anagrams { dict : list, dict : list, ...}
	grouped_anagrams = {}
	# Iterate over the list
	for string in strs:
		# Sort element
		sort_str = str(sorted(string))
		
		# Check hash for sorted string, but add normal string
		if sort_str in grouped_anagrams:
			grouped_anagrams[sort_str].append(string)
		else:
			grouped_anagrams[sort_str] = [string]
		
	return list(grouped_anagrams.values())
```
**Analysis**: We only iterate over the `strs` list once, but we have the additional TC of sorting each string. `m` is the number of strings and `n` is the length of the longest string
- **TC:** O(m * nlogn) 
- **SC**: O(m * n)