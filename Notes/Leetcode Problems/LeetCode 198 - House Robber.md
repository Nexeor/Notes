2025-07-10 15:18

Status: #Leaf 

Tags: #LeetCode #Algorithms #DynamicProgramming

# LeetCode 198 - House Robber
**LeetCode:** https://leetcode.com/problems/house-robber/description/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/house_robber.py
## Problem
You are given an integer array `nums` where `nums[i]` represents the amount of money the `i`th house has. The houses are arranged in a straight line, i.e. the `i`th house is the neighbor of the `(i-1)`th and `(i+1)`th house.

You are planning to rob money from the houses, but you cannot rob **two adjacent houses** because the security system will automatically alert the police if two adjacent houses were  _both_ broken into.

Return the _maximum_ amount of money you can rob **without** alerting the police.

**Example 1:**
- **Input**: `nums = [1, 1, 3, 3]`
- **Output:** `4`
- **Explanation:**`nums[0] + nums[2] = 1 + 3 = 4`
**Example 2:**
- **Input:** `nums = [2, 9, 8, 3, 6]`
- **Output:** `16`
- **Explanation**:
**Constraints:**
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`
## Solution
At each number we have a choice: we include it or we don't. However, if we do choose a number, then we cannot choose it's neighbors. We can apply a *bottom-up* DP approach by using a greedy strategy to track the highest possible route so far seen amongst all numbers.
1) Create values `house_a` to represent the greatest path seen so far, and `house_b` to represent the maximum path to the previous house. Assign both to 0.
2) Iterate over `nums`
	1) Get max of `house + house_a` vs `house_b` 
		1) If `cur_house + house_a > house_b` then a path including `cur_house` is the maximum path, otherwise `house_b` must be in the path and not `cur_house`
	2) Either way, `house_a = house_b` to move down `nums`
	3) Then `house_b = max`, as the maximum path to `house_b` is the `max` determined easleir

**Analysis:**
- **TC: O(n)** - Look at each elem once
- **SC: O(1)** - Constant space `house_a` and `house_b`
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        house_a, house_b = 0, 0

        for house in nums:
            cur_max = max(house + house_a, house_b)
            house_a = house_b
            house_b = cur_max
        
        return house_b
```
## References
Video solution: https://www.youtube.com/watch?v=73r3KWiEvyk