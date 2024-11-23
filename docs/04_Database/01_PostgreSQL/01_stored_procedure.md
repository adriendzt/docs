---
title: Stored Procedure
layout: default
nav_order: 1
parent: "PostgreSQL"
---

## Stored procedure for counting

Example of a PostgreSQL stored procedure to count the records given an id and a table name:


```sql
CREATE OR REPLACE FUNCTION count_function(
  Var_Int_my_id INT,
  Var_Str_Table_Name TEXT,
  OUT Var_Int_Count INT
)
AS $$
BEGIN
  -- Initialize the ouput variable
  Var_Int_Count := 0;
  
  -- Execute the query to count
  EXECUTE 'SELECT COUNT(my_column) FROM ' || Var_Str_Table_Name || ' WHERE my_id = $1' INTO Var_Int_Count
  USING Var_Int_my_id;
  
  -- Return the result
  RETURN;
END;
$$ LANGUAGE plsgsql;
```

To call this function:

```sql
select count_function('5','my_table_name');
```


## Stored procedure to select and return a json

Example of a PostgreSQL stored procedure to select the records given an id (my_id) and a table name and return json of the records:


```sql
CREATE OR REPLACE FUNCTION select_function(
  Var_Int_my_id INT,
  Var_Str_Table_Name TEXT,
  OUT Var_Str_Query_Output JSON
)
AS $$
BEGIN
  -- Initialize the ouput variable
  Var_Str_Query_Output := '[]'::JSON;
  
  -- Execute the query to count
  EXECUTE 'SELECT json_agg(e) FROM (SELECT my_column1, my_column2 FROM ' || Var_Str_Table_Name || ' WHERE my_id = $1) e'
  INTO Var_Str_Query_Output
  USING Var_Int_my_id;
  
  -- Return the result
  RETURN;
END;
$$ LANGUAGE plsgsql;
```

To call this function:

```sql
select select_function('5','my_table_name');
```

## Stored procedure to select given 2 parameters and return a json


Example of a PostgreSQL stored procedure to select the records given an id (my_id), a field (my_field) and a table name and return json of the records:

```sql
CREATE OR REPLACE FUNCTION select_function2(
  Var_Int_my_id INT,
  Var_Str_Table_Name TEXT,
  Var_Str_my_field TEXT,
  OUT Var_Str_Query_Output JSON
)
AS $$
BEGIN
  -- Initialize the ouput variable
  Var_Str_Query_Output := '[]'::JSON;
  
  -- Execute the query to count
  EXECUTE 'SELECT json_agg(e) FROM (SELECT my_column1, my_column2 FROM ' || Var_Str_Table_Name || ' WHERE my_id = $1 AND my_field = $2) e'
  INTO Var_Str_Query_Output
  USING Var_Int_my_id,Var_Str_my_field;
  
  -- Return the result
  RETURN;
END;
$$ LANGUAGE plsgsql;
```

To call this function:

```sql
select select_function2('5','my_table_name','my_field_value');
```

## Stored procedure to update records given a parameter and a table name

Example of a PostgreSQL stored procedure to update the field my_field given an id (my_id) and a table name:

```sql
CREATE OR REPLACE FUNCTION update_function(
  Var_Int_my_id INT,
  Var_Str_Table_Name TEXT,
  Var_Str_my_field_new_value TEXT,
  OUT Var_Str_Update_Result TEXT
)
AS $$
DECLARE
  pre_count INT;
  update_count INT;
BEGIN
  -- Initialize the ouput variable
  Var_Str_Update_Result := '';
  
  -- Execute the query to update the record and get the count of updated rows
  
  EXECUTE 'SELECT COUNT(*) FROM ' || Var_Str_Table_Name || ' WHERE my_id =' || Var_Int_my_id INTO pre_count
  EXECUTE 'UPDATE ' || Var_Str_Table_Name || ' SET my_field =''' || Var_Str_my_field_new_value || ''' WHERE my_id =' || Var_Int_my_id;
  GET DIAGNOSTICS update_count = ROW_COUNT;
  
  -- Check if the update was successful
  IF pre_count = update_count THEN
    Var_Str_Update_Result := 'Success';
  ELSE
    Var_Str_Update_Result := 'Failure';
  END IF;


  
  -- Return the result
  RETURN;
END;
$$ LANGUAGE plsgsql;
```

To call this function:

```sql
select update_function('5','my_table_name','my_new_value');
```