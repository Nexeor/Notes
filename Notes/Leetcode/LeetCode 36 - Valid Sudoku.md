2025-07-23 15:30

Status: #Leaf

Tags: #LeetCode #Hashmaps

# LeetCode 36 - Valid Sudoku
**LeetCode**: https://leetcode.com/problems/valid-sudoku/description/
**GitHub**: https://github.com/Nexeor/LeetCode/blob/main/arrays_and_hashing/valid_sudoku.py
## Problem
Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:
1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**
- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**
![[Pasted image 20250723154951.png]]
- **Output:** `true`
**Constraints:**
- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`.
## Solution
We can use a hashmap to track which elements we've seen in which positions. If we see a duplicate elem we can return false.
1) Setup: Create empty dict of sets for rows, columns, sub_boxes
	1) `rows[i]` is a set containing the values seen in the `ith` row so far 
	2) Same applies for `columns` and `sub_boxes`
2) Iterate over each row:
	1) Iterate over each column:
		1) Skip the elem if it's empty (`'.'`)
		2) Return `false` if we've already seen that elem in that row/column/square
		3) Add that elem to the row/column/square set
3) Return `true`
**Analysis:**
- **TC: O(n)** - Iterate over each elem once
- **SC: O(n)** - Store each elem once
```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Create dicts
        rows = collections.defaultdict(set)
        columns = collections.defaultdict(set)
        sub_boxes = collections.defaultdict(set)

        for row_index, row in enumerate(board):
            for col_index, col in enumerate(row):
                if board[row_index][col_index] != '.':
                    if (board[row_index][col_index] in rows[row_index]
                        or board[row_index][col_index] in columns[col_index]
                        or board[row_index][col_index] in sub_boxes[row_index // 3, col_index // 3]):
                        return False
                    
                    rows[row_index].add(board[row_index][col_index])
                    columns[col_index].add(board[row_index][col_index])
                    sub_boxes[(row_index // 3, col_index // 3)].add(board[row_index][col_index])
        print(rows)
        print(columns)
        print(sub_boxes)
        return True
```
## References
