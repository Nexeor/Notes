2025-07-16 14:45

Status: #Leaf

Tags: #LeetCode #Algorithms #DynamicProgramming 

# LeetCode 213 - House Robber II
**LeetCode:** https://leetcode.com/problems/house-robber-ii/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/house_robber_II.py
## Problem
You are given an integer array `nums` where `nums[i]` represents the amount of money the `i`th house has. The houses are arranged in a circle, i.e. the first house and the last house are neighbors.

You are planning to rob money from the houses, but you cannot rob **two adjacent houses** because the security system will automatically alert the police if two adjacent houses were _both_ broken into.

Return the _maximum_ amount of money you can rob **without** alerting the police.

**Example 1:**
- **Input**: `nums = [3, 4, 3]`
- **Output:** `4`
- **Explanation:** You cannot rob `nums[0] + nums[2] = 6` because `nums[0]` and `nums[2]` are considered adjacent. The maximum you can rob is `nums[1] = 4`.  
**Example 2:**
- **Input:** `nums = [2, 9, 8, 3, 6]`
- **Output:** `15`
- **Explanation**: You cannot rob `nums[0] + nums[2] + nums[4] = 16` because `nums[0]` and `nums[4]` are adjacent houses. The maximum you can rob is `nums[1] + nums[4] = 15`. 
**Constraints:**
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`
## Solution
The solution to this problem is essentially a minor variation on the inspiring problem [[LeetCode 198 - House Robber]]. We can either start at `nums[0]` or `nums[1]`. If we start at `0`, then we cannot access `nums[-1]`, as it is considered adjacent. The same is true for `nums[1]` and `nums[-2]`. Therefore, if "start" at `nums[0]`, and ignore `nums[-1]`, we can run the original House Robber algorithm to get the max path from that index. Then we run the same algorithm starting at `nums[1]` and ignoring `nums[-2]` to get the max path from that index. Then we return the greater of those two results.
1) Run [[LeetCode 213 - House Robber II]] on `nums[:-1]` and on `nums[1:]`, returning the greater result

**Analysis:**
- **TC: O(n)** - We look at every element exactly twice
- **SC: O(1)** - Constant space `house_a` and `house_b`
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
            
        return max(self.max_rob(nums[:-1]), self.max_rob(nums[1:]))
    
    def max_rob(self, nums: List[int]) -> int:
        house_a, house_b = 0, 0

        for house in nums:
            cur_max = max(house + house_a, house_b)
            house_a = house_b
            house_b = cur_max
        
        return house_b
```
## References
