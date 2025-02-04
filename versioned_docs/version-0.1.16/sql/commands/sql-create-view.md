---
id: sql-create-view
title: CREATE VIEW
description: Create a non-materialized view.
slug: /sql-create-view
---

Use the `CREATE VIEW` command to create a non-materialized view, which runs every time the view is referenced in a query. A non-materialized view can be created based on sources, tables, views, or indexes.

## Syntax

```sql
CREATE VIEW view_name [ ( column_name [, ...] ) ] AS select_query;
```

## Parameters

|Parameter                  | Description           |
|---------------------------|-----------------------|
|*mv_name*                  |The name of the view to be created.|
|*column_name*              |Specify the columns of the view.|
|*select_query*             |A SELECT query that retrieves data for the view. See [SELECT](sql-select.md) for the syntax and examples of the `SELECT` command.|


## Examples

The following statements create views based a user-defined table and a materialized source, and then create a new view based on the existing views. The data for the materialized source is generated by the built-in [load generator](/create-source/create-source-datagen.md).

```sql
-- Create a table and add some records.

CREATE TABLE t1 (a int, b int, c int);

INSERT INTO t1 VALUES (115, 1, 8), (585, 2, 3), (601, 3, 7);

-- Create a source (whose data is generated by the built-in generator).

CREATE MATERIALIZED SOURCE s1 (i1 int, c1 varchar) 
WITH (
     connector = 'datagen',
     fields.i1.kind = 'sequence',
     fields.i1.start = '1',
     fields.c1.kind = 'random',
     fields.c1.length = '16',
     fields.c1.seed = '3',
     datagen.rows.per.second = '10'
 ) ROW FORMAT JSON;

-- Create views based on the table, source, and existing views.

CREATE VIEW v1 (a1, b1) AS SELECT a, b FROM t1;

CREATE VIEW v2 AS SELECT * FROM s1 ORDER BY i1;

CREATE VIEW v3 AS SELECT a1, i1, c1 FROM v1 LEFT JOIN v2 ON v1.b1=v2.i1;
```
Let's query view `v3`.
```sql
SELECT * FROM v3;
```
```
 a1  | i1 |        c1        
-----+----+------------------
 115 |  1 | pGWJLsbmPJZZWpBe
 585 |  2 | FT7BRdifYMrRgIyI
 601 |  3 | 0zsMbNLxQh9yYtHh
```


:::note

Names and unquoted identifiers are case-insensitive. Therefore, you must double-quote any of these fields for them to be case-sensitive.

:::