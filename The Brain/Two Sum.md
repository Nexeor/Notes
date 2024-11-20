2024-11-19 16:57

Status:

Tags: #LeetCode #ProgrammingConcepts 

# Two Sum
### Problem
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

Example 1:
- Input: nums = [2,7,11,15], target = 9
- Output: [0,1]
- Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:
- Input: nums = [3,2,4], target = 6
- Output: [1,2]
Example 3:
- Input: nums = [3,3], target = 6
- Output: [0,1]
### Solution: Hashing
In this solution we iterate through the array only once by using a hash map. As we traverse through the array we store each number and its corresponding index. This way, when we reach a new number, we can calculate the "complement" (the value that when summed with the new number will result in the target). We then query the hashmap for the complement, and if it exists we know that the complement already appeared earlier in the array. We can then return our current position and the complement's position (which was stored in the hashmap as the value)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Create a hash
        complements = {}
        for pos in range(0, len(nums)):
            # Subtract the number from the target to get the complement
            complement = target - nums[pos]
            # Search the hash for the complement
            if complement in complements:
                return [pos, complements[complement][0]]
            # Didn't find complement, so hash this num
            else:
	            # check for duplicates
                if nums[pos] in complements:
                    complements[nums[pos]].append(pos)
                else:
                    complements[nums[pos]] = [pos]
        
```
**Analysis**: Fastest TC and SC. Only iterates over nums once and requires a single linear sized hash for additional space. 
- **TC:** O(n) 
- **SC**: O(n)
## References
