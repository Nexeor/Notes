2024-11-19 15:40

Status:

Tags: #LeetCode #ProgrammingConcepts #Arrays

# Contains Duplicate

### Problem
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
- **Input:** nums = [1,2,3,1]
- **Output:** true
**Explanation:** The element 1 occurs at the indices 0 and 3.

**Example 2:**
- **Input:** nums = [1,2,3,4]
- **Output:** false
**Explanation:** All elements are distinct.

**Example 3:** 
- **Input:** nums = [1,1,1,3,3,4,3,2,4,2]
- **Output:** true

**Constraints:**
- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

### Solution
#### Solution 1: Sorting
If we sort the array before traversal, duplicate elements will always appear adjacent to each other:
```python
 def containsDuplicate(self, nums):
        nums = sorted(nums)
        
        for num_pos in range(1, len(nums)):
            if nums[num_pos - 1] == nums[num_pos]:
                return True
        return False
```
**Analysis**: 
- **TC:** O(nlogn) 
- **SC**: O(1)
#### Solution 2: Hashing
As we traverse the array, we hash each element as a key with a value of 0. If we ever attempt to hash a key that already exists in the hash, then we 
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
**Analysis**:
- **TC:**: O(n)
- **SC**: O(n)
## References
