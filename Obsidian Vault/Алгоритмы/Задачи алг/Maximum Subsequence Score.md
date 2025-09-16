> [!NOTE]
> You are given two **0-indexed** integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a **subsequence** of indices from `nums1` of length `k`.
> 
> For chosen indices `i0`, `i1`, ..., `ik - 1`, your **score** is defined as:
> 
> - The sum of the selected elements from `nums1` multiplied with the **minimum** of the selected elements from `nums2`.
> - It can defined simply as: `(nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1])`.
> 
> Return _the **maximum** possible score._
> 
> A **subsequence** of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.
> 
> **Example 1:**
> 
> **Input:** nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
> **Output:** 12
> **Explanation:** 
> The four possible subsequence scores are:
> - We choose the indices 0, 1, and 2 with score = (1+3+3) * min(2,1,3) = 7.
> - We choose the indices 0, 1, and 3 with score = (1+3+2) * min(2,1,4) = 6. 
> - We choose the indices 0, 2, and 3 with score = (1+3+2) * min(2,3,4) = 12. 
> - We choose the indices 1, 2, and 3 with score = (3+3+2) * min(1,3,4) = 8.
> Therefore, we return the max score, which is 12.
> 
> **Example 2:**
> 
> **Input:** nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
> **Output:** 30
> **Explanation:** 
> Choosing index 2 is optimal: nums1[2] * nums2[2] = 3 * 10 = 30 is the maximum possible score.
> 
> **Constraints:**
> 
> - `n == nums1.length == nums2.length`
> - `1 <= n <= 105`
> - `0 <= nums1[i], nums2[j] <= 105`
> - `1 <= k <= n`

### **Идея решения 

**Условие задачи:**  
Даны два массива `nums1` и `nums2` одинаковой длины `n`. Нужно выбрать **подпоследовательность** из `k` индексов (порядок не важен). **Счёт** вычисляется как:  
1. **Сумма** выбранных элементов из `nums1`.  
2. **Минимум** среди выбранных элементов из `nums2`.  

Итоговый счёт = `(сумма nums1) * (минимум nums2)`.  
**Цель:** Найти максимально возможный счёт.  

---

### **Ключевая идея решения**  
1. **Связь между `nums1` и `nums2`:**  
   - Нам нужно выбрать `k` элементов так, чтобы их сумма в `nums1` была **максимально возможной**, а минимум в `nums2` — **как можно больше**.  
   - Если мы фиксируем `min(nums2) = x`, то нужно выбрать `k` элементов с `nums2[i] >= x` и с максимальной суммой в `nums1`.  

2. **Оптимальная стратегия:**  
   - **Отсортируем пары `(nums2[i], nums1[i])` в порядке убывания `nums2[i]`.**  
   - Теперь для каждого возможного `x = nums2[i]` (где `i` — один из первых `k` элементов после сортировки):  
     - Выберем `k` элементов с **наибольшими `nums1[j]`**, у которых `nums2[j] >= x`.  
     - Это гарантирует, что `x` будет минимумом, а сумма `nums1` — максимальной.  

3. **Эффективный подсчёт:**  
   - Используем **мини-кучу (min-heap)** для хранения `k` наибольших `nums1[j]` среди элементов, где `nums2[j] >= x`.  
   - Это позволит быстро пересчитывать сумму при переходе к следующему `x`.  

---

### **Пошаговый алгоритм**
1. **Сортируем пары `(nums2[i], nums1[i])` в порядке убывания `nums2[i]`.**  
   - Это поможет перебирать кандидатов на минимум от большего к меньшему.  

2. **Используем кучу для хранения `k` наибольших `nums1`:**  
   - Идём по отсортированным парам и поддерживаем кучу из `k` самых больших `nums1`.  
   - Если куча уже содержит `k` элементов, то:  
     - Если новый `nums1[i]` больше минимального в куче, заменяем его.  
     - Иначе пропускаем.  

3. **Вычисляем текущий счёт и обновляем максимум:**  
   - Для каждого `x = nums2[i]` (текущий минимум):  
     - Сумма = сумма элементов в куче.  
     - Текущий счёт = `сумма * x`.  
     - Запоминаем максимум.  

---

### **Почему это работает?**
- Мы перебираем все возможные кандидаты на минимум (`nums2[i]`).  
- Для каждого такого кандидата берём `k` элементов с `nums2[j] >= nums2[i]` и максимальной суммой `nums1`.  
- Так как массив отсортирован по убыванию `nums2`, мы гарантируем, что `nums2[i]` будет минимумом в выбранной группе.  
```python
import heapq

class Solution(object):
    def maxScore(self, nums1, nums2, k):
        # Сортируем пары по убыванию nums2, чтобы минимум был nums2[i]
        paired = sorted(zip(nums1, nums2), key=lambda x: -x[1])
        
        max_score = 0
        current_sum = 0
        min_heap = []  # храним k наибольших nums1
        
        for i in range(len(paired)):
            num1, num2 = paired[i]
            
            # Добавляем в кучу
            heapq.heappush(min_heap, num1)
            current_sum += num1
            
            # Если куча превысила размер k, удаляем наименьший nums1
            if len(min_heap) > k:
                removed = heapq.heappop(min_heap)
                current_sum -= removed
            
            # Если набрали ровно k элементов, обновляем счёт
            if len(min_heap) == k:
                current_min = num2  # т.к. массив отсортирован по убыванию nums2
                current_score = current_sum * current_min
                if current_score > max_score:
                    max_score = current_score
        
        return max_score
```
**Сложность:**  
- Сортировка: `O(n log n)`.  
- Проход с кучей: `O(n log k)`.  
- **Итого:** `O(n log n)` (доминирует сортировка).  

