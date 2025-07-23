2025-07-14 14:27

Status: #Leaf

Tags: #Algorithms #LeetCode #DynamicProgramming

# Problem Name
LeetCode: https://leetcode.com/problems/climbing-stairs/description/
Github: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/climbing_stairs.py
### Problem
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?
#### Example 1:
- **Input:** `n = 2`
- **Output:** `2`
- **Explanation:** Two ways to reach top step:
	1) 1 + 1
	2) 2
#### Example 2:
- **Input:** `n = 3`
- **Output:** `3`
- **Explanation:** Three ways to reach top step:
	1) 1 + 1 + 1
	2) 1 + 2
	3) 2 + 1
#### Constraints:
- `1 <= n <= 45`
### Solutions
We can use "bottom" up DP by starting at the base-case and working our way backwards. We only need to track the last two steps, as each step is only dependent on the two steps before it. 
- Setup: Create `step_a` and `step_b` to track the steps to reach the `nth` and `nth + 1` steps. Set both to `1` (one way to reach the first and second step)
1) Iterate over each step
	1) Set `step_b` to `step_a + step_b` (`nth` step = `n - 2` + `n - 1`)
	2) Set `step_a` to previous value of `step_b`
2) Return `step_a`
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        step_a = 1
        step_b = 1

        for i in range(0, n):
            temp = step_b
            step_b = step_a + step_b
            step_a = temp

        return step_a
```
##### Analysis
Complexity analysis
- **TC:** O(n) as we iterate over each step once
- **SC**: O(1) for constant space `step_a` and `step_b`
## References
NeetCode solution: https://www.youtube.com/watch?v=Y0lT9Fck7qI