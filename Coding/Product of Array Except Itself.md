2024-11-20 17:18

Status: 

Tags: #LeetCode #ProgrammingConcepts 

# Product of Array Except Self
### Problem
Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`. Each product is **guaranteed** to fit in a **32-bit** integer.
#### Example 1:
```python
Input: nums = [1,2,4,6]

Output: [48,24,12,8]
```
#### Example 2:
```python
Input: nums = [-1,0,1,2,3]

Output: [0,-6,0,0,0]
```
#### Constraints:
- `2 <= nums.length <= 1000`
- `-20 <= nums[i] <= 20`
### Solutions
#### Solution: Prefix and Postfix Notation
Each index of the final solution is the product of all numbers before it and all numbers after it. Therefore we can simplify each position into a product of two numbers, expressed as a tuple. Iterate forwards through the array, saving the first value of each tuple as the cumulative product of each number before it. Iterate backwards through the array, saving the second value of each tuple as the cumulative product of each number after it. Then, at the end, multiply the tuples together. 

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        products = [(1, 1)] * len(nums)
        product = 1
        # Iterate forwards for prefix
        for pos in range(1, len(nums)):
            product = product * nums[pos - 1]
            products[pos] = (product, 1)
        # Iterate backwards for suffix
        product = 1
        for pos in range(len(nums) - 2, -1, -1):
            product = product * nums[pos + 1]
            products[pos] = (products[pos][0], product)
        # Combine tuples
        final_products = []
        for (prefix, suffix) in products:
            final_products.append(prefix * suffix)
        return final_products
```
##### Analysis
We iterate over the array linearly three times, meaning we have linear time complexity. We also store an additional tuple array that is linearly sized to the input array.
- **TC:** O(n)
- **SC**: O(1)
## References
