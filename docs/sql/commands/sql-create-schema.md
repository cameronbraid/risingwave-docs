---
id: sql-create-schema
title: CREATE SCHEMA
description: Create a new schema.
slug: /sql-create-schema
---

Use the `CREATE SCHEMA` command to create a new schema.

## Syntax

```sql
CREATE SCHEMA [IF NOT EXISTS] [database_name.]schema_name;
```

import rr from '@theme/RailroadDiagram'

export const svg = rr.Diagram(
rr.Stack(
   rr.Sequence(
      rr.Terminal('CREATE SCHEMA'),
      rr.Optional(rr.Terminal('IF NOT EXISTS')),
      rr.Optional(rr.Sequence(
         rr.NonTerminal('database_name', 'skip'),
         rr.Terminal('.')
      )),
      rr.NonTerminal('schema_name', 'skip'),
      rr.Terminal(';'),
   ),
)
);

<drawer SVG={svg} />


## Parameters
|Parameter or clause        | Description           |
|---------------------------|-----------------------|
|*schema_name*                   |The name of the schema to be created.|
|<b>IF NOT EXISTS</b> clause      |Creates a schema if the schema name has not already been used. Otherwise throws an error.|
|*database_name*                 |The name of the database for the schema to be created in. If not specified, the schema will be created in the default database "dev".|

## Example
```sql
CREATE SCHEMA IF NOT EXISTS schema_1;
```

:::note

Names and unquoted identifiers are case-insensitive. Therefore, you must double-quote any of these fields for them to be case-sensitive.

:::