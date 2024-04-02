Given an array of integers, return true if any element in the array is repeated. Return false otherwise

**Example 1:**
**Input:** nums = [1,2,3,1]
**Output:** true

**Example 2:**
**Input:** nums = [1,2,3,4]
**Output:** false

**Example 3:**
**Input:** nums = [1,1,1,3,3,4,3,2,4,2]
**Output:** true

## Approach 1: Brute Force
We can brute force the solution by checking every elem against every other elem
- TC: O(n^2)
- SC: In-Place algorithm

Code: 
```
class Solution(object):
    def containsDuplicate(self, nums):
        for elem in nums:
            if nums.count > 1:
                return false
```

Problem: Fails at large array sizes

## Approach 2: Sorting
By sorting the array, we can compare each elem with just its neighbor to check for duplicates
- TC(nlogn)
- SC: In-place algorithm

Code:
```
class Solution(object):

    def containsDuplicate(self, nums):

        nums = sorted(nums)

        for i in range(1, len(nums)):

            if nums[i] == nums[i - 1]:

                return True

        return False
```

## Approach 3: Hashset
A hashset can be used instead of a list. For each elem, attempt to add it to the hashset. If it fails, then we have found a duplicate. 

To do: Write code for this solution



