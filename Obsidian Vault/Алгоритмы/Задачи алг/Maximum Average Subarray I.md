> [!NOTE]
> You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
> 
> Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return _this value_. Any answer with a calculation error less than `10-5` will be accepted.
> 
> **Example 1:**
> 
> **Input:** nums = [1,12,-5,-6,50,3], k = 4
> **Output:** 12.75000
> **Explanation:** Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
> 
> **Example 2:**
> 
> **Input:** nums = [5], k = 1
> **Output:** 5.00000

### **Краткая идея решения:**

1. **Sliding Window (скользящее окно):**
    
    - Сначала вычисляется сумма первых `k` элементов (`current_sum`).
        
    - Затем окно сдвигается на один элемент вправо: из суммы вычитается уходящий элемент (`nums[i]`) и прибавляется новый (`nums[i + k]`).
        
    - На каждом шаге проверяется, не стала ли текущая сумма (`current_sum`) больше максимальной (`max_sum`).
        
    - В конце возвращается среднее значение для подмассива с максимальной суммой (`max_sum / k`).
        
```python
class Solution(object):
    def findMaxAverage(self, nums, k):
        current_sum = sum(nums[:k])
        max_sum = current_sum
        
        for i in range(len(nums) - k):
            current_sum = current_sum - nums[i] + nums[i + k]
            if current_sum > max_sum:
                max_sum = current_sum
        
        return float(max_sum) / k
```
### **Сложность:**

- **Время:** **O(n)** — проходим по массиву один раз.
    
- **Память:** **O(1)** — используются только константные переменные (`current_sum`, `max_sum`).