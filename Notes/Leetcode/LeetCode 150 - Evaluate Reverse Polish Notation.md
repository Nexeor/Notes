2025-07-24 15:05

Status: #Leaf

Tags: #LeetCode 

# LeetCode 150 - Evaluate Reverse Polish Notation
**LeetCode**: https://leetcode.com/problems/evaluate-reverse-polish-notation/description/
**GitHub**:
## Problem
You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return _an integer that represents the value of the expression_.

**Note** that:
- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Example 1:**
- **Input:** `tokens = ["2","1","+","3","*"]`
- **Output:** `9`
- **Explanation:** `((2 + 1) * 3) = 9`
**Example 2:** 
- **Input:** `tokens = ["4","13","5","/","+"]`
- **Output:** `6`
- **Explanation:** `(4 + (13 / 5)) = 6`
**Constraints:**
- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.
## Solution
This is a rather straightforward stack problem. Each non-op character is pushed onto the stack. For each op character we pop off the last two elems and then use it to complete the operation, pushing the result back onto the stack. At the end `stack[0]` will be the result. 

**Analysis:**
- **TC: O(n)** - Iterate over each character once
- **SC: O(n)** - Stack grows linearly with `tokens`
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []

        for token in tokens:
            if token == "+":
                stack.append(stack.pop() + stack.pop())
            elif token == "-":
                to_subtract = stack.pop()
                target = stack.pop()
                stack.append(target - to_subtract)
            elif token == "*":
                stack.append(stack.pop() * stack.pop())
            elif token == "/":
                divisor = stack.pop()
                dividend = stack.pop()
                stack.append(int(dividend / divisor))
            else:
	            stack.append(int(token))
        
        return stack[0]
```
## References
