2025-07-21

Status: #Leaf

Tags: #LeetCode #Algorithms #DynamicProgramming 

# LeetCode 647 - Palindromic Substrings
**LeetCode:** https://leetcode.com/problems/decode-ways/description/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/decode_ways.py
## Problem
You have intercepted a secret message encoded as a string of numbers. The message is **decoded**via the following mapping:

`"1" -> 'A'   "2" -> 'B'   ...   "25" -> 'Y'   "26" -> 'Z'`

However, while decoding the message, you realize that there are many different ways you can decode the message because some codes are contained in other codes (`"2"` and `"5"` vs `"25"`).

For example, `"11106"` can be decoded into:
- `"AAJF"` with the grouping `(1, 1, 10, 6)`
- `"KJF"` with the grouping `(11, 10, 6)`
- The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).
Note: there may be strings that are impossible to decode.  
  
Given a string s containing only digits, return the **number of ways** to **decode** it. If the entire string cannot be decoded in any valid way, return `0`.

The test cases are generated so that the answer fits in a **32-bit** integer.

**Example 1:**
- **Input**: `s = "12"`
- **Output:** `2`
- **Explanation:** "12" could be decoded as (1 2) or (12)
**Example 2:**
- **Input:** `s = "226"`
- **Output:** 3
- **Explanation:** "226" could be decoded as (2 26), (22 6), or (2 2 6)
**Example 3:**
- **Input:** `s = "06"`
- **Output:** 0
- **Explanation:** Cannot decode a leading 0 
**Constraints:**
-  `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).
## Solution
This problem is trickier than it first looks. The **Top Down** approach is more intuitive: We recursively call the subproblems, cache their results when they finish, and used the cache results to resolve the recursive stack more efficiently, solving each subproblem once. 
1) Setup: Create `cache = {len(s) : 1}`, as once we reach the end of `s`  return base case `1`
2) Write a recursive function, `calcWays(i : int)` where `i` is the index of `s`
	1) Base Case: if `i` in cache, return `cache[i]` (the stored result for the number of codes at that index)
	2) Base Case: if `s[i] == 0`, return `0`, as this path is no longer viable
	3) Get the number of codes for the next index: `numCodes = calcWays(i + 1)`
	4) If its a double code (starts with 1 or 2), then `numCodes += calcWays(i + 2`
	5) Cache the results under `cache[i]`
	6) Return `numCodes`
3) Call `calcWays` on `i = 0` and return it

**Analysis:**
- **TC: O(n)** - Solve each subproblem, once for each character
- **SC: O(n)** - Store the result of each subproblem, once for each character
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        cache = {len(s) : 1}

        def calcWays(i: int):
            if i in cache:
                return cache[i]
            elif s[i] == "0":
                return 0
            
            numCodes = calcWays(i + 1)
            if i + 1 < len(s) 
	            and (s[i] == "1" 
	            or (s[i] == "2" and s[i + 1] in "0123456")):
	                numCodes += calcWays(i + 2)
            cache[i] = numCodes
            return numCodes
        
        return calcWays(0)
```
## References
