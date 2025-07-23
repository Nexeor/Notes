2025-07-23 02:10

Status: #Leaf

Tags: #LeetCode #Hashmaps 

# Two Sum
**LeetCode**: https://leetcode.com/problems/two-sum/description/
**GitHub**: 
## Problem
Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**
- **Input:** `nums = [2,7,11,15], target = 9`
- **Output:** `[0,1]`
- **Explanation:** Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.
**Example 2:**
- **Input:** `nums = [3,2,4], target = 6`
- **Output:** `[1,2]`
Example 3:
- **Input:** `nums = [3,3], target = 6`
- **Output:** `[0,1]`
**Constraints:**
- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**
## Solution
For each num we can calculate its *complement*: the other value that num would need to sum to `target`. We can iterate over `nums`, and record the complement of each element in a hash. As we iterate, we check to see if the complement for the current elem has been seen on a previous iteration. 
1) Setup: Create an empty hashmap
2) Iterate over `nums`
	1) Calculate `complement = target - num`
	2) If we've already seen the complement, return it's index and the current index
	3) If we haven't seen the complement, record this element and its index in the hashmap
**Analysis:**
- **TC: O(n)** - Look at each elem once
- **SC: O(n)** - Record once for each elem
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}

        for i, num in enumerate(nums):
            complement = target - num
            if complement in hashmap:
                return [hashmap[complement], i]
            hashmap[num] = i
        
        return -1
```
## References
