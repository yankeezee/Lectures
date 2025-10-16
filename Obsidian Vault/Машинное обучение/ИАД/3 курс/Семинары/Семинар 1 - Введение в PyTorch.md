
## 1. Базовые операции NumPy vs. PyTorch

-   **Сумма квадратов:** Продемонстрирована эквивалентность операций между NumPy и PyTorch для суммирования квадратов чисел.
    ```python
    # NumPy
    np.square(np.arange(1, 10001)).sum() # -> 333383335000
    # PyTorch
    torch.square(torch.arange(1, 10001)).sum() # -> tensor(333383335000)
    ```

## 2. Реализация сигмоиды

-   Сигмоида ($\sigma(x) = \frac{1}{1 + e^{-x}}$) реализована с использованием `torch.exp()`.
    ```python
    def sigm(x):
        return 1/(1 + torch.exp(-x))
    ```

## 3. Реализация средней квадратичной ошибки (MSE)

-   MSE ($MSE(\hat y, y) = \frac{1}{n} \cdot \sum_{i=1}^n (\hat y - y)^2$) реализована с использованием `torch.mean()` и поэлементных операций.
    ```python
    def mse(y_true, y_pred):
        return torch.mean((y_true - y_pred)**2)
    ```

## 4. Копирование тензоров (`clone()`)

-   **Проблема:** Присваивание `y = x` создает *ссылку* на тот же тензор, а не копию. Изменение `y` также изменяет `x`.
    ```python
    x = torch.tensor([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
    y = x # y ссылается на x
    y[2] = torch.ones(3)
    # print(x) покажет измененный x
    ```
-   **Решение:** Использовать метод `.clone()` для создания независимой копии.
    ```python
    x = torch.tensor([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
    y = x.clone() # y теперь независимая копия x
    y[2] = torch.ones(3)
    # print(x) покажет оригинальный x
    ```

## 5. Расчет градиента (`requires_grad=True`, `.backward()`)

-   Для автоматического расчета градиентов необходимо установить `requires_grad=True` для тензора, по которому нужно считать градиент.
-   Вызов `.backward()` на скалярном результате вычислительной цепочки заполняет атрибут `.grad` у всех тензоров, для которых `requires_grad=True`.
    ```python
    w =  torch.tensor([[5.,10.],[1.,2.]], requires_grad=True)
    function =  torch.prod(torch.log(torch.log(w + 7)))
    function.backward()
    print(w.grad) # Выведет тензор с градиентами
    ```

## 6. Градиентный спуск вручную

-   Минимизация функции с помощью градиентного спуска:
    1.  Инициализация `w` с `requires_grad=True`.
    2.  Цикл:
        *   Прямой проход (`function = ...`).
        *   Обратный проход (`function.backward()`).
        *   Обновление весов: `w.data -= alpha * w.grad` (используем `.data` для обновления без отслеживания градиентов).
        *   Обнуление градиентов: `w.grad.zero_()` (важно для следующей итерации).
    ```python
    w = torch.tensor([[5., 10.], [1., 2.]], requires_grad=True)
    alpha = 0.001
    for _ in range(500):
        function = (w + 7).log().log().prod()
        function.backward()
        with torch.no_grad(): # Альтернатива w.data -= ...
             w -= alpha * w.grad
        w.grad.zero_()
    print(w) # Оптимизированное значение w
    ```

## 7. DataLoader

-   `torch.utils.data.DataLoader` используется для эффективной загрузки данных батчами.
-   **`batch_size`**: количество объектов в одном батче.
-   **`shuffle=True`**: перемешивает данные каждую эпоху (для тренировочной выборки).
-   **`shuffle=False`**: не перемешивает данные (для тестовой/валидационной выборки).
    ```python
    # Предполагается наличие train_set и test_set (например, Dataset объекты)
    train_loader = torch.utils.data.DataLoader(train_set, batch_size=64, shuffle=True)
    test_loader = torch.utils.data.DataLoader(test_set, batch_size=64, shuffle=False)
    
    # Итерация по DataLoader
    for images, labels in train_loader:
        print(images.shape, labels.shape) # Пример: torch.Size([64, 1, 28, 28]) torch.Size([64])
        break
    ```

## 8. Двухслойная полносвязная нейросеть

-   Определяется класс модели, наследующий `nn.Module`.
-   Слои определяются в `__init__`.
-   Прямой проход (`forward`) описывает последовательность операций.
-   `nn.Sequential` удобен для последовательного применения слоев.
-   `nn.Linear`: полносвязный слой.
-   `nn.ReLU`, `nn.LeakyReLU`: функции активации.
    ```python
    import torch.nn as nn # Добавить в импорты
    
    class NeuralNet(nn.Module):
        def __init__(self, in_features, num_classes, hidden_size):
            super().__init__()
            self.model = nn.Sequential(
                nn.Linear(in_features=in_features, out_features=hidden_size),
                nn.ReLU(),
                nn.Linear(in_features=hidden_size, out_features=hidden_size, bias=False), # Пример без bias
                nn.LeakyReLU(0.1),
                nn.Linear(in_features=hidden_size, out_features=num_classes), # Выходной слой на 10 нейронов
            )
    
        def forward(self, x):
            return self.model(x)
    ```

---