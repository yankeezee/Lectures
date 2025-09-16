> [!NOTE]
> Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.
> 
> **Example 1:**
> 
> **Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
> **Output:** 6
> **Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
> 
> **Example 2:**
> 
> **Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
> **Output:** 10
> **Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
> 
> **Constraints:**
> 
> - `1 <= nums.length <= 105`
> - `nums[i]` is either `0` or `1`.
> - `0 <= k <= nums.length`

### **Идея решения:**

Используем **скользящее окно (two pointers)** для нахождения максимального подмассива, содержащего не более `k` нулей.

- **Левый указатель (`left`)** — начало окна.
    
- **Правый указатель (`right`)** — конец окна, движется вправо.
    
- **Счётчик нулей (`zero_count`)** — сколько нулей в текущем окне.
    

**Алгоритм:**

1. Двигаем `right` вправо:
    
    - Если встретили `0`, увеличиваем `zero_count`.
        
2. Если `zero_count > k`, двигаем `left` вправо, пока `zero_count` не станет ≤ `k`:
    
    - Если `nums[left] == 0`, уменьшаем `zero_count`.
        
3. На каждом шаге обновляем максимальную длину (`max_length = max(max_length, right - left + 1)`).
    
```python
class Solution(object):

	def longestOnes(self, nums, k):
		left=0
		right = 0
		maxArr = 0
		countZ = 0
		
		for i in range(len(nums)):
			print(i)
			
			if nums[right]==0:
				countZ +=1
			
			if countZ > k:
				while countZ > k:
	
					if nums[left]==0:
						countZ -=1
					left +=1
				
			maxArr= max(maxArr, right-left+1)
			right +=1
		
		return maxArr
```
### **Сложность:**

- **Время:** **O(n)** — проход по массиву один раз.
    
- **Память:** **O(1)** — константные переменные.