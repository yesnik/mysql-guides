# Select order by `field()`

We have a table:

|id|program_year|program|
|--|------------|-------|
|1|2022|Fall|
|2|2022|Spring|
|3|2022|Fall|
|4|2021|Fall|
|5|2021|Spring|

We can sort this items so "Spring" comes before "Fall" within a year. String function [FIELD()](https://dev.mysql.com/doc/refman/9.1/en/string-functions.html#function_field) will help us:

```sql
SELECT * FROM test_table
ORDER BY 
    program_year,
    FIELD(program, 'Spring', 'Fall')
```

|id|program_year|program|
|--|------------|-------|
|5|2021|Spring|
|4|2021|Fall|
|2|2022|Spring|
|1|2022|Fall|
|3|2022|Fall|
