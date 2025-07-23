2024-11-20 17:18

Status:

Tags: #LeetCode #ProgrammingConcepts 

# Top K Frequent Elements
### Problem
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements within the array. The test cases are generated such that the answer is always **unique**. You may return the output in **any order**.
#### Example 1:
```python
Input: nums = [1,2,2,3,3,3], k = 2

Output: [2,3]
```
#### Example 2:
```python
Input: nums = [7,7], k = 1

Output: [7]
```
#### Constraints:
- - `1 <= nums.length <= 10^4`.
- `-1000 <= nums[i] <= 1000`
- `1 <= k <= number of distinct elements in nums`.
### Solutions
#### Solution: Hashing with Bucket Sort
We start by iterating over nums and counting the frequency using a dictionary. We then convert that dict into a list where the index corresponds to frequency and the nums with that frequency are stored as the values in an array. We then search backwards through this list for the top K elements. 
```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
	# 1) Count the frequency in hashmap ({ num : frequency })
	frequencies = {}
	for num in nums:
		frequencies[num] = frequencies.get(num, 0) + 1
	
	# 2) Rebuild as list with index as frequency
	frequencies = list(frequencies.items())
	count = [[] for i in range(len(nums) + 1)]
	for (num, frequency) in frequencies:
		count[frequency].append(num)
	
	# 3) Get top K elements
	k_more = k
	top_k = []
	pos = len(count) - 1
	while k_more > 0:
		if count[pos] != []:
			top_k = top_k + count[pos]
			k_more = k_more - len(count[pos])
		pos = pos - 1
	
	return top_k
```
##### Analysis
We iterate linearly over nums, frequencies, and count, but only once each, so a time complexity of approximately `O(3n) = O(n)`. It also requires linear additional space for the additional hash map and lists
- **TC:**  `O(n)`
- **SC**: `O(n`
## References
