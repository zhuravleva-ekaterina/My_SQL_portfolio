### MY SQL portfolio
---

  <details>
<summary>Data Correction Requests</summary>
<br>

## **1. Создание пустой таблицы**
**Задание.**
	
Создать таблицу поставка (supply), которая имеет ту же структуру, что и таблиц book.

  <details>
<summary>Структура и наполнение таблицы</summary>
<br>


| Поле	|Тип, описание |
| ------ | -------------- |
| supply_id	| INT PRIMARY KEY AUTO_INCREMENT |
| title	| VARCHAR(50) |
| author	| VARCHAR(30) |
| price	| DECIMAL(8, 2) |
| amount	| INT |

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	CREATE TABLE supply(
    	supply_id INT PRIMARY KEY AUTO_INCREMENT, 
    	title VARCHAR(50),
    	author VARCHAR(30),
    	price DECIMAL(8, 2),
    	amount INT);
```  
  </details>
  
## **2. Добавление записей, вложенные запросы**
**Задание.**
	
Занести из таблицы supply в таблицу book только те книги, авторов которых нет в  book.

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |



| supply_id | title          | author           | price  | amount |
|-----------|----------------|------------------|--------|--------|
| 1         | Лирика         | Пастернак Б.Л.   | 518.99 | 2      |
| 2         | Черный человек | Есенин С.А.      | 570.20 | 6      |
| 3         | Белая гвардия  | Булгаков М.А.    | 540.50 | 7      |
| 4         | Идиот          | Достоевский Ф.М. | 360.80 | 3      |

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	INSERT INTO book ( title, author,  price,  amount) 
	SELECT title, author, price, amount 
	FROM supply
	WHERE author NOT IN (
        		SELECT author 
    			FROM book);

	SELECT * FROM book;
```  
  </details>
  
## **3. Добавление записей из другой таблицы**
**Задание.**
	
Добавить из таблицы supply в таблицу book, все книги, кроме книг, написанных Булгаковым М.А. и Достоевским Ф.М.

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |


| supply_id | title          | author           | price  | amount |
|-----------|----------------|------------------|--------|--------|
| 1         | Лирика         | Пастернак Б.Л.   | 518.99 | 2      |
| 2         | Черный человек | Есенин С.А.      | 570.20 | 6      |
| 3         | Белая гвардия  | Булгаков М.А.    | 540.50 | 7      |
| 4         | Идиот          | Достоевский Ф.М. | 360.80 | 3      |

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	INSERT INTO book ( title, author,  price,  amount) 
	SELECT title, author, price, amount 
	FROM supply
	WHERE author<> 'Булгаков М.А.' AND author<> 'Достоевский Ф.М.';

	SELECT * FROM book;
```  
  </details>  

## **4. Запросы на обновление нескольких столбцов**
**Задание.**
	
В таблице book необходимо скорректировать значение для покупателя в столбце buy таким образом, чтобы оно не превышало количество экземпляров книг, указанных в столбце amount. А цену тех книг, которые покупатель не заказывал, снизить на 10%.

  <details>
<summary>Структура и наполнение таблицы</summary>
<br>

| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	UPDATE book 
	SET buy = IF(buy > amount, amount, buy),
	price = IF(buy = 0 , price * 0.9, price);
	
	SELECT * FROM book;
```  
  </details> 

## **5. Запросы на обновление нескольких таблиц**
**Задание.**
	
Для тех книг в таблице book , которые есть в таблице supply, не только увеличить их количество в таблице book ( увеличить их количество на значение столбца amountтаблицы supply), но и пересчитать их цену (для каждой книги найти сумму цен из таблиц book и supply и разделить на 2).

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |


| supply_id | title          | author           | price  | amount |
|-----------|----------------|------------------|--------|--------|
| 1         | Лирика         | Пастернак Б.Л.   | 518.99 | 2      |
| 2         | Черный человек | Есенин С.А.      | 570.20 | 6      |
| 3         | Белая гвардия  | Булгаков М.А.    | 540.50 | 7      |
| 4         | Идиот          | Достоевский Ф.М. | 360.80 | 3      |

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	UPDATE book, supply 
	SET book.amount = book.amount + supply.amount,
	book.price = (book.price + supply.price)/2
	WHERE book.title = supply.title AND book.author = supply.author;

	SELECT * FROM book;
