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

## **1. Century From Year**
  
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
  SELECT  (yr + 99) / 100 as century
  FROM years;
```

  ** Alternative solution**
```sql
  SELECT 
    CASE
      WHEN yr%100 = 0 THEN yr/100
      WHEN yr%100 > 0 THEN yr/100+1
    END AS century
  FROM years
```

  ** Alternative solution**
```sql
  SELECT
  EXTRACT(CENTURY FROM TO_DATE(yr::text, 'YYYY')) AS century
  FROM years;
```

</details>
