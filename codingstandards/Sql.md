# SQL Coding Standards

## Layout

### Avoid single lines

```sql
-- Not Great
SELECT  * FROM [schema].[table] WHERE [id] = 1

-- Better
SELECT  *
FROM [schema].[table]
WHERE [id] = 1
```

### Indenting

```sql
-- Not Great
SELECT  * FROM [schema].[table] t1 INNER JOIN [schema].[table2] t2 ON t1.id = t2.id WHERE t1.[id] = 1

-- Better
SELECT  *
FROM [schema].[table] t1
INNER JOIN [schema].[table2] t2
ON t1.id = t2.id
WHERE t1.[id] = 1

-- Best - indenting the on allows the reader to clearly identify the join conditions.
SELECT  *
FROM [schema].[table] t1
INNER JOIN [schema].[table2] t2
    ON t1.id = t2.id
WHERE t1.[id] = 1


SELECT  *
FROM [schema].[table] t1
INNER JOIN [schema].[table2] t2
    ON t1.id = t2.id
        AND t1.field2 = t2.field2
WHERE t1.[id] = 1

```