```  
  </details>


  </details>
 
 ---
 
  <details>
<summary>Simple queries</summary>
<br>

  ## **1. Предложение GROUP BY, HAVING**
**Задание.**
	
Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

Схема БД состоит из четырех таблиц:

Схема БД состоит из четырех таблиц:
	  
Product(maker, model, type)
	  
PC(code, model, speed, ram, hd, cd, price)
	  
Laptop(code, model, speed, ram, hd, price, screen)
	  
Printer(code, model, color, type, price)
	  
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	SELECT hd FROM pc
	GROUP BY hd
	HAVING count(hd)>1
```  
  </details>	

  ## **2. Пересечение и разность**
**Задание.**
	
Найдите производителя, выпускающего ПК, но не ПК-блокноты.

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

Схема БД состоит из четырех таблиц:
	  
Product(maker, model, type)
	  
PC(code, model, speed, ram, hd, cd, price)
	  
Laptop(code, model, speed, ram, hd, price, screen)
	  
Printer(code, model, color, type, price)
	  
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.
  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	SELECT maker FROM Product WHERE type='PC'
	EXCEPT SELECT maker FROM Product WHERE type='Laptop'
```  
  </details>	
	
	
## **3. Remove String Spaces**
  
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
  
## **4. Century From Year**
  
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
  
  ## **5. Returning Strings**
  
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
    
  ## **6. Is n Divisible by x and y?**
  
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
     
  ## **7. Expressions Matter**
  
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
    
## **8.Count Odd Numbers below n**

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
    
## **9.  Sum of odd numbers**

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
    
## **10. Fake Binary**

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
    
## **11. Convert to Hexadecimal**

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
    
## **12. Rounding Decimals**

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

## **1. Запросы на выборку из нескольких таблиц**
**Задание.**	

Вывести информацию о книгах (жанр, книга, автор), относящихся к жанру, включающему слово «роман» в отсортированном по названиям книг виде.	

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
	SELECT name_genre, title, name_author
	FROM genre 
		INNER JOIN book ON genre.genre_id=book.genre_id
		INNER JOIN author ON book.author_id=author.author_id
	WHERE name_genre='Роман'
	ORDER BY title; 
```  
  </details>
	
## **2. Запрос на обновление, связанные таблицы**
**Задание.**
	
Для книг, которые уже есть на складе (в таблице book), но по другой цене, чем в поставке (supply),  необходимо в таблице book увеличить количество на значение, указанное в поставке,  и пересчитать цену. А в таблице  supply обнулить количество этих книг. 
	Формула для пересчета цены:
	
 price=(p_1*k_1+p_2*k_2)/(k_1+k_2), 

где 
	p1, p2 - цена книги в таблицах book и supply;
	k1, k2 - количество книг в таблицах book и supply.
  
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
	
  
  ## **3.  Явные операции соединения, подзапрос**
**Задание.**
	
Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

Схема БД состоит из четырех таблиц:

Product(maker, model, type)

PC(code, model, speed, ram, hd, cd, price)

Laptop(code, model, speed, ram, hd, price, screen)

Printer(code, model, color, type, price)

Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

  </details>

  <details>
<summary>Solution</summary>
<br>

```sql
	SELECT DISTINCT maker, price
	FROM product, printer
	WHERE product.model = printer.model
		AND printer.color = 'y'
		AND printer.price = (SELECT MIN(price) FROM printer WHERE printer.color = 'y');
```  
  </details>

  ## **4.  Явные операции соединения. Пересечение и разность**
**Задание.**
	
Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

  <details>
<summary>Структура и наполнение таблиц</summary>
<br>

Схема БД состоит из четырех таблиц:

Product(maker, model, type)

PC(code, model, speed, ram, hd, cd, price)

Laptop(code, model, speed, ram, hd, price, screen)

Printer(code, model, color, type, price)

Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

  </details>
  
  <details>
<summary>Solution</summary>
<br>

```sql
	SELECT DISTINCT maker
	FROM product t1 
	JOIN pc t2 ON t1.model=t2.model
	WHERE speed>=750 AND maker IN (
		SELECT maker FROM product t1 
		JOIN laptop t2 ON t1.model=t2.model
		WHERE speed>=750);
```  
  </details>
  
  </details>
