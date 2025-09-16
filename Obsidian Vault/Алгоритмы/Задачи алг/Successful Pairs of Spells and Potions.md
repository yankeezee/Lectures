> [!NOTE]
> You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `ith` spell and `potions[j]` represents the strength of the `jth` potion.
> 
> You are also given an integer `success`. A spell and potion pair is considered **successful** if the **product** of their strengths is **at least** `success`.
> 
> Return _an integer array_ `pairs` _of length_ `n` _where_ `pairs[i]` _is the number of **potions** that will form a successful pair with the_ `ith` _spell._
> 
> **Example 1:**
> 
> **Input:** spells = [5,1,3], potions = [1,2,3,4,5], success = 7
> **Output:** [4,0,3]
> **Explanation:**
> - 0th spell: 5 * [1,2,3,4,5] = [5,**10**,**15**,**20**,**25**]. 4 pairs are successful.
> - 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
> - 2nd spell: 3 * [1,2,3,4,5] = [3,6,**9**,**12**,**15**]. 3 pairs are successful.
> Thus, [4,0,3] is returned.
> 
> **Example 2:**
> 
> **Input:** spells = [3,1,2], potions = [8,5,8], success = 16
> **Output:** [2,0,2]
> **Explanation:**
> - 0th spell: 3 * [8,5,8] = [**24**,15,**24**]. 2 pairs are successful.
> - 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful. 
> - 2nd spell: 2 * [8,5,8] = [**16**,10,**16**]. 2 pairs are successful. 
> Thus, [2,0,2] is returned.
> 
> **Constraints:**
> 
> - `n == spells.length`
> - `m == potions.length`
> - `1 <= n, m <= 105`
> - `1 <= spells[i], potions[i] <= 105`
> - `1 <= success <= 1010`

**Идея:**

1. Отсортировать массив `potions`.
    
2. Для каждого заклинания `spell` вычислить минимальное значение зелья `min_potion`, при котором произведение `spell * potion >= success`.
    
3. Найти количество элементов в `potions`, которые >= `min_potion`, используя **ручной бинарный поиск**.
```python
class Solution(object):

	def successfulPairs(self, spells, potions, success):
	
		pairs = [0]*len(spells)
		n_p = len(potions)
		sort = sorted(potions)
		
		for i in range(len(spells)):
		
			left = 0
			right = n_p-1
			
			while left<right:
			
				mid = (left+right)//2
				
				if sort[mid]*spells[i]>=success:
					right = mid
				
				else:
					left=mid+1
			
			if sort[left]*spells[i]>=success:
				pairs[i] = n_p-left
			
			else:
				pairs[i]=0
		
		return pairs
```
