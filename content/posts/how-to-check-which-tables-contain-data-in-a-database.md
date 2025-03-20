---
title: "How to Check Which Tables Contain Data in a Database"
date: 2025-01-29
---

When managing a database, it's often useful to identify which tables contain data. This can help with troubleshooting, maintenance, or simply understanding the database's structure and usage. Here’s how to check for tables with data across different relational database management systems (RDBMS).

## **For PostgreSQL**

In PostgreSQL, you can dynamically generate a query to check row counts for all tables within a schema using `pg_catalog.pg_tables`:

### Using a PL/pgSQL Block:

```
DO $$
DECLARE
    tbl RECORD;
BEGIN
    FOR tbl IN
        SELECT schemaname, tablename
        FROM pg_catalog.pg_tables
        WHERE schemaname = 'public'  -- Change schema if needed
    LOOP
        EXECUTE format(
            'SELECT COUNT(*) AS row_count, ''%I'' AS table_name FROM %I.%I',
            tbl.tablename, tbl.schemaname, tbl.tablename
        );
    END LOOP;
END $$;
```

This script loops through all tables in the `public` schema and outputs the row count for each table.

### Alternative Query:

If you want to see the row counts for all tables in one query:  

```
SELECT table_name,
       (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema = 'public') AS row_count
FROM information_schema.tables
WHERE table_schema = 'public';
```

This approach lists the tables and their corresponding row counts.

## **For MySQL**

In MySQL, you can check which tables contain data by querying the row count directly from each table. Here’s a way to do this dynamically:

### Query to Get Row Counts:

```
SELECT table_name, table_rows
FROM information_schema.tables
WHERE table_schema = 'your_database_name';
```

- Replace `your_database_name` with the name of your database.
- The `table_rows` column provides an approximate count of rows for each table (note that this value might not always be 100% accurate).

## **For SQL Server**

In SQL Server, you can use the `sys.tables` system view to dynamically check the row counts for all tables:

### Query:

```
SELECT t.name AS table_name,
       p.rows AS row_count
FROM sys.tables t
JOIN sys.partitions p
     ON t.object_id = p.object_id
WHERE p.index_id IN (0, 1);  -- 0 = Heap, 1 = Clustered Index
```

This query returns the row counts for all user tables in the current database.

## **For SQLite**

In SQLite, you can use the `sqlite_master` table to get a list of tables and dynamically query each one for its row count:

### Query:

```
SELECT name AS table_name,
       (SELECT COUNT(*) FROM name) AS row_count
FROM sqlite_master
WHERE type = 'table';
```

This query lists all tables and their row counts.

## Conclusion

Checking which tables contain data is a common task across all databases. Most databases provide ways to dynamically generate queries or use system views to count rows in each table. By leveraging these techniques, you can quickly identify which tables have data and understand your database's structure more effectively.

Go to Source
