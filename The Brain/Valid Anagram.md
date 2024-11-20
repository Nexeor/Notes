2024-11-19 16:25

Status:

Tags:

# Valid Anagram
### Problem
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

Example 1:
- Input: s = "anagram", t = "nagaram"
- Output: true
Example 2:
- Input: s = "rat", t = "car"
- Output: false
Constraints:
- 1 <= s.length, t.length <= 5 * 104
- s and t consist of lowercase English letters.
### Solution
#### Solution 1: Hashing
We can hash both strings with the chars as keys and num chars as the value. If we then compare these two hashes they should be equivalent. 
```python
 def isAnagram(self, s, t):
        if len(s) != len(t):
            return False

        s_dict, t_dict = {}, {}
        for pos in range(0, len(s)):
            s_dict[s[pos]] = s_dict.get(s[pos], 0) + 1
            t_dict[t[pos]] = t_dict.get(t[pos], 0) + 1

        return s_dict == t_dict
        
```
**Analysis**: Fastest TC but requires extra SC for hashes
- **TC:** O(s + t) 
- **SC**: O(s + t)
#### Solution 2: Sorting
If we want to solve without additional space (no hashes), we can sort the strings in place and compare them.
```python
def isAnagram(self, s, t):
	if len(s) != len(t):
		return False

	return sorted(s) == sorted(t)
```
**Analysis**: Sorting in place means no extra SC but comes at the need to iterate through the strings multiple times with sorted()
- **TC:** O(slogs + tlogt)
- **SC**: O(1)
## References
