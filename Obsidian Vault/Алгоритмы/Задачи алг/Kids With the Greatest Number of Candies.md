> [!NOTE]
> There are `n` kids with candies. You are given an integer array `candies`, where each `candies[i]`represents the number of candies the `ith` kid has, and an integer `extraCandies`, denoting the number of extra candies that you have.
> 
> Return _a boolean array_ `result` _of length_ `n`_, where_ `result[i]` _is_ `true` _if, after giving the_ `ith` _kid all the_ `extraCandies`_, they will have the **greatest** number of candies among all the kids__, or_ `false` _otherwise_.
> 
> Note that **multiple** kids can have the **greatest** number of candies.
> 
> **Example 1:**
> 
> **Input:** candies = [2,3,5,1,3], extraCandies = 3
> **Output:** [true,true,true,false,true] 
> **Explanation:** If you give all extraCandies to:
> - Kid 1, they will have 2 + 3 = 5 candies, which is the greatest among the kids.
> - Kid 2, they will have 3 + 3 = 6 candies, which is the greatest among the kids.
> - Kid 3, they will have 5 + 3 = 8 candies, which is the greatest among the kids.
> - Kid 4, they will have 1 + 3 = 4 candies, which is not the greatest among the kids.
> - Kid 5, they will have 3 + 3 = 6 candies, which is the greatest among the kids.
> 
> **Example 2:**
> 
> **Input:** candies = [4,2,1,1,2], extraCandies = 1
> **Output:** [true,false,false,false,false] 
> **Explanation:** There is only 1 extra candy.
> Kid 1 will always have the greatest number of candies, even if a different kid is given the extra candy.
> 
> **Example 3:**
> 
> **Input:** candies = [12,1,12], extraCandies = 10
> **Output:** [true,false,true]

### 💡 **Идея решения**:

1. Сначала находим максимальное количество конфет среди всех детей:  
    `max_cand = max(candies)`
    
2. Затем для каждого ребёнка проверяем:  
    если дать ему **все** `extraCandies`, будет ли у него **не меньше**, чем `max_cand`.
    
3. Добавляем результат (`True` или `False`) в выходной список.
    

Пример:  
Если у ребёнка 2 конфеты, и `extraCandies = 3`,  
а максимум среди всех — 5,  
то `2 + 3 = 5 >= 5 → True`.

---
```python
class Solution(object):

	def kidsWithCandies(self, candies, extraCandies):
	
		max_cand = max(candies)
		
		res = []
		
		for cand in candies:
		
			res.append(cand+extraCandies>=max_cand)
		
		return res
```
### ⏱ **Сложность**:

- **Время**:
    
    - `O(n)` — один проход, чтобы найти максимум
        
    - `O(n)` — второй проход, чтобы проверить каждого ребёнка  
        ➤ **Итого: `O(n)`**
        
- **Память**:
    
    - `O(n)` — для хранения результата (список из `n` булевых значений)