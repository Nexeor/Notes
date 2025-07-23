2025-07-14 14:27

Status: #Leaf

Tags: #Algorithms #Dynamic-Programming #LeetCode 

# Problem Name
### Problem
LeetCode: https://leetcode.com/problems/min-cost-climbing-stairs/submissions/1697907285/
Github: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/min_cost_climbing_stairs.py
#### Example 1:
- **Input:** `cost = [10,15,20]`
- **Output:** `15`
- **Explanation:**
	1) Start at index 1
	2) Pay 15: Climb two steps to top
	3) Return 15
#### Example 2:
- **Input:** `cost = [1,100,1,1,1,100,1,1,100,1]`
- **Output:** `6`
- **Explanation:**
	1) Start at index 0
	2) Pay 1, jump two to 2
	3) Pay 1, jump two to 4
	4) Pay 1, jump two to 6
	5) Pay 1, jump once to 7
	6) Pay 1, jump twice to 9
	7) Pay 1, jump once to top
	8) Total cost: `1 + 1 + 1 + 1 + 1 + 1 = 6`
#### Constraints:
- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`
### Solutions
By starting at the end and looking back, we can choose the most efficient jump at each step:
1) Iterate over array, starting at `len(cost) - 3` (the third to last elem)
	1) At each position, record the cost of reaching that position as `min(pos - 1, pos - 2) + pos`
		1) Greedy rule: always jump from minimum cost 
2) Return the minimum of the cost from the first pos or second pos, as we can start from either
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        for i in range(len(cost) - 3, -1, -1):
            cost[i] = min(cost[i + 1], cost[i + 2]) + cost[i]

        return min(cost[0], cost[1])
```
##### Analysis
Complexity analysis
- **TC:** O(n), as we iterate over the cost list once
- **SC**: O(0), we use the provided `cost` array in place, so no extra memory is needed 
## References
