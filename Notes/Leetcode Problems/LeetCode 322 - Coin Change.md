2025-07-21

Status: #Leaf

Tags: #LeetCode #Algorithms #DynamicProgramming 

# LeetCode 91 - Decode Ways
**LeetCode:** https://leetcode.com/problems/coin-change/description/
**Github**: https://github.com/Nexeor/LeetCode/blob/main/1D-Dynamic/coin_change.py
## Problem
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**
- **Input**: `coins = [1, 2, 5], amount = 11`
- **Output:** `2`
- **Explanation:** `11 = 5 + 5 + 1`
**Example 2:**
- **Input:** `s = [2], amount = 3
- **Output:** `-1`
**Example 3:**
- **Input:** `s = [1], amount = 0`
- **Output:** 0
**Constraints:**
-- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`
## Solution
While a greedy approach looks possible, we actually cannot always choose the largest denomination that is smaller than the target because we don't know if the other smaller coins will sum with that one to form the result. As such, we have to do a decision tree. We start with the total amount, deduct each coin from it, and then repeat recursively on that new amount. We can *cache* the smallest result for each amount to only solve for each amount once. 
1) Setup: Create an empty cache
2) Recursive function: `calcChange(amount)`
	1) Base case: `cur_amount == 0`, we've reached the total -> return 0
	2) Base case: `cur_amount in cache` -> return cached result
	3) Otherwise, for each coin:
		1) If `cur_amount - coin >= 0`, then this is a valid pass, call `calcChange` recursively with the new amount
	4) Cache the minimum result found from the recursive calls. This is now the min number of coins to solve for this amount. 
3) Call calcChange(amount)
4) Return -1 if calcChange returns a value greater than amount (no way to reach amount with given coins)
5) Otherwise return the result of calcChange
**Analysis:**
- **TC: O(num_coins * amount)** - Solve one subproblem for range(amount) and each coin
- **SC: O(t)** - Store results for range(amount)
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        cache = {}

        def calcChange(cur_amount):
            if cur_amount == 0:
                return 0
            if cur_amount in cache:
                return cache[cur_amount]

            minNumCoins = amount + 1
            for coin in coins:
                if cur_amount - coin >= 0:
                    minNumCoins = 
	                    min(minNumCoins, 
	                    1 + calcChange(cur_amount - coin))
            cache[cur_amount] = minNumCoins 
            return minNumCoins
        
        minCoins = calcChange(amount)
        return -1 if minCoins >= amount + 1 else minCoins
```
## References
