2025-07-23 13:25

Status: #Leaf

Tags: #LeetCode #DynamicProgramming #PrefixSuffix

# LeetCode 152 - Maximum Product Subarray
**LeetCode**: https://leetcode.com/problems/maximum-product-subarray/description/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/1D_Dynamic/maxium_product_subarray.py
## Problem
Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**
- **Input:** `nums = [2,3,-2,4]`
- **Output:** `6`
- **Explanation:** `[2,3] has the largest product 6.
**Example 2:**
- **Input:** `nums = [-2,0,-1]`
- **Output:** `0`
- **Explanation:** `The result cannot be 2, because [-2,-1] is not a subarray.`
**Constraints:**
- `1 <= nums.length <= 2 * 104`
- `-10 <= nums[i] <= 10`
- The product of any subarray of `nums` is **guaranteed** to fit in a **32-bit** integer.
## Solution
The key insight here is that the answer is dependent on the number of negatives and zeroes :
- If there are no negative element or an even number of negative elements, the answer will be the entire array
- If there is an odd number of negative elements, then one of the negative elements divides the array into a "left" and a "right" portion, one of which will be correct subarray. 
- Each zero essentially divides the array into multiple arrays, as including a zero will always result in a product of zero

Case 1: All Positives `[1, 2, 3, 4]`
- Take the total product of the array
Case 2: Even Negatives `[1, -2, 3, -4]`
- Product becomes positive again after two negatives
Case 3: Odd Negatives `[1, 2, -3, 4]`
- Can't just take product: negative flips sign
- Can't just multiply forward, because right portion is the max: 
	- `max = max(1, 1 * 2) = 3`
	- `max = max(3, 3 * -3) = 3`
	- `max = max(3, -9 * 4) = -36`
- However, if we instead multiply backwards:
	- `max = max(4, 4 * -3) = 4`
	- `max = max(4, -12 * 2) = 4`
	- `max = max(4, -24 * -2) = 4`
	- We do get the correct max product!
Case 4: Zeroes `[1, -2, 0, -3, 4]`
- Zeroes are never included unless zero is the max, as their product will always ben zero
- Therefore we treat zeroes as "resets" that reset the current prefix or suffix to one

From the above cases we can see that the max will either be the prefix or the suffix. We calculate both and return the greater value. 
1) Setup: Set answer to greatest num, set prefix and suffix to 1
	1) If the greatest subarray is a single value, this setup will catch that
2) Iterate over nums:
	1) Calculate new prefix and suffix
	2) If prefix or suffix is greater than ans, set ans to that
	3) If prefix or suffix is zero, reset them to 1
3) Return the answer
**Analysis:**
- **TC: O(n)** - Iterate over nums twice: once for max and once for prefix/suffix
- **SC: O(1)** - Constant space cur_max, prefix, suffix
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        cur_max = max(nums)
        prefix, suffix = 1, 1

        for i, num in enumerate(nums):
            prefix = prefix * num
            suffix = suffix * nums[-i - 1]
            cur_max = max(cur_max, prefix, suffix)
            if prefix == 0:
                prefix = 1
            if suffix == 0:
                suffix = 1

        return cur_max
```
## References
https://www.youtube.com/watch?v=hnswaLJvr6g