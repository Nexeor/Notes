2025-07-23 16:16

Status: #Leaf

Tags: #LeetCode #Hashmaps

# LeetCode 128 - Longest Consecutive Sequence
**LeetCode**: https://leetcode.com/problems/longest-consecutive-sequence/description/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/arrays_and_hashing/longest_consecutive_sequence.py
## Problem
Given an **unsorted** array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**
- **Input:** `nums = [100,4,200,1,3,2]`
- **Output:** `4`
- **Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`
**Example 2:**
- **Input: `nums = [0,3,7,2,5,8,4,6,0,1]`**
- **Output:** `9`
**Example 3:**
- **Input:** `nums = [1,0,1,2]`
- **Output:** `3`
**Constraints:**
- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
## Solution
This is a simple set problem. As we iterate over the array, we create a set containing each unique element. We can then use that set to verify whether element `i + 1` exists, allowing us to count the length of each subsequence. 
1) Setup: Create a set of the given numbers
2) Iterate over each unique num in the set
	1) If `num - 1` exists in the set, then this isn't the start of a unique sequence, so skip it
	2) Otherwise, count how many times `num + 1` exists to get the len of the sequence
3) Return the len of the greatest sequence
**Analysis:**
- **TC: O(n)** - Iterate over each element at most three times
- **SC: O(n)** - Store each elem once
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        hashmap = defaultdict(int)
        for num in nums:
            hashmap[num] += 1

        max_len = 0
        for num in hashmap.keys():
            if num - 1 in hashmap:
                continue
            
            cur_len = 1
            while num + 1 in hashmap:
                cur_len += 1
                num += 1
            max_len = max(max_len, cur_len)
        return max_len
```
## References
