> [!NOTE]
> Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return _the reordered list_.
> 
> The **first** node is considered **odd**, and the **second** node is **even**, and so on.
> 
> Note that the relative order inside both the even and odd groups should remain as it was in the input.
> 
> You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.
> 
> **Example 1:**
> 
> ![](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)
> 
> **Input:** head = [1,2,3,4,5]
> **Output:** [1,3,5,2,4]
> 
> **Example 2:**
> 
> ![](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)
> 
> **Input:** head = [2,1,3,5,6,4,7]
> **Output:** [2,3,6,7,1,5,4]
> 
> **Constraints:**
> 
> - The number of nodes in the linked list is in the range `[0, 104]`.
> - `-106 <= Node.val <= 106`

### **Идея решения задачи**

Задача **Odd Even Linked List** требует перегруппировки связного списка так, чтобы все узлы с **нечетными** индексами шли первыми, а с **четными** — после них. При этом **относительный порядок** внутри каждой группы должен сохраняться.

#### **Ключевые шаги алгоритма:**

1. **Разделение списка на два подсписка**:
    
    - `odd` — для узлов с нечетными позициями (1, 3, 5, ...).
        
    - `even` — для узлов с четными позициями (2, 4, 6, ...).
        
    - `even_head` — сохраняет начало четного списка, чтобы потом прицепить его к концу нечетного.
        
2. **Проход по списку и перераспределение указателей**:
    
    - На каждой итерации `odd.next` перепрыгивает через один узел (ведет к следующему нечетному).
        
    - Аналогично `even.next` переходит к следующему четному узлу.
        
    - Узлы `odd` и `even` перемещаются вперед.
        
3. **Объединение списков**:
    
    - После завершения цикла `odd.next` привязывается к `even_head`.
        

### **Пример работы**

**Исходный список:**  
`1 → 2 → 3 → 4 → 5 → None`

**После обработки:**

1. Нечетные узлы: `1 → 3 → 5`
    
2. Четные узлы: `2 → 4`
    
3. Результат: `1 → 3 → 5 → 2 → 4 → None`
    

---
```python
class Solution(object):
    def oddEvenList(self, head):
        odd = head  # Указатель на нечетные узлы
        if head and head.next:
            even_head = even = head.next  # Сохраняем начало четного списка
        else:
            return head  # Если список пуст или из одного элемента

        while even and even.next:
            odd.next = odd.next.next  # Перепрыгиваем через узел (нечетный)
            even.next = even.next.next  # Аналогично для четного
            
            odd = odd.next  # Перемещаем указатели вперед
            even = even.next
        
        odd.next = even_head  # Объединяем списки
        return head
```
### **Сложность алгоритма**

- **Время:** **O(n)** — проход по списку выполняется за один линейный проход.
    
- **Память:** **O(1)** — используются только несколько дополнительных указателей, константная память.
