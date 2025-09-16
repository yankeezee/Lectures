> [!NOTE]
> Given a binary tree `root`, a node _X_ in the tree is named **good** if in the path from root to _X_ there are no nodes with a value _greater than_ X.
> 
> Return the number of **good** nodes in the binary tree.
> 
> **Example 1:**
> 
> **![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)**
> 
> **Input:** root = [3,1,4,3,null,1,5]
> **Output:** 4
> **Explanation:** Nodes in blue are **good**.
> Root Node (3) is always a good node.
> Node 4 -> (3,4) is the maximum value in the path starting from the root.
> Node 5 -> (3,4,5) is the maximum value in the path
> Node 3 -> (3,1,3) is the maximum value in the path.
> 
> **Example 2:**
> 
> **![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_2.png)**
> 
> **Input:** root = [3,3,null,4,2]
> **Output:** 3
> **Explanation:** Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.
> 
> **Example 3:**
> 
> **Input:** root = [1]
> **Output:** 1
> **Explanation:** Root is considered as **good**.
> 
> **Constraints:**
> 
> - The number of nodes in the binary tree is in the range `[1, 10^5]`.
> - Each node's value is between `[-10^4, 10^4]`.
### Идея решения

Для решения задачи нужно **обойти дерево** (например, рекурсивно) и для каждого узла проверять, является ли он "хорошим".

**Ключевая идея:**  
При обходе дерева нужно **передавать текущий максимум** на пути от корня до текущего узла.

- Если значение узла **>= текущего максимума**, то он "хороший", и нужно обновить максимум.
    
- Иначе — узел не "хороший", и максимум остаётся прежним.
    

### Алгоритм

1. **Начинаем с корня**, изначальный максимум = значению корня (так как корень всегда хороший).
    
2. **Рекурсивно обходим левое и правое поддеревья**, передавая текущий максимум.
    
3. **Если текущий узел >= максимума**, увеличиваем счётчик хороших узлов и обновляем максимум.
    
4. **Повторяем** для всех узлов.
```python
def goodNodes(root):

    def dfs(node, current_max):
        if not node:
            return 0
        count = 0
        if node.val >= current_max:
            count += 1
            current_max = node.val
        count += dfs(node.left, current_max)
        count += dfs(node.right, current_max)
        return count
    
    return dfs(root, root.val)
```
### Сложность

- **Время:** `O(N)` — обходим каждый узел ровно один раз.
    
- **Память:** `O(H)` — где `H` высота дерева (затраты на рекурсивный стек).