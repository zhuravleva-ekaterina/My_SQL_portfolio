### MY SQL portfolio
---
  <details>
<summary>Simple queries</summary>
<br>
  
## **1. Remove String Spaces**
  
  Task.
  
  Remove the spaces from the string, then return the resultant string.
  You are given a table 'nospace' with column 'x', return a table with column 'x' and your result in a column named 'res'.
  
  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT x, REPLACE(x, ' ', '') AS res 
  FROM nospace;
```
  </details>
  
## **2. Century From Year**
  
  Task.
  
  Given a year, return the century it is in.

Examples:
```
1705 --> 18
  
1900 --> 19
  
1601 --> 17
  
2000 --> 20
``` 

In SQL, you will be given a table years with a column yr for the year. Return a table with a column century.
  
  <details>
<summary>Solution</summary>
<br>
    
```sql
  SELECT (yr + 99) / 100 as century
  FROM years;
```
  </details>
  
  <details>
<summary>Alternative solution</summary>
<br>
  
```sql
  SELECT 
    CASE
      WHEN yr%100 = 0 THEN yr/100
      WHEN yr%100 > 0 THEN yr/100+1
    END AS century
  FROM years;
```
  </details>

  <details>
<summary>Alternative solution</summary>
<br>
  
```sql
  SELECT EXTRACT(CENTURY FROM TO_DATE(yr::text, 'YYYY')) AS century
  FROM years;
```
  </details>
  
  ## **3. Returning Strings**
  
  Task.
  You are given a table person with a column name. Return a table with a column greeting that contains Hello, <name> how are you doing today?.

Example:

```
name = "John" -> greeting = "Hello, John how are you doing today?"
```

  <details>
<summary>Solution</summary>
<br>
  
```sql
  SELECT 'Hello, ' || name || ' how are you doing today?' AS greeting FROM person;
```
</details>
  
  <details>
<summary>Alternative solution</summary>
<br>
  
```sql
  UPDATE person SET name = CONCAT('Hello, ', name, ' how are you doing today?');
  SELECT name AS greeting FROM person;
```
</details>
    
  ## **4. Is n Divisible by x and y?**
  
  Task.
  You will be given a table with columns n, x, and y. Your task is to check if n is divisible by the two numbers x and y. All inputs are positive, non-zero digits.

  <details>
<summary>Solution</summary>
<br>
  
```sql
  SELECT id,
    CASE
      WHEN n%x=0 AND n%y=0
      THEN true
      ELSE false
    END AS res
  FROM kata;
```
</details>
     
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
  <details>
<summary>Solution</summary>
<br>
  
```sql
  SELECT GREATEST(a * b * c, a + b + c, a * (b + c), (a + b) * c)
    AS res
  FROM expression_matter;
```
</details>
    
## **6.Count Odd Numbers below n**

Task.
Given a number n, return the number of positive odd numbers below n.

Examples (Input -> Output):
```
7  -> 3 (because odd numbers below 7 are [1, 3, 5])
15 -> 7 (because odd numbers below 15 are [1, 3, 5, 7, 9, 11, 13])
```

  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT n, n/2 AS res FROM oddcount;
```
</details>
    
## **7.  Sum of odd numbers**

Task.
Given the triangle of consecutive odd numbers:
```

             1
          3     5
       7     9    11
   13    15    17    19
21    23    25    27    29
...
```
Calculate the row sums of this triangle from the row index (starting at index 1). The table nums contains the integer n (the input row index).

Examples:
```
n = 1 -> res = 1
n = 2 -> res = 8 (because 3 + 5 = 8)
n = 3 -> res = 27 (because 7 + 9 + 11 = 27)
```

  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT n * n * n AS res
  FROM nums;
```
</details>
    
## **8. Fake Binary**

Task.
Given a string of digits, you should replace any digit below 5 with '0' and any digit 5 and above with '1'. Return the resulting string.
Note: input will never be an empty string

  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT x,
  regexp_replace(regexp_replace(x, '[0-4]', '0', 'g'), '[5-9]', '1', 'g') AS res
  FROM fakebin;
```
</details>
    
