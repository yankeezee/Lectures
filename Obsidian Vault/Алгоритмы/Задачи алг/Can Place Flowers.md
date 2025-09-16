> [!NOTE]
> You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.
> 
> Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` _if_ `n` _new flowers can be planted in the_ `flowerbed`_without violating the no-adjacent-flowers rule and_ `false` _otherwise_.
> 
> **Example 1:**
> 
> **Input:** flowerbed = [1,0,0,0,1], n = 1
> **Output:** true
> 
> **Example 2:**
> 
> **Input:** flowerbed = [1,0,0,0,1], n = 2
> **Output:** false
> 
> **Constraints:**
> 
> - `1 <= flowerbed.length <= 2 * 104`
> - `flowerbed[i]` is `0` or `1`.
> - There are no two adjacent flowers in `flowerbed`.
> - `0 <= n <= flowerbed.length`
### Идея решения задачи

Задача заключается в том, чтобы определить, можно ли посадить `n` новых цветков в клумбу (`flowerbed`), соблюдая правило: цветки не могут быть посажены на соседних участках.

#### Ключевые моменты:

1. **Правило соседства**: Если на участке `i` уже есть цветок (т.е. `flowerbed[i] = 1`), то участки `i-1` и `i+1` должны быть пустыми (`0`).
    
2. **Проверка возможности посадки**: Для каждого пустого участка (`0`) нужно проверить, что его соседи тоже пусты (или участок находится на границе клумбы).
    
3. **Жадный алгоритм**: Мы можем проходить по клумбе и сажать цветки везде, где это возможно, сразу помечая участки как занятые. Это оптимально, потому что посадка цветка раньше освобождает больше места для последующих.

```python
def canPlaceFlowers(flowerbed, n):
    count = 0
    length = len(flowerbed)
    for i in range(length):
        if flowerbed[i] == 0:
            left_empty = (i == 0) or (flowerbed[i-1] == 0)
            right_empty = (i == length - 1) or (flowerbed[i+1] == 0)
            if left_empty and right_empty:
                flowerbed[i] = 1
                count += 1
                if count >= n:
                    return True
    return count >= n
```
#### Сложность:

- Время: O(n), где n — длина `flowerbed` (проход один раз).
    
- Память: O(1) (используем константную память).