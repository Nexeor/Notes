2024-11-19 15:40

Status: #Leaf 

Tags: #LeetCode #ProgrammingConcepts #Arrays

# Contains Duplicate
**LeetCode:** https://leetcode.com/problems/contains-duplicate/description/
**Github:**
## Problem
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
- **Input:** `nums = [1,2,3,1]`
- **Output:** `true`

**Example 2:**
- **Input:** `nums = [1,2,3,4]`
- **Output:** `false`

**Example 3:** 
- **Input:** `nums = [1,1,1,3,3,4,3,2,4,2]`
- **Output:** `true`

**Constraints:**
- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
## Solution
We can use a set to track which numbers we've already seen
1) Setup: Create empty dict to track seen nums
2) Iterate over nums
	1) If we've already seen the num, return True
	2) If we haven't seen the num, mark it as seen
3) Return false if we iterate over entire array

**Analysis**:
- **TC: O(n)** - Look at each elem once
- **SC**: **O(n)** - Store each elem once
```python
class Solution(object):
    def containsDuplicate(self, nums):
        num_dict = {}
        
        for num in nums:
            if num in num_dict:
                return True
            else:
                num_dict[num] = 0
        return False
```

## References
