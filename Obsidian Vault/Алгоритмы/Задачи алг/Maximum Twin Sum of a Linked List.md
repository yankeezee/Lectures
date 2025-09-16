> [!NOTE]
> In a linked list of size `n`, where `n` is **even**, the `ith` node (**0-indexed**) of the linked list is known as the **twin** of the `(n-1-i)th` node, if `0 <= i <= (n / 2) - 1`.
> 
> - For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.
> 
> The **twin sum** is defined as the sum of a node and its twin.
> 
> Given the `head` of a linked list with even length, return _the **maximum twin sum** of the linked list_.
> 
> **Example 1:**
> 
> ![](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)
> 
> **Input:** head = [5,4,2,1]
> **Output:** 6
> **Explanation:**
> Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
> There are no other nodes with twins in the linked list.
> Thus, the maximum twin sum of the linked list is 6. 
> 
> **Example 2:**
> 
> ![](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)
> 
> **Input:** head = [4,2,2,3]
> **Output:** 7
> **Explanation:**
> The nodes with twins present in this linked list are:
> - Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
> - Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
> Thus, the maximum twin sum of the linked list is max(7, 4) = 7. 
> 
> **Example 3:**
> 
> ![](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)
> 
> **Input:** head = [1,100000]
> **Output:** 100001
> **Explanation:**
> There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.

### **Идея решения задачи о максимальной сумме близнецов в связном списке**

**Условие задачи:**  
Дан связный список чётной длины `n`.

- **Близнец** узла с индексом `i` — это узел с индексом `n-1-i` (для `0 <= i <= n/2 - 1`).
    
- **Сумма близнецов** — это сумма значения узла и его близнеца.
    
- Нужно найти **максимальную сумму близнецов** в списке.
    

---

### **Ключевые шаги решения**

#### **1. Найти середину списка**

Так как список имеет чётную длину, его можно разбить на две половины:

- Первая половина: индексы `0` до `n/2 - 1`.
    
- Вторая половина: индексы `n/2` до `n-1`.
    
#### **2. Развернуть вторую половину списка**

Чтобы эффективно находить близнецов, можно:

1. Разделить список на две половины.
    
2. **Развернуть вторую половину** (тогда порядок узлов совпадёт с порядком первой половины).
    
#### **3. Пройти по двум половинам и вычислить суммы**

После разворота второй половины:

- Идём одновременно по первой и развёрнутой второй половине.
    
- Считаем сумму текущих узлов и обновляем максимум.
    
#### **4. Вернуть максимальную сумму**

```python

def maxTwinSum(head):
    # Находим середину списка (черепаха и заяц)
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Разворачиваем вторую половину
    prev = None
    while slow:
        next_node = slow.next
        slow.next = prev
        prev = slow
        slow = next_node
    
    # Теперь prev - голова развёрнутой второй половины
    max_sum = 0
    first_half = head
    second_half = prev
    
    # Считаем суммы близнецов
    while second_half:
        current_sum = first_half.val + second_half.val
        max_sum = max(max_sum, current_sum)
        first_half = first_half.next
        second_half = second_half.next
    
    return max_sum
```
**Сложность:**

- Время: **O(n)** (проход для нахождения середины + разворот + проход для сумм).
    
- Память: **O(1)** (разворот делается на месте).
### 1. Метод "зайца и черепахи" (быстрый и медленный указатели)

**Цель:** Найти середину связного списка за один проход.
```python
slow = fast = head  # Оба начинают с головы списка
while fast and fast.next:  # Пока fast может сделать 2 шага
    slow = slow.next       # Черепаха: 1 шаг
    fast = fast.next.next  # Заяц: 2 шага
```
**Как это работает на примере [4,2,2,3]:**

Шаги:

1. Начало: slow=4, fast=4
    
2. После 1 итерации:
    
    - slow переходит на 2 (следующий после 4)
        
    - fast переходит на 2 (через один от 4)
        
3. После 2 итерации:
    
    - slow переходит на 2 (следующий)
        
    - fast переходит на None (через один от 2 - конец списка)
        
4. Цикл завершается, slow останавливается на втором элементе 2
    

**Итог:** slow остановился на начале второй половины списка.
### 2. Разворот второй половины списка

**Цель:** Развернуть вторую половину, чтобы можно было идти с конца.
```python
prev = None
while slow:  # Пока есть текущий узел
    next_node = slow.next  # Сохраняем следующий узел
    slow.next = prev       # Разворачиваем указатель
    prev = slow           # prev становится текущим
    slow = next_node      # Переходим к следующему
```
**Как это работает на второй половине [2,3]:**

Шаги:

1. Начало: slow=2, prev=None
    
2. Первая итерация:
    
    - next_node = 3 (сохранили следующий)
        
    - slow.next = None (2 теперь указывает на None)
        
    - prev = 2
        
    - slow = 3
        
3. Вторая итерация:
    
    - next_node = None (конец списка)
        
    - slow.next = 2 (3 теперь указывает на 2)
        
    - prev = 3
        
    - slow = None
        
4. Цикл завершается
    

**Итог:** prev указывает на голову развёрнутого списка [3,2]