## **9. Convert to Hexadecimal**

Task.
Turn the numeric columns (arms, legs) into equivalent hexadecimal values.

monsters table schema:
```
- id
- name
- legs
- arms
- characteristics
```

  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT to_hex(legs) AS legs, 
          to_hex(arms) AS arms 
  FROM monsters;
```
</details>
    
## **10. Rounding Decimals**

Task.
Given the following table 'decimals':
```
- id
- number1
- number2
```
Return a table with two columns (number1, number2), the value in number1 should be rounded down and the value in number2 should be rounded up.

  <details>
<summary>Solution</summary>
<br>

```sql
  SELECT floor(number1) AS number1, 
         ceiling(number2) AS number2 
  FROM decimals
```
</details>

</details>

  ---
<details>
<summary>Difficult queries</summary>
<br>
  
## **1.**
**Задание.**
	
Для книг, которые уже есть на складе (в таблице book), но по другой цене, чем в поставке (supply),  необходимо в таблице book увеличить количество на значение, указанное в поставке,  и пересчитать цену. А в таблице  supply обнулить количество этих книг. 
	Формула для пересчета цены:
	
$$ price=(p_1*k_1+p_2*k_2)\(k_1+k_2), $$

где \\( p1, p2 - цена книги в таблицах book и supply;
	k1, k2 - количество книг в таблицах book и supply. \\)
  
  <details>
<summary>Структура и наполнение таблиц</summary>
<br>
  
  Таблица book
  
| book_id | title                 | author_id | genre_id | price  | amount |
|---------|-----------------------|-----------|----------|--------|--------|
| 1       | Мастер и Маргарита    | 1         | 1        | 670.99 | 3      |
| 2       | Белая гвардия         | 1         | 1        | 540.50 | 5      |
| 3       | Идиот                 | 2         | 1        | 460.00 | 10     |
| 4       | Братья Карамазовы     | 2         | 1        | 799.01 | 3      |
| 5       | Игрок                 | 2         | 1        | 480.50 | 10     |
| 6       | Стихотворения и поэмы | 3         | 2        | 650.00 | 15     |
| 7       | Черный человек        | 3         | 2        | 570.20 | 6      |
| 8       | Лирика                | 4         | 2        | 518.99 | 2      |


  Таблица supply
  

| supply_id | title                 | author           | price  | amount |
|-----------|-----------------------|------------------|--------|--------|
| 1         | Доктор Живаго         | Пастернак Б.Л.   | 380.80 | 4      |
| 2         | Черный человек        | Есенин С.А.      | 570.20 | 6      |
| 3         | Белая гвардия         | Булгаков М.А.    | 540.50 | 7      |
| 4         | Идиот                 | Достоевский Ф.М. | 360.80 | 3      |
| 5         | Стихотворения и поэмы | Лермонтов М.Ю.   | 255.90 | 4      |
| 6         | Остров сокровищ       | Стивенсон Р.Л.   | 599.99 | 5      |


  Таблица author                         
  
			
| author_id | name_author      |				
|-----------|------------------|			
| 1         | Булгаков М.А.    |				
| 2         | Достоевский Ф.М. |				
| 3         | Есенин С.А.      |				
| 4         | Пастернак Б.Л.   |				
| 5         | Лермонтов М.Ю.   |							

  Таблица genre
	  
| genre_id | name_genre  |
|----------|-------------|
| 1        | Роман       |
| 2        | Поэзия      |
| 3        | Приключения |	  
    
  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	UPDATE book b
        	INNER JOIN author a USING(author_id)
        	INNER JOIN supply s ON b.title=s.title 
                                AND a.name_author=s.author
	SET b.amount=b.amount + s.amount,
    	b.price=(b.price*b.amount + s.price*s.amount)/(b.amount+s.amount),
   	s.amount=0
	WHERE b.price <> s.price;
```  
  </details>
	
  </details>
