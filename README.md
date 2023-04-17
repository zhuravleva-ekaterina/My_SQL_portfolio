# Портфолио SQL
Zhuravleva Ekaterina | Журавлёва Екатерина | zhuravleva-ekaterina-v@yandex.ru |[Телеграм](https://t.me/ekaterina96zhuravleva)</p> 
------

Ниже приведены SQL запросы, краткое описание задачи. 

Данные запросы были выполнены в ходе обучения `в Яндекс.Практикуме, профессия "Аналитик данных".` Также представлены решения задач с платформы `sql-ex`, `Codewars`.

  <details>
<summary>Проект "Базовый SQL" (Яндекс.Практикум)</summary>
<br>

**Описание проекта:**
В данном проекте буду работать с базой данных, которая хранит информацию о венчурных фондах и инвестициях в компании-стартапы. Эта база данных основана на датасете Startup Investments, опубликованном на платформе Kaggle. 

**Цель проекта -** проанализировать данные о фондах и инвестициях и написать запросы к базе

  <details>
<summary>1. Посчитайте, сколько компаний закрылось.</summary>
<br>

**Решение:**

```sql
SELECT count(id)
FROM company
WHERE status = 'closed'
```  

  </details>

  <details>
<summary>2. Отобразите количество привлечённых средств для новостных компаний США. Используйте данные из таблицы company. Отсортируйте таблицу по убыванию значений в поле funding_total .</summary>
<br>

**Решение:**

```sql
SELECT sum(funding_total) 
FROM company
WHERE category_code = 'news' AND country_code = 'USA'
GROUP BY name
ORDER BY sum(funding_total) DESC
```  

  </details>
  
  <details>
<summary>3. Найдите общую сумму сделок по покупке одних компаний другими в долларах. Отберите сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.</summary>
<br>

**Решение:**

```sql
SELECT SUM(price_amount)
FROM acquisition
WHERE term_code = 'cash'
        AND acquired_at BETWEEN '2011-01-01' AND '2013-12-31'
```  
  </details> 
  
  <details>
<summary>4. Отобразите имя, фамилию и названия аккаунтов людей в твиттере, у которых названия аккаунтов начинаются на 'Silver'.</summary>
<br>

**Решение:**

```sql
SELECT first_name,
       last_name,
       twitter_username
FROM people
WHERE twitter_username LIKE 'Silver%'
``` 
  </details>  
    
  <details>
<summary>5. Выведите на экран всю информацию о людях, у которых названия аккаунтов в твиттере содержат подстроку 'money', а фамилия начинается на 'K'.</summary>
<br>

**Решение:**

```sql
SELECT *
FROM people
WHERE twitter_username LIKE '%money%' AND last_name LIKE 'K%'
``` 
  </details>  
    
  <details>
<summary>6. Для каждой страны отобразите общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируйте данные по убыванию суммы.</summary>
<br>

**Решение:**

```sql
SELECT country_code,
       sum(funding_total)
FROM company
GROUP BY country_code
ORDER BY 2 DESC
``` 
  </details>   
  </details>
