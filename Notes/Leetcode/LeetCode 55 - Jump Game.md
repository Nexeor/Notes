2025-07-01 17:24

Status: #Greedy #LeetCode #Algorithms 

Tags: #Leaf 

# LeetCode 55 - Jump Game
**LeetCode:** https://leetcode.com/problems/jump-game/description/
**Github:** https://github.com/Nexeor/LeetCode/blob/main/greedy/jump_game.py

## Problem
You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.

**Example 1:**
- **Input:** `nums` = `[2,3,1,1,4]`
- **Output:** `true`
- **Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**
- **Input:** `nums` = `[3,2,1,0,4]`
- **Output:** false
- **Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

**Constraints:**
- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`
## Solution
We can use a *greedy* algorithm to solve this problem:
- Setup: `goal_pos = len(nums) - 1`
1) Iterate backwards from `goal_pos` 
	1) If `i + nums[i] >= goal_pos` then we can jump from `i` to `goal_pos`: `i` becomes new `goal_pos`, continue iterating
2) If we reach `i = 0` and `goal_pos = 0` than there is a valid path, `return true`
3) Otherwise, If we reach `i = 0` and `goal_pos != 0` than there is no valid path, `return false`

**Analysis:**
- **TC: O(n)** - Look at each num once 
- **SC: O(1)** - Track constant space `goal_pos` and `i`
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        goal = len(nums) - 1

        for pos in range(len(nums) - 1, -1, -1):
            if nums[pos] >= goal - pos:
                goal = pos
        
        return goal == 0
```
## References
Video solution: https://www.youtube.com/watch?v=Yan0cv2cLy8