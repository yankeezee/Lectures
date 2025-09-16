> [!problem]
> You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.
> 
> Return _the merged string._
> 
> **Example 1:**
> 
> **Input:** word1 = "abc", word2 = "pqr"
> **Output:** "apbqcr"
> **Explanation:** The merged string will be merged as so:
> word1:  a   b   c
> word2:    p   q   r
> merged: a p b q c r
> 
> **Example 2:**
> 
> **Input:** word1 = "ab", word2 = "pqrs"
> **Output:** "apbqrs"
> **Explanation:** Notice that as word2 is longer, "rs" is appended to the end.
> word1:  a   b 
> word2:    p   q   r   s
> merged: a p b q   r   s
> 
> **Example 3:**
> 
> **Input:** word1 = "abcd", word2 = "pq"
> **Output:** "apbqcd"
> **Explanation:** Notice that as word1 is longer, "cd" is appended to the end.
> word1:  a   b   c   d
> word2:    p   q 
> merged: a p b q c   d
> 
> **Constraints:**
> 
> - `1 <= word1.length, word2.length <= 100`
> - `word1` and `word2` consist of lowercase English letters.

---
**Идея решения:**
Итерируемся по индексам от `0` до `max(len(word1), len(word2))`. На каждой итерации добавляем символ из `word1[i]`, если он существует, затем из `word2[i]`, если он есть. Это даёт чередование. Если одна строка длиннее, её хвост автоматически добавляется после окончания короткой.

---
```python
def mergeAlternately(self, word1, word2):
	
	res = ""
	
	for i in range(max(len(word1), len(word2))):

		if i<len(word1):
			res+=word1[i]
	
		if i<len(word2):
			res+=word2[i]
	
	return res
```

---

**Временная сложность:** `O(n + m)`
где `n = len(word1)`, `m = len(word2)` — мы проходим максимум по `n + m` символам.

**Память:** `O(n + m)`
используем дополнительную строку `res`, содержащую результат с длиной до `n + m`.
