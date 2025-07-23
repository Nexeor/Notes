2024-11-20 17:18

Status:

Tags: #LeetCode #ProgrammingConcepts 

# Encode and Decode Strings
### Problem
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings. Please implement `encode` and `decode`.
#### Example 1:
```python
Input: ["neet","code","love","you"]

Output:["neet","code","love","you"]
```
#### Example 2:
```python
Input: ["we","say",":","yes"]

Output: ["we","say",":","yes"]
```
#### Constraints:
- `0 <= strs.length < 100`
- `0 <= strs[i].length < 200`
- `strs[i]` contains only UTF-8 characters.
### Solutions
#### Solution: Meta-Data Characters
The strings can contain any UTF-8 encoded characters, so we can designate an escape character, but we need an additional check in case that character appears in the string. To do this we count the length of each string, then prefix is with "len!" where len is the length of the upcoming string. 

```python 
["we","say",":","yes"] = "2!we3!say1!:3!yes"
```

Then, in the decode method, we start by splitting off the prefix from the '!' character and saving the the number of characters. We then split off that many characters from the remaining string and add it to the output array. After this, S will no longer contain the first word and its prefix.
	
```python 
["we","say",":","yes"] = "2!we3!say1!:3!yes" = "3!say1!:3!yes"
decoded_strings = ["we"]
```

```python
    def encode(self, strs: List[str]) -> str:
        encoded_string = ""
        for string in strs:
            meta = str(len(string)) + "!"
            encoded_string = encoded_string + meta + string    
        return encoded_string 

    def decode(self, s: str) -> List[str]:
        decoded_strings = []
        while len(s) > 0:
            # Find '!' and split off num as len
            # Count num characters and save as seperate strings
            break_char = s.find("!") # int: where the ! is
            num_char = int(s[0:break_char]) # int: number of characters
            decoded_str = s[break_char + 1 : break_char + num_char + 1]
            decoded_strings.append(decoded_str)
            s = s[break_char + num_char + 1:]
            print(decoded_str)
            print(s)
        return decoded_strings
```
##### Analysis
We only iterate through each string once and use a constant amount of extra space
- **TC:** O(m)
- **SC**: O(n)
## References
