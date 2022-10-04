### MY SQL portfolio
---
  <details>
<summary>Simple queries</summary>
<br>
  
## **1. Remove String Spaces**
  
  Your task is to remove the spaces from the string, then return the resultant string.
  You are given a table 'nospace' with column 'x', return a table with column 'x' and your result in a column named 'res'.
  
  **Solution**

```sql
  SELECT x, REPLACE(x, ' ', '') AS res 
  FROM nospace
```
</details>
