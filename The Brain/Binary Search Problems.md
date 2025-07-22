2025-03-03 18:11

Status: #Branch

Tags: #Algorithms #ProgrammingConcepts #Search 

# Binary Search Problems
Binary search is a search algorithm for finding a target element in a **sorted** array. It works by picking an element, and checking whether the target is smaller or larger than the selected element. The search space is then divided in half: the upper half if the target is larger or the lower half if the target is smaller. 
1) Set the "left" and "right" to the start and end of the list
2) Calculate the middle index (left + right // 2)
	1) Make sure this is an integer, not a float, by taking the floor/ceiling of the operation
3) If the middle index is the target, than return the index
	1) If target is smaller than the element at the middle index, set "right" to mid  - 1
	2) Otherwise, if the target is larger than the element at the middle index, set "left" to mid + 1
4) Repeat step 3 until the target is found
	1) If left becomes greater than right, target is not in search space

**TC:** O(log(n))
**SC:** Constant (left, right, and middle indexes)

Binary search is best used when we need to find a specific element in a large, sorted, dataset. If the dataset isn't sorted, it's best to use a different algorithm or sort the data first. Binary search only applies to arrays and list type data structures.

### Basic Algorithm
The basic binary search algorithm in Python is the following:
- [Neetcode: Binary Search](https://neetcode.io/problems/binary-search)
```python
 def search(self, nums: List[int], target: int) -> int:
	l, r = 0, len(nums) - 1

	while l <= r:
		cur = (l + r) // 2
		if nums[cur] < target:
			l = cur + 1
		elif nums[cur] > target:
			r = cur - 1
		else:
			return cur
	
	return -1
```

### 2D Variation
We can modify this algorithm to search over sorted 2D arrays as well. This method applies 2 binary searches:
- First find the row who can contain the target `(row[0] < target < row[-1])`
- Then binary search within the row to find the target
- [Neetcode: Search a 2D Matrix](https://neetcode.io/problems/search-2d-matrix)

```python
def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
	# Binary search on leftmost column
	top, bot, mid = 0, len(matrix) - 1, 0
	while top <= bot:
		mid = (top + bot) // 2
		# Target greater than last in cur row -> move down a row
		if target > matrix[mid][-1]:
			top = mid + 1 
		# Target less than first in cur row -> move up a row
		elif target < matrix[mid][0]:
			bot = mid - 1
		else:
			break
	
	# Binary search on row
	arr = matrix[mid]
	l, r, mid = 0, len(arr) - 1, 0
	while l <= r:
		mid = (l + r) // 2
		if arr[mid] < target:
			l = mid + 1
		elif arr[mid] > target:
			r = mid - 1
		else: 
			return True

	return False
```

### Using Binary Search on a Search Space
We can also use binary search to more quickly find a solution to a problem that has a defined search range. 

For example, in the [Koko Eating Bananas](https://neetcode.io/problems/eating-bananas) problem we have a series of banana piles that must be consumed within h hours, and we can only eat from one pile each hour. What must our eating speed (k) be to consume all the piles within the h hours? 

To apply binary search to this we must create a search range by finding the max and min possible values for k, the eating speed:
- The slowest possible eating speed is 1, otherwise we would never eat any bananas
- The fastest possible eating speed is the size of the largest pile, as we can never move between piles within a single hour. Therefore the fastest we can eat a pile is eating the largest pile in a single hour.

We now have a range of \[1, 2, 3, ..., m] where m is the size of the largest pile. Now that we have a search range we can perform binary search, calculating whether its possible to eat all the bananas with the selected time limit. We repeat this until we reach an eating speed that is possible, but the speed immediately to the left is not. This will be the minimum possible eating speed. 
## References
