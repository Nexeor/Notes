2025-07-10 16:12

Status: #Leaf

Tags: #Algorithms #LeetCode #Greedy #Hashing

# LeetCode 763 - Partition Labels
LeetCode: https://leetcode.com/problems/partition-labels/description/
Github:https://github.com/Nexeor/LeetCode/blob/main/partition_labels.py
## Problem
You are given a string `s` consisting of lowercase English letters.

We want to split the string into as many substrings as possible, while ensuring that each letter appears in at most one substring.

Return a list of integers representing the size of these substrings in the order they appear in the string.

**Example 1:**
- `Input: s = "xyxxyzbzbbisl"`
- `Output: [5, 5, 1, 1, 1]`
- Explanation: The string can be split into `["xyxxy", "zbzbb", "i", "s", "l"]`.
**Example 2:**
- `Input: s = "abcabc"`
- `Output: [6]`
**Constraints:**
- `1 <= s.length <= 100`
## Solution
1) Setup: 
	1) Iterate over the input string and count the occurrences of each elem
	2) Create an empty `cur_set`dict to track the elems in the current partition
	3) Create an empty `partitions` arr to track the result
	4) Set the `cur_partition` to zero
2) Iterate over the input string
	1) Count each time we see each elem in `cur_set`
	2) Increment `cur_partition`
	3) When we've counted all instances of an elem `cur_set[elem] == count[elem]`, remove the elem from `cur_set`
	4) If `cur_set` is empty, then we've found a new partition, append `cur_partition` to `partitions` and reset `cur_partiton` to zero
3) Return the `partitions` array

**Analysis**:
- **TC: O(n)** - Look at each elem twice
- **SC: O(m)** - Track count of each unique elem, scales with size of unique elems
```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        count = Counter(s)
        seen = {}
        partitions = []
        cur_partition = 0

        for i, elem in enumerate(s):
            seen[elem] = seen.get(elem, 0) + 1
            cur_partition += 1
            
            if seen[elem] == count[elem]:
                del seen[elem]

            if not seen:
                partitions.append(cur_partition)
                cur_partition = 0
        
        return partitions
```
## References
