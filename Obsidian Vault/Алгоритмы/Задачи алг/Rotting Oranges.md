> [!NOTE]
> You are given an `m x n` `grid` where each cell can have one of three values:
> 
> - `0` representing an empty cell,
> - `1` representing a fresh orange, or
> - `2` representing a rotten orange.
> 
> Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.
> 
> Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.
> 
> **Example 1:**
> 
> ![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)
> 
> **Input:** grid = \[\[2,1,1\],\[1,1,0],\[0,1,1]\]
> **Output:** 4
> 
> **Example 2:**
> 
> **Input:** grid = \[\[2,1,1],\[0,1,1],\[1,0,1]]
> **Output:** -1
> **Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
> 
> **Example 3:**
> 
> **Input:** grid = \[\[\0,2]]
> **Output:** 0
> **Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.
## Подход (BFS - поиск в ширину)

1. **Инициализация**:
    
    - Собираем все изначально гнилые апельсины (значение 2) в очередь
        
    - Заводим счетчик свежих апельсинов (значение 1)
        
2. **Многоуровневый BFS**:
    
    - Обрабатываем все текущие гнилые апельсины (текущий уровень)
        
    - Заражаем соседние свежие апельсины (4 направления)
        
    - Добавляем новые зараженные апельсины в очередь
        
    - Увеличиваем счетчик минут после обработки каждого уровня
        
3. **Проверка результата**:
    
    - Если остались свежие апельсины → возвращаем -1
        
    - Иначе возвращаем количество минут
```python
from collections import deque

def orangesRotting(grid):
    if not grid:
        return -1
    
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    minutes = 0
    
    # Собираем все гнилые апельсины и считаем свежие
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh += 1
    
    # Если нет свежих апельсинов, сразу возвращаем 0
    if fresh == 0:
        return 0
    
    # Направления для 4-связных соседей
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue and fresh > 0:
        # Обрабатываем текущий уровень (все гнилые на текущей минуте)
        for _ in range(len(queue)):
            r, c = queue.popleft()
            
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                # Проверяем границы и что апельсин свежий
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2  # портим апельсин
                    fresh -= 1
                    queue.append((nr, nc))
        
        if queue:  # Если были новые зараженные апельсины
            minutes += 1
    
    return minutes if fresh == 0 else -1
```

## Сложность

- **Время**: O(m×n) - каждый апельсин обрабатывается один раз
    
- **Память**: O(m×n) - в худшем случае очередь содержит все апельсины
    

### **Как расшифровывается BFS?**  
**BFS** — это **Breadth-First Search** (поиск в ширину).  

Это алгоритм обхода графа (или сетки, как в данной задаче), который:  
1. Обрабатывает все соседние вершины (клетки) **текущего уровня** перед переходом на следующий уровень.  
2. Гарантирует, что мы находим **кратчайший путь** или минимальное время заражения (как в задаче с апельсинами).  

### **Почему именно `deque`?**  
`deque` (двусторонняя очередь) из модуля `collections` используется потому, что:  

1. **Эффективность операций**:
   - `popleft()` — **O(1)** (быстрее, чем `list.pop(0)`, который работает за **O(n)**).  
   - `append()` — **O(1)** (так же быстро, как у `list`).  

2. **BFS требует FIFO (First-In-First-Out)**:
   - Мы обрабатываем элементы **в порядке их добавления** (первый сгнивший апельсин заражает соседей, затем очередь переходит к следующим).  

3. **Удобство обработки уровней**:
   - В коде используется `for _ in range(len(queue))`, чтобы обработать **ровно один уровень** за раз (одну минуту).  
   - `deque` позволяет эффективно удалять элементы слева (`popleft()`) и добавлять справа (`append()`).  

### **Альтернативы `deque`?**  
Можно использовать обычный `list`, но:  
- `queue.pop(0)` будет **медленным** (O(n) вместо O(1)).  
- Для больших сеток это **сильно замедлит** алгоритм.  

### **Вывод**  
`deque` — оптимальная структура данных для BFS, потому что:  
✅ Быстро удаляет элементы из начала (`popleft()`).  
✅ Позволяет эффективно обрабатывать уровни.  
✅ Гарантирует **O(m×n)** время работы.  

Если бы мы использовали `list`, сложность стала бы **O(m×n × n)** (из-за `pop(0)`), что недопустимо для больших данных.