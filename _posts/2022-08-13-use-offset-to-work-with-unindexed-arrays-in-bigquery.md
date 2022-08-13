---
layout: single
title: Use offset to work with unindexed arrays in BigQuery
tags: coding image-recognition image-processing
---

Recently I faced a challenge of working with multilevel nested arrays in BigQuery. The table I was working with had a structure somewhat like this:

* id
* level_one_struct_array
  * name
  * ...
  * level_two_struct_array
    * name
    * ...

I needed to unnest the arrays, change values within the level two array, and then reaggregate everything.

The trouble is, using `UNNEST` in BigQuery doesn't preserve order. So if I unnested each array and then reaggregated them, I wouldn't necessarily get things back in the right order. And in my use case, order mattered.

The solution: use `OFFSET` to add indexes to the arrays, somewhat as follows:

```sql
WITH level_one_flattened AS (
    SELECT
        id,
        level_one_offset,
        level_one_struct.*
    FROM table_name,
        table_name.level_one_struct_array AS level_one_struct 
        WITH OFFSET AS level_one_offset    
), 

level_two_flattened AS (
    SELECT
        id,
        level_one_offset,
        level_two_offset,
        level_two_struct.*
    FROM level_one_flattened,
        level_one_flattened.level_two_struct_array AS level_two_struct 
        WITH OFFSET AS level_two_offset    
), 

...
```

With this done, I could work with `level_one_flattened` and `level_two_flattened`, then reaggregate everything at the end in the appropriate order using the offset-generated indexes.

It's not rocket science, and I'm sure people with much greater expertise in SQl than me are very familiar with this. But it wasn't something I needed to use until recently, when it came in very handy.

[Read more about `UNNEST` and `OFFSET` in BigQuery's docs here](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#unnest_operator).