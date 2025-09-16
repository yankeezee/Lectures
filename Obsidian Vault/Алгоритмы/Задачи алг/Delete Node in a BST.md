> [!NOTE]
> Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return _the **root node reference** (possibly updated) of the BST_.
> 
> Basically, the deletion can be divided into two stages:
> 
> 1. Search for a node to remove.
> 2. If the node is found, delete the node.
> 
> **Example 1:**
> 
> ![](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)
> 
> **Input:** root = [5,3,6,2,4,null,7], key = 3
> **Output:** [5,4,6,2,null,null,7]
> **Explanation:** Given key to delete is 3. So we find the node with value 3 and delete it.
> One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
> Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.
> ![](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)
> 
> **Example 2:**
> 
> **Input:** root = [5,3,6,2,4,null,7], key = 0
> **Output:** [5,3,6,2,4,null,7]
> **Explanation:** The tree does not contain a node with value = 0.
> 
> **Example 3:**
> 
> **Input:** root = [], key = 0
> **Output:** []
> 
> **Constraints:**
> 
> - The number of nodes in the tree is in the range `[0, 104]`.
> - `-105 <= Node.val <= 105`
> - Each node has a **unique** value.
> - `root` is a valid binary search tree.
> - `-105 <= key <= 105`
> 
> **Follow up:** Could you solve it with time complexity `O(height of tree)`?

### Идея решения задачи удаления узла из BST

**Определение BST (Binary Search Tree)**: В бинарном дереве поиска для каждого узла:

- Все узлы левого поддерева содержат значения меньше значения текущего узла.
    
- Все узлы правого поддерева содержат значения больше значения текущего узла.
    
- Левые и правые поддеревья также являются BST.
    

**Цель задачи**: Удалить узел с заданным ключом `key` из BST, сохранив свойства BST.

#### Ключевые моменты:

1. **Поиск узла для удаления**:
    
    - Начинаем с корня и рекурсивно ищем узел с ключом `key`.
        
    - Если `key` меньше значения текущего узла, идем в левое поддерево.
        
    - Если `key` больше значения текущего узла, идем в правое поддерево.
        
    - Если `key` равен значению текущего узла, это узел для удаления.
        
2. **Удаление узла**:
    
    - **Случай 1**: У узла нет детей (лист) → просто удаляем его (возвращаем `null`).
        
    - **Случай 2**: У узла только один ребенок → заменяем узел на его ребенка.
        
    - **Случай 3**: У узла два ребенка:
        
        - Находим **наименьший узел в правом поддереве** (или **наибольший в левом поддереве**).
            
        - Копируем значение этого узла в удаляемый узел.
            
        - Рекурсивно удаляем найденный узел (чтобы избежать дублирования).
            

#### Пример 1:

- **Вход**: `root = [5,3,6,2,4,null,7]`, `key = 3`
    
- **Поиск**: Находим узел `3` (у него два ребенка: `2` и `4`).
    
- **Удаление**:
    
    - Находим наименьший узел в правом поддереве (`4`).
        
    - Копируем `4` в узел `3`.
        
    - Удаляем узел `4` (теперь он лист).
        
- **Вывод**: `[5,4,6,2,null,null,7]` (или `[5,2,6,null,4,null,7]`).
    

#### Пример 2:

- **Вход**: `root = [5,3,6,2,4,null,7]`, `key = 0`
    
- **Поиск**: Узел `0` не найден → возвращаем исходное дерево.
    
- **Вывод**: `[5,3,6,2,4,null,7]`.
    

#### Пример 3:

- **Вход**: `root = []`, `key = 0`
    
- **Поиск**: Дерево пустое → возвращаем `[]`.
    
- **Вывод**: `[]`.

#### Код

```python
def deleteNode(root, key):
    if not root:
        return None
    
    if key < root.val:
        root.left = deleteNode(root.left, key)
    elif key > root.val:
        root.right = deleteNode(root.right, key)
    else:
        # Случай 1: Нет детей или один ребенок
        if not root.left:
            return root.right
        elif not root.right:
            return root.left
        # Случай 3: Два ребенка
        else:
            # Находим наименьший узел в правом поддереве
            temp = root.right
            while temp.left:
                temp = temp.left
            # Копируем значение
            root.val = temp.val
            # Удаляем найденный узел
            root.right = deleteNode(root.right, temp.val)
    return root
```
#### Сложность:

- **Время**:
    
    - В среднем случае (сбалансированное дерево) — O(log n).
        
    - В худшем случае (несбалансированное дерево) — O(n).
        
- **Память**:
    
    - Рекурсивный подход использует O(h) памяти (h — высота дерева).