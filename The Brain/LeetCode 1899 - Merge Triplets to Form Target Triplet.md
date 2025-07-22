2025-07-10 15:18

Status: #Leaf 

Tags: #LeetCode #Algorithms #Greedy 

# LeetCode 1899 - Merge Triplets to Form Target Triplet
**LeetCode:** https://leetcode.com/problems/merge-triplets-to-form-target-triplet/description/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/greedy/merge_triplets.py
## Problem
You are given a 2D array of integers `triplets`, where `triplets[i] = [ai, bi, ci]` represents the `ith` **triplet**. You are also given an array of integers `target = [x, y, z]` which is the triplet we want to obtain.

To obtain `target`, you may apply the following operation on `triplets` zero or more times. Choose two **different** triplets `triplets[i]` and `triplets[j]` and update `triplets[j]` to become `[max(ai, aj), max(bi, bj), max(ci, cj)]`.  
* E.g. if `triplets[i] = [1, 3, 1]` and `triplets[j] = [2, 1, 2]`, `triplets[j]` will be updated to `[max(1, 2), max(3, 1), max(1, 2)] = [2, 3, 2]`.

Return `true` if it is possible to obtain `target` as an **element** of `triplets`, or `false` otherwise.

**Example 1:**
- `Input: triplets = [[1,2,3],[7,1,1]], target = [7,2,3]`
- `Output: True`
- Choose the first and second triplets, update the second triplet to be: `[max(1, 7), max(2, 1), max(3, 1)] = [7, 2, 3].`
**Example 2:**
- `Input: triplets = [[2,5,6],[1,4,4],[5,7,5]], target = [5,4,6]`
- `Output: false`
**Constraints:**
- `1 <= triplets.length <= 1000`
- `1 <= ai, bi, ci, x, y, z <= 100`
## Solution
It can be intuited that any triplet with an elem greater than the target cannot be included in the merge. For instance `[5, 7, 5]` in example 2 cannot be used, as merging it with any other array will result in a middle value of `7`, greater than the target. 

Using this intuition we can use a *greedy* algorithm: first checking if the triplet is valid and then checking if it contains a target value:
1) Setup: Create `has_seen` array of three `false` values to track whether we've seen the target
2) Iterate over each triplet
	1) If the triplet contains an elem that is greater than it's corresponding target, skip it
	2) Otherwise, if the triplet contains a target elem, mark it `true` in `has_seen`
3) Return `true` if `has_seen` is all `true`. Otherwise return `false` 

**Analysis:**
- **TC: O(n)** - Look at each elem in each triplet once
- **SC: O(1)** - Constant space of `has_seen` array
```python
class Solution:
    def mergeTriplets(self, triplets: List[List[int]], target: List[int]) -> bool:
        has_appeared = [False, False, False]

        for triplet in triplets:
            is_valid = True
            for i, elem in enumerate(triplet):
                if elem > target[i]:
                    is_valid = False

            if not is_valid:
                continue

            for i, elem in enumerate(triplet):
                if elem == target[i]:
                    has_appeared[i] = True

        return all(has_appeared)
```
## References
Video solution: https://www.youtube.com/watch?v=kShkQLQZ9K4