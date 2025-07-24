2025-07-23 14:42

Status: #Leaf

Tags: #LeetCode #Arrays 

# LeetCode 271- Encode and Decode Strings
**LeetCode**: Premium
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/arrays_and_hashing/encode_and_decode_strings.py
## Problem
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

Please implement `encode` and `decode`

**Example 1:**
- **Input:** `["neet","code","love","you"]`
- **Output:** `["neet","code","love","you"]`
**Example 2:**
- **Input:** `["we","say",":","yes"]`
- **Output:** `["we","say",":","yes"]`
**Constraints:**
- `0 <= strs.length < 100`
- `0 <= strs[i].length < 200`
- `strs[i]` contains only UTF-8 characters.
## Solution
The intuitive solution would be to use some sort of delimiter to mark where one string ends and another begins. However, if `strs` contains our delimiter (as `strs[i]` may be any UTF-8 char), then our solution will fail. We can overcome this by including the number of characters in the next string as part of our delimiter, and use it to skip to the next word when decoding.
1) Encode:
	1) Iterate over strs
		1) Add each to single string prefixed with `len(string) + #`
	2) Return encoded string
2) Decode:
	1) Iterate over the characters in the string
		1) Look for delimiter `#` and save the number of characters 
		2) Append `s[i + 1 : i + num_char + 1]` as a word
		3) Move `i` up such that `i = i + num_char`, to skip to the next word's `num_char`
**Analysis:**
- **TC: O(m)** - Look at each character once
- **SC: O(m + n)** - Store each char of each string
- Where **m** is the *total number of chars* and **n** is the *total number of strings*
```python
class Solution:
    def encode(self, strs: List[str]) -> str:
        code = ""
        for string in strs:
            code += str(len(string)) + "#" + string
        print(code)
        return code

    def decode(self, s: str) -> List[str]:
        words = []
        i = 0
        while i < len(s):
            num_char = ""
            while s[i] != "#":
                num_char += s[i]
                i += 1
            i += 1
            num_char = int(num_char)
            words.append(s[i:i+num_char])
            i = i + num_char

        return words
```
## References
