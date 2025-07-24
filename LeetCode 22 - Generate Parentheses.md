2025-07-24 15:28

Status: #Leaf

Tags: #LeetCode #Stack

# LeetCode 22 - Generate Parentheses
**LeetCode**: https://leetcode.com/problems/generate-parentheses/description/
**GitHub**:
## Problem
Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**
- **Input:** `n = 3`
- **Output:** `["((()))","(()())","(())()","()(())","()()()"]`
**Example 2:**
- **Input:** `n = 1`
- **Output:** `["()"]`
**Constraints:**
- `1 <= n <= 8`
## Solution
This is closer a backtracking problem than a stack problem, but we can use a stack to improve the performance. At each step we choose whether to add an opening or closing parentheses, following valid placement rules. Using backtracking we can explore all placement possibilities, using a stack to track the current paren being considered. This is more efficient that passing a concatenated string. 
1) Create `paren` stack and `valid_paren` list
2) Write `generate()` function that takes the number of open and closed parens
	1) Base Case: If we have open and closed parens equal to n, then save as valid answer and return
	2) If we haven't used all open parens, recurse on `num_open + 1`
		1) Remember to push/pop from stack before and after recursing
	3) If we haven't used all closed parens and we have more open than closed, recurse on `num_closed + 1`
		1) Remember to push/pop from stack before and after recursing
3) Call `generate(0, 0)` and return saved parens
**Analysis:**
- **TC: O(4^[n]/n^[3/2])** - Builds a number of parens equivalent to the nth Catalan number
- **SC: O(n)** - `cur_paren` scales linearly to `n`
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        cur_paren = []
        valid_paren = []

        def generate(num_open, num_closed):
            if num_open == num_closed == n:
                valid_paren.append("".join(cur_paren))
                return 

            if num_open < n:
                cur_paren.append("(")
                generate(num_open + 1, num_closed)
                cur_paren.pop()
            if num_closed < num_open and num_closed < n:
                cur_paren.append(")")
                generate(num_open, num_closed + 1)
                cur_paren.pop()

        generate(0, 0)
        return valid_paren
```
## References
