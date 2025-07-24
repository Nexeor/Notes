2025-07-24 14:18

Status: #Leaf

Tags: #LeetCode #Stack

# LeetCode 20 - Valid Parentheses
**LeetCode**: https://leetcode.com/problems/valid-parentheses/description/
**GitHub**:
## Problem
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**
- **Input:** `s = "()"`
- **Output:** `true`
**Example 2:**
- **Input:** `s = "()[]{}"`
- **Output:** `true`
**Example 2:**
- **Input:** `s = "([)]"`
- **Output:** `false`
**Constraints:**
- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.
## Solution
This is a simple stack problem. If we encounter an opening paren, we push it onto the stack. If we encounter a closing paren, we inspect the top elem of the stack and see if it is a matching paren. If it isn't, we return false. Continue this until we've processed all the characters. If the stack is empty then all parens are accounted for, return true
1) Setup: Create stack and lookup table for opening -> closing parens
2) Iterate over each character
	1) If the character is an opening paren `{[(`, then push it onto the stack
	2) If the character is a closing paren `]})`, then pop the top elem from the stack
		1) If the popped paren does not match the closing paren, return `false`
3) If the stack is empty, then all parens accounted for, return `true`
**Analysis:**
- **TC: O(n)** - We look at each paren once
- **SC: O(n)** - Stack scales linearly to input
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 != 0:
            return False
        
        stk = []
        lookup = {"{" : "}", "[" : "]", "(" : ")"}
        for char in s:
            if char in "{[(":
                stk.append(char)
            else:
                if not stk or lookup[stk.pop()] != char:
                    return False
        
        return len(stk) == 0
```
## References
