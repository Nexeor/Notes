2025-07-23 15:23

Status: #Leaf

Tags: #LeetCode #Arrays #PrefixSuffix 

# LeetCode 238 - Product of Array Except Itself
**LeetCode**: https://leetcode.com/problems/product-of-array-except-self/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/arrays_and_hashing/product_except_self.py
## Problem
Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**
- **Input:** `nums = [1,2,3,4]`
- **Output:** `[24,12,8,6]`
**Example 2:**
- **Input:** `nums = [-1,1,0,-3,3]`
- **Output:** `[0,0,9,0,0]`
**Constraints:**
- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- The input is generated such that `answer[i]` is **guaranteed** to fit in a **32-bit** integer.
## Solution
To calculate the product of each position, we need the product of all elems before (the **prefix**) and the product of all elems after (the **suffix**). If, for each position, we record its prefix and suffix, we can simply multiply them to get the result.
1) Setup: Create prefix and suffix arrays
2) Iterate forwards over nums, recording the prefix for each pos
3) Iterate backwards over nums, recording the postfix for each pos
4) Multiply the suffix and prefix of each position
**Analysis:**
- **TC: O(n)** - We consider each elem exactly three times
- **SC: O(n)** - For each elem, record a prefix and a postfix
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        suffix = [1] * len(nums)

        cur_prod = 1
        for i in range(1, len(nums)):
            cur_prod *= nums[i-1]
            prefix[i] = cur_prod
        
        cur_prod = 1
        for i in range(len(nums) - 2, -1, -1):
            cur_prod *= nums[i+1]
            suffix[i] = cur_prod 
        
        ans = []
        for i in range(0, len(nums)):
            ans.append(prefix[i] * suffix[i])

        return ans
```
## References