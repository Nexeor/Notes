2025-07-01 17:27

Status: #Leaf 

Tags: #LeetCode #Algorithms #Greedy 

# LeetCode 53 - Maximum Subarray
**LeetCode:** https://leetcode.com/problems/maximum-subarray/description/
**Github:** https://github.com/Nexeor/LeetCode/blob/main/greedy/maximum_subarray.py
## Problem
Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**
- **Input:** `nums = [-2,1,-3,4,-1,2,1,-5,4]`
- **Output:** `6`
- **Explanation:** The subarray `[4,-1,2,1]` has the largest sum `6`
**Example 2:**
- **Input:** `nums = [1]`
- **Output:** `1`
- **Explanation:** The subarray `[1]` has the largest sum `1`.
**Example 3:**
- **Input:** `nums` = `[5,4,-1,7,8]`
- **Output:** `23`
- **Explanation:** The subarray `[5,4,-1,7,8]` has the largest sum `23`.

**Constraints**: 
- `1 <= nums.length <= 105`
- `-10^4 <= nums[i] <= 10^4`
## Solution
We can use a *greedy* strategy to solve this problem. 
1) Set the cur_sum and max_sum to first num
2) Iterate over each other input num, in order
	1) If cur_sum is negative, the previous nums are not part of the maximum subarray. Set cur_sum to zero.
	2) Add the next elem to the cur_sum
	3) Update the max_sum
3) Return the max sum

**Analysis:**
- **TC: O(n)** - Look at each num once
- **SC: O(1)** - Track constant space cur_sum and max_sum
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        cur_sum, max_sum = nums[0], nums[0]

        for num in nums[1:]:
            # Cut off prefix if negative
            if cur_sum < 0:
                cur_sum = 0
            
            # Add next element
            cur_sum += num
            max_sum = max(cur_sum, max_sum)
        
        return max_sum

```

## References
Video solution: https://www.youtube.com/watch?v=5WZl3MMT0Eg