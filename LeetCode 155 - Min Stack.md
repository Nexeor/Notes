2025-07-24 14:41

Status: #Leaf

Tags: #LeetCode #Stack

# LeetCode 155 - Min Stack
**LeetCode**: https://leetcode.com/problems/min-stack/description/
**GitHub**:
## Problem
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1:**
**Input**
- `["MinStack","push","push","push","getMin","pop","top","getMin"]`
- `[[],[-2],[0],[-3],[],[],[],[]]`
**Output**
- `[null,null,null,null,-3,null,0,-2]`
**Explanation**
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```
**Constraints:**
- `-231 <= val <= 231 - 1`
- Methods `pop`, `top` and `getMin` operations will always be called on **non-empty** stacks.
- At most `3 * 104` calls will be made to `push`, `pop`, `top`, and `getMin`.
## Solution
The only challenge here is implementing `getMin` in `O(1)` time, meaning we have to maintain the current minimum as we add and remove elems from the stack. However, if that minimum is later removed from the stack we will need to update to the new minimum in `O(1)` time as well. We can do this by maintaining a separate `min_stack` alongside our normal stack. We only push to this min stack when we encounter a new (or equal) minimum, and only pop from it when the corresponding elem is popped from the main stack
1) Setup: Create `stack` and `min_stack`
2) When pushing onto `stack`, if elem is equal to or smaller than the minimum, or `min_stack` is empty, push it onto `min_stack` as well
3) When popping from `stack`, if the popped elem is the top of `min_stack`, pop from `min_stack` as well
![[Pasted image 20250724150101.png]]
**Analysis:**
- **TC: O(1)** - All operations are constant time
- **SC: O(n)** - Stack(s) scale linearly with input
```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []


    def push(self, val: int) -> None:
        self.stack.append(val)
        
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        

    def pop(self) -> None:
        if self.min_stack[-1] == self.stack[-1]:
            self.min_stack.pop()
        return self.stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
        
```
## References
