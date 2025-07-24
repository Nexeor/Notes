2025-07-24 16:17

Status: #Leaf

Tags: #LeetCode #Stack

# LeetCode 739 - Daily Temperatures
**LeetCode**: https://leetcode.com/problems/daily-temperatures/description/
**GitHub**:
## Problem
Given an array of integers `temperatures` represents the daily temperatures, return _an array_ `answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ `ith` _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Example 1:**
- **Input:** `temperatures = [73,74,75,71,69,72,76,73]`
- **Output:** `[1,1,4,2,1,1,0,0]`
**Example 2:**
- **Input:** `temperatures = [30,40,50,60]`
- **Output:** `[1,1,1,0]`
**Constraints:**
- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`
## Solution
The key insight in this problem is that we can remember previous temperatures as we iterate. Then, when we encounter a higher temperature, we record the difference in days between the recorded and current element. We can use a *stack* to track previous temperatures and the day they occured (`i`). Then, if our new temp is higher than the top elem of the stack, we can pop off that elem and record the difference between the day we saw that temp and our current day. Repeat this until the stack is greater than the current temp (or empty), then push the current temp onto the stack so we can record it for later. 
1) Setup: Create `ans` array filled with zeroes and empty `stack`
2) Iterate over `temperatures`
	1) While our stack isn't empty and the current `temp` is greater than the top of stack:
		1) Pop the top of the stack
		2) Record the difference between the popped elem day and current day in `ans[day]`
	2) Push the current temp and current day onto the stack
3) Return `ans` array

**Analysis:**
- **TC: O(n)** - Iterate over each elem once
- **SC: O(n)** - Stack and ans scales linearly with input size 
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        ans = [0] * len(temperatures)
        stack = []

        for i, temp in enumerate(temperatures):
            while stack and stack[-1][0] < temp:
                prev_temp, index  = stack.pop()
                ans[index] = i - index
            stack.append((temp, i))

        return ans
```
## References
