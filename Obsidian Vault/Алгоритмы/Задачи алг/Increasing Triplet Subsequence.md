> [!NOTE]
> Given an integer array `nums`, return `true` _if there exists a triple of indices_ `(i, j, k)` _such that_ `i < j < k` _and_ `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.
> 
> **Example 1:**
> 
> **Input:** nums = [1,2,3,4,5]
> **Output:** true
> **Explanation:** Any triplet where i < j < k is valid.
> 
> **Example 2:**
> 
> **Input:** nums = [5,4,3,2,1]
> **Output:** false
> **Explanation:** No triplet exists.
> 
> **Example 3:**
> 
> **Input:** nums = [2,1,5,0,4,6]
> **Output:** true
> **Explanation:** The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
## Ключевые наблюдения

1. Нам не нужно находить все возможные тройки - достаточно обнаружить хотя бы одну подходящую.
    
2. Можно искать не конкретные индексы, а последовательность возрастающих значений.
    
3. Достаточно найти последовательность из трех чисел, где каждое следующее больше предыдущего.
    

## Основные подходы к решению

### 1. Наивное решение (перебор всех троек)

**Сложность: O(n³)**

- Проверяем все возможные комбинации i < j < k
    
- Простое, но неэффективное для больших массивов
    

### 2. Оптимальное решение (поиск последовательности)

**Сложность: O(n)**

- Будем искать последовательность из трех чисел, запоминая два наименьших найденных числа и одно число, которое больше их обоих
    

```python
def increasingTriplet(nums):
    first = second = float('inf')
    for num in nums:
        if num <= first:
            first = num
        elif num <= second:
            second = num
        else:
            return True
    return False
```
**Сложность алгоритма: O(n)** (линейная)  

- Алгоритм делает **один проход** по массиву  
- На каждой итерации выполняет **константное** число операций (сравнения и присваивания)  
- **Память: O(1)** (использует всего две переменные `first` и `second`)  

Это оптимальное решение для данной задачи.