### MY SQL portfolio
---
  <details>
<summary>Simple queries</summary>
<br>
  
## **1. Remove String Spaces**
  
  Task.
  
  Remove the spaces from the string, then return the resultant string.
  You are given a table 'nospace' with column 'x', return a table with column 'x' and your result in a column named 'res'.
  
  **Solution**

```sql
  SELECT x, REPLACE(x, ' ', '') AS res 
  FROM nospace
```

## **2. Century From Year**
  
  Task.
  
  Given a year, return the century it is in.

Examples
1705 --> 18
  
1900 --> 19
  
1601 --> 17
  
2000 --> 20

In SQL, you will be given a table years with a column yr for the year. Return a table with a column century.
  
  **Solution**

```sql
  SELECT (yr + 99) / 100 as century
  FROM years;
```

  **Alternative solution**
  
```sql
  SELECT 
    CASE
      WHEN yr%100 = 0 THEN yr/100
      WHEN yr%100 > 0 THEN yr/100+1
    END AS century
  FROM years;
```

  **Alternative solution**
  
```sql
  SELECT EXTRACT(CENTURY FROM TO_DATE(yr::text, 'YYYY')) AS century
  FROM years;
```

  ## **3. Returning Strings**
  
  Task.
  You are given a table person with a column name. Return a table with a column greeting that contains Hello, <name> how are you doing today?.

Example:

name = "John" -> greeting = "Hello, John how are you doing today?"

  **Solution**
  
```sql
  SELECT 'Hello, ' || name || ' how are you doing today?' AS greeting FROM person
```
  
  **Alternative solution**
  
```sql
  UPDATE person SET name = CONCAT('Hello, ', name, ' how are you doing today?');
  SELECT name AS greeting FROM person
```
  
  ## **4. Is n Divisible by x and y?**
  
  Task.
  You will be given a table with columns n, x, and y. Your task is to check if n is divisible by the two numbers x and y. All inputs are positive, non-zero digits.

  **Solution**
  
```sql
  SELECT id,
    CASE
      WHEN n%x=0 AND n%y=0
      THEN true
      ELSE false
    END AS res
  FROM kata
```

  ## **5. Expressions Matter**
  
  Task.
  Given three integers a, b, c where 1  ≤  a,  b,  c  ≤  10, return the largest number obtained after inserting the following operators and brackets in any order: +, *, (). You can use the same operator more than once, and it is not necessary to use all the operators and brackets. However, you must use a, b, and c only once, and you may not swap their order.

Example:

```
Given a = 1, b = 2, c = 3:
1 * (2 + 3) = 5
1 * 2 * 3 = 6
1 + 2 * 3 = 7
(1 + 2) * 3 = 9
So the maximum value that you can obtain is 9.
```
  **Solution**
  
```sql
  SELECT GREATEST(a * b * c, a + b + c, a * (b + c), (a + b) * c)
    AS res
  FROM expression_matter;
```
</details>
