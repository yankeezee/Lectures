> [!NOTE]
> Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.
> 
> Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.
> 
> Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.
> 
> Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.
> 
> **Example 1:**
> 
> **Input:** piles = [3,6,7,11], h = 8
> **Output:** 4
> 
> **Example 2:**
> 
> **Input:** piles = [30,11,23,4,20], h = 5
> **Output:** 30
> 
> **Example 3:**
> 
> **Input:** piles = [30,11,23,4,20], h = 6
> **Output:** 23
> 
> **Constraints:**
> 
> - `1 <= piles.length <= 104`
> - `piles.length <= h <= 109`
> - `1 <= piles[i] <= 109`

### **Идея решения**
1. **Проблема**: Нужно найти минимальную скорость `k` (бананов/час), чтобы Коко съела все кучи `piles` за `≤ h` часов.
2. **Ключевая идея**: 
   - Используем **бинарный поиск** по возможным значениям `k` (от `1` до `max(piles)`).
   - Для каждого `k` (середина диапазона `mid`) проверяем, успеет ли Коко съесть все бананы.
   - Если не успевает (`total_hours > h`) — увеличиваем скорость (`left = mid + 1`).
   - Если успевает (`total_hours ≤ h`) — пробуем меньшую скорость (`right = mid - 1`).
```python
```python
import math
class Solution(object):
        
    def minEatingSpeed(self, piles, h):
        """
        :type piles: List[int]
        :type h: int
        :rtype: int
        """

        left = 1
        right = max(piles)        
        while left <= right:
            total_hours = 0
            mid = (right + left) // 2
            for pile in piles:
                #total_hours += math.ceil(pile / float(mid))
                total_hours += (pile + mid-1) // mid
            if total_hours > h:
                left = mid + 1
            else:
                right = mid - 1

        return left
```
### **Сложность алгоритма**
- **Время**: **O(n log m)**, где:
  - `n` — количество куч (`piles`),
  - `m` — максимальное число бананов в куче (`max(piles)`).
- **Пространство**: **O(1)** (константная память, не зависит от входных данных).

### **Пошаговое объяснение кода**
1. **Инициализация границ**:
   - `left = 1` — минимальная возможная скорость (1 банан/час).
   - `right = max(piles)` — максимальная скорость (можно съесть самую большую кучу за 1 час).

2. **Бинарный поиск** (`while left <= right`):
   - `mid = (left + right) // 2` — текущая проверяемая скорость.
   - **Подсчёт часов** (`total_hours`):
     - Для каждой кучи `pile` вычисляем время как `(pile + mid - 1) // mid` (округление вверх без дробей).
   - **Проверка условия**:
     - Если `total_hours > h` → скорость `mid` слишком мала → ищем в правой половине (`left = mid + 1`).
     - Если `total_hours ≤ h` → скорость `mid` допустима → пробуем меньшие значения (`right = mid - 1`).

3. **Результат**:
   - После завершения цикла `left` указывает на минимальную подходящую скорость.

### **Пример работы**
Для `piles = [3, 6, 7, 11]`, `h = 8`:
- `left = 1`, `right = 11` → `mid = 6` → `total_hours = 1 + 1 + 2 + 2 = 6 ≤ 8` → пробуем меньшую скорость.
- `left = 1`, `right = 5` → `mid = 3` → `total_hours = 1 + 2 + 3 + 4 = 10 > 8` → увеличиваем скорость.
- `left = 4`, `right = 5` → `mid = 4` → `total_hours = 1 + 2 + 2 + 3 = 8 ≤ 8` → сохраняем `k = 4`.
- **Итог**: `left = 4` (правильный ответ).

### **Почему работает?**
- Бинарный поиск гарантирует нахождение минимального `k` за логарифмическое время.
- Формула `(pile + mid - 1) // mid` точно вычисляет часы без погрешностей дробных чисел.