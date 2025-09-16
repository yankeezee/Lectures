> [!NOTE]
> You are given a **0-indexed** integer array `costs` where `costs[i]` is the cost of hiring the `ith` worker.
> 
> You are also given two integers `k` and `candidates`. We want to hire exactly `k` workers according to the following rules:
> 
> - You will run `k` sessions and hire exactly one worker in each session.
> - In each hiring session, choose the worker with the lowest cost from either the first `candidates`workers or the last `candidates` workers. Break the tie by the smallest index.
>     - For example, if `costs = [3,2,7,7,1,2]` and `candidates = 2`, then in the first hiring session, we will choose the `4th` worker because they have the lowest cost `[3,2,7,7,**1**,2]`.
>     - In the second hiring session, we will choose `1st` worker because they have the same lowest cost as `4th` worker but they have the smallest index `[3,**2**,7,7,2]`. Please note that the indexing may be changed in the process.
> - If there are fewer than candidates workers remaining, choose the worker with the lowest cost among them. Break the tie by the smallest index.
> - A worker can only be chosen once.
> 
> Return _the total cost to hire exactly_ `k` _workers._

### **Решение:**

1. **Используем две мини-кучи (min-heap):**
    
    - Одна для первых `candidates` элементов (`left_heap`).
        
    - Другая для последних `candidates` элементов (`right_heap`).
        
2. **На каждом шаге:**
    
    - Берем минимум из двух куч.
        
    - Если взяли из `left_heap`, добавляем следующий элемент слева.
        
    - Если взяли из `right_heap`, добавляем следующий элемент справа.
        
3. **Повторяем `k` раз** и суммируем стоимость.
```python
import heapq

class Solution:
    def totalCost(self, costs, k, candidates):
        left_heap = []
        right_heap = []
        n = len(costs)
        left_ptr = 0
        right_ptr = n - 1
        total_cost = 0

        for _ in range(k):
            # Заполняем левую кучу, если есть кандидаты
            while left_ptr <= right_ptr and len(left_heap) < candidates:
                heapq.heappush(left_heap, costs[left_ptr])
                left_ptr += 1
            # Заполняем правую кучу, если есть кандидаты
            while left_ptr <= right_ptr and len(right_heap) < candidates:
                heapq.heappush(right_heap, costs[right_ptr])
                right_ptr -= 1

            # Выбираем минимальный элемент из двух куч
            if left_heap and right_heap:
                if left_heap[0] <= right_heap[0]:
                    total_cost += heapq.heappop(left_heap)
                else:
                    total_cost += heapq.heappop(right_heap)
            elif left_heap:
                total_cost += heapq.heappop(left_heap)
            else:
                total_cost += heapq.heappop(right_heap)

        return total_cost
```
### **Сложность:**

- **Время:** `O(n + k log n)` (куча поддерживается за `O(log n)`).
    
- **Память:** `O(n)` (для хранения куч).

### **Разбор сложности `O(n + k log n)`**

#### **1. Разбивка алгоритма на шаги:**
1. **Инициализация куч (`left_heap` и `right_heap`):**  
   - Нужно добавить первые `candidates` элементов в `left_heap` и последние `candidates` в `right_heap`.  
   - В худшем случае (`candidates ≈ n/2`) это займет **`O(n)`** операций (так как `heapq.heappush` работает за `O(log m)` для кучи размера `m`, но вставка `n` элементов даст `O(n log n)`, но здесь мы вставляем только `2 * candidates` элементов, поэтому `O(n)`).  

2. **`k` итераций выбора работника:**  
   - На каждой итерации (`k` раз):  
     - **Извлечение минимума** из кучи (`heappop`) — `O(log candidates)`.  
     - **Добавление нового элемента** в кучу (`heappush`) — `O(log candidates)`.  
   - Так как `candidates` может быть до `O(n)`, то в худшем случае `O(log n)` на операцию.  
   - Итого: `O(k log n)`.  

3. **Итоговая сложность:**  
   - `O(n)` (инициализация) + `O(k log n)` (`k` операций с кучей).  
   - **`O(n + k log n)`** — доминирует больший из этих членов.  

---

#### **2. Почему не `O(n log n)`?**
- Если `k` мало (например, `k = 1`), то сложность `O(n)` (доминирует инициализация).  
- Если `k` велико (например, `k ≈ n`), то сложность `O(n log n)`.  
- В среднем (`k` и `candidates` небольшие) — `O(n + k log n)`.  

---

#### **3. Примеры для понимания:**
1️⃣ **Пример 1** (`n = 6`, `k = 2`, `candidates = 2`):  
   - Инициализация: `O(2 + 2) = O(4)` (добавление в две кучи).  
   - `k` итераций: `2 * O(log 2) = O(2)`.  
   - **Итого:** `O(4 + 2) = O(6)` (линейная сложность).  

2️⃣ **Пример 2** (`n = 1000`, `k = 100`, `candidates = 100`):  
   - Инициализация: `O(200)` (добавление 200 элементов в кучи).  
   - `k` итераций: `100 * O(log 100) ≈ O(100 * 6.6) = O(660)`.  
   - **Итого:** `O(200 + 660) = O(860)`.  

3️⃣ **Худший случай** (`k = n`, `candidates = n/2`):  
   - Инициализация: `O(n)`.  
   - `k` итераций: `n * O(log n)`.  
   - **Итого:** `O(n + n log n) = O(n log n)`.  

---

### **Вывод:**
- **`O(n)`** — подготовка куч (вставка `2 * candidates` элементов).  
- **`O(k log n)`** — `k` операций извлечения/добавления в кучу.  
- **Итог:** `O(n + k log n)`.  

Если `k` мало, доминирует `O(n)`, если `k` велико — `O(k log n)`.