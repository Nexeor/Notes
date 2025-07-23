2025-07-23 03:13

Status: #Leaf

Tags: #LeetCode #Hashmaps

# LeetCode 347 - Top K Frequent Elements
**LeetCode**: https://leetcode.com/problems/top-k-frequent-elements/description/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/arrays_and_hashing/top_k_frequent_elements.py
## Problem
Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

**Example 1:**
- **Input:** `nums = [1,1,1,2,2,3], k = 2`
- **Output:** `[1,2]`
**Example 2:**
- **Input:** `nums = [1], k = 1`
- **Output:** `[1]`
**Constraints:**
- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.
## Solution
We can solve this problem in linear time by taking a three step process. First we count how many times each elem appears. Then we create a 2-D array where the index corresponds to the frequency, and we store the numbers with that frequency at that index. This will organize all of our elems along an array, with the index being how many times that elem appears. Then we iterate backwards over that 2-D array and copy the first `k` elems we see. 
1) Map the frequency of each character
2) Create a new map that reverses the previous map: At index `i` we get the characters that have appeared `i` times
3) Iterate backwards over the new map and return the first `k` results
**Analysis:**
- **TC: O(n)** - Iterate over nums twice
- **SC: O(n)** - Create two different maps scaling to nums 
```python
class Solution(object):
    def topKFrequent(self, nums, k):
        hashmap = {}
        for num in nums:
            hashmap[num] = 1 + hashmap.get(num, 0)
        
        freq_map = [[] for _ in range(0, len(nums) + 1)]
        for num, freq in hashmap.items():
            freq_map[freq].append(num)

        top_k = []
        for freq in reversed(freq_map):
            for num in freq:
                top_k.append(num)
                if len(top_k) == k:
                    return top_k
```
## References
