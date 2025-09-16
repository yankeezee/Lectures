> [!NOTE]
> Write a solution to find all customers who never order anything.
> 
> Return the result table in **any order**.
> 
> The result format is in the following example.
> 
> **Example 1:**
> 
> **Input:** 
> Customers table:
> +----+-------+
> | id | name  |
> +----+-------+
> | 1  | Joe   |
> | 2  | Henry |
> | 3  | Sam   |
> | 4  | Max   |
> +----+-------+
> Orders table:
> +----+------------+
> | id | customerId |
> +----+------------+
> | 1  | 3          |
> | 2  | 1          |
> +----+------------+
> **Output:** 
> +-----------+
> | Customers |
> +-----------+
> | Henry     |
> | Max       |
> +-----------+

```sql
select name as customers

from customers

where id not in (

select customerId

from Orders

);
```
Или альтернативный вариант с LEFT JOIN:
```sql
SELECT c.name AS Customers
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customerId
WHERE o.customerId IS NULL;
```