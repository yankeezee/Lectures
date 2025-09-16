> [!NOTE]
> Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.
> 
> A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
> 
> - For example, `"ace"` is a subsequence of `"abcde"`.
> 
> A **common subsequence** of two strings is a subsequence that is common to both strings.
> 
> **Example 1:**
> 
> **Input:** text1 = "abcde", text2 = "ace" 
> **Output:** 3  
> **Explanation:** The longest common subsequence is "ace" and its length is 3.
> 
> **Example 2:**
> 
> **Input:** text1 = "abc", text2 = "abc"
> **Output:** 3
> **Explanation:** The longest common subsequence is "abc" and its length is 3.
> 
> **Example 3:**
> 
> **Input:** text1 = "abc", text2 = "def"
> **Output:** 0
> **Explanation:** There is no such common subsequence, so the result is 0.
> 
> **Constraints:**
> 
> - `1 <= text1.length, text2.length <= 1000`
> - `text1` and `text2` consist of only lowercase English characters.

## 🧠 Главная идея (динамическое программирование)

Мы хотим **избежать повторного вычисления** для одних и тех же подстрок.

### Подзадача:

Пусть `dp[i][j]` — это длина LCS между:

- первыми `i` символами `text1`
    
- первыми `j` символами `text2`
    

---

## 📐 Как заполняем таблицу `dp`?

Идём по всем символам строк. Для каждой пары `i, j`:

- Если `text1[i-1] == text2[j-1]`,  
    → значит, символ входит в LCS  
    → `dp[i][j] = dp[i-1][j-1] + 1`
    
- Иначе:  
    → либо пропускаем символ из `text1`, либо из `text2`  
    → `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
    

---

## 🎯 Пример

Для строк `text1 = "abcde"`, `text2 = "ace"`:

> [!NOTE]
> ||""|a|c|e|
> |---|---|---|---|---|
> |""|0|0|0|0|
> |a|0|1|1|1|
> |b|0|1|1|1|
> |c|0|1|2|2|
> |d|0|1|2|2|
> |e|0|1|2|3|

**Ответ = `dp[5][3] = 3`**

---

## 🧾 Итоговая формула перехода:

```python
if text1[i]==text2[j]:
	dp[i][j] = dp[i - 1][j - 1] + 1

else:
	dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

---

## ⏱️ Сложность:

- **Время**: `O(n * m)` — перебираем все пары символов
    
- **Память**: `O(n * m)` (можно оптимизировать до `O(min(n, m))`, если нужно только значение)
## 📌 Проблема без нулевой строки/столбца

Если ты **не добавляешь "нулевую строку и нулевой столбец"**, то индексы в `dp[i][j]` напрямую соответствуют `text1[i]` и `text2[j]`.

Это порождает проблему:

- Когда `i = 0` или `j = 0`, ты попытаешься обратиться к `dp[i-1][j-1]`, `dp[i-1][j]`, или `dp[i][j-1]`,
    
- А это выходит за границы массива (`-1` индекс) — нужно писать **отдельные условия** на границы.
## 🛠 Правильная версия:

Чтобы **не обращаться к отрицательным индексам**, нужно **сдвинуть все индексы на 1**, как и планировалось при создании `dp`:

```python
class Solution(object):
    def longestCommonSubsequence(self, text1, text2):
        dp = [[0]*(len(text2)+1) for _ in range(len(text1)+1)]
        for i in range(1, len(text1)+1):
            for j in range(1, len(text2)+1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[len(text1)][len(text2)]

```