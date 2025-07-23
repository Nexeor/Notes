2024-11-20 17:18

Status: #Leaf

Tags: #Algorithms #LeetCode #Greedy #Stack

# Problem Name
LeetCode: https://leetcode.com/problems/valid-parenthesis-string/submissions/1694591410/
Github: https://github.com/Nexeor/LeetCode/blob/main/greedy/valid_parenthesis_string.py
### Problem
You are given a string `s` which contains only three types of characters: `'('`, `')'` and `'*'`.

Return `true` if `s` is **valid**, otherwise return `false`.

A string is valid if it follows all of the following rules:
- Every left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Every right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
- A `'*'` could be treated as a right parenthesis `')'` character or a left parenthesis `'('` character, or as an empty string `""`.
#### Example 1:
- **Input:** `s = "((**)"
- **Output:** `true`
- **Explanation:** One of the `*` can be treated as a `)` and the other as an empty string
#### Example 2:
- **Input:** `s = "(((*)`
- **Output:** `false`
- **Explanation:** The string is not valid because there is an extra `'('` at the beginning, regardless of the extra `'*'`.
#### Constraints:
- `1 <= s.length <= 100`
### Solution
Because `*` can be treated as wildcards, we can pair `(` with `)` when available and only use `*` to pair when necessary. Leftover `*` can be treated as empty spaces and ignored.
- Setup: Create two stacks, one to track left paren and another to track wildcards
1) Iterate over the string:
	1) If the character is a left paren or wildcard, push it's index onto the appropriate stack
	2) If the character is a right paren: 
		1) Pop a left paren if available
		2) Otherwise, pop a wildcard if avaliable
		3) If neither left paren nor wildcard, return False
2) Until left paren stack or wildcard stack are empty:
	1) If top wildcard's index is greater than top left paren index, pop both 
	2) Otherwise, return false (wildcard appears before left paren)
3) Return true if left paren is empty
```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        left_paren = []
        wild_card = []

        for i, char in enumerate(s):
            if char == '(':
                left_paren.append(i)
            elif char == '*':
                wild_card.append(i)
            else:
                if left_paren:
                    left_paren.pop()
                elif wild_card:
                    wild_card.pop()
                else:
                    return False
        
        while left_paren and wild_card:
            if wild_card[-1] > left_paren[-1]:
                wild_card.pop()
                left_paren.pop()
            else: 
                return False
        
        return len(left_paren) == 0     
```
##### Analysis
Complexity analysis
- **TC:** O(n) as we iterate over each elem at most twice
- **SC**: O(n) as stacks scale with input string
## References
