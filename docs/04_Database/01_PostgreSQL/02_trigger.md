---
title: Trigger
layout: default
nav_order: 2
parent: "PostgreSQL"
---

## Trigger example

We would like to update field_1 for all the records which have the same id (my_id) when the field_1 is updated for one record


```sql
-- Trigger function for field_1 update
CREATE OR REPLACE FUNCTION field_1_update(
  RETURNS trigger
  LANGUAGE plpgsql
)
AS $$
BEGIN
  EXECUTE format(
    'UPDATE %I
	SET field_1 = ''%s''
	WHERE my_id = %s;', TG_TABLE_NAME, NEW.field_1, NEW.my_id
  );
  RETURN NEW;
END;
$$


-- Create the trigger for all tables with a field_1 column
DO $$
DECLARE
  table_rec RECORD;
BEGIN
  -- Iterate over tables having the field_1 column
  FOR table_rec IN
    select c.relname
	from pg_class as c
	inner join pg_attribute as a on a.attrelid = c.oid
	where a.attname = 'field_1' and c.relkind = 'r'
  LOOP
    -- Construct and execute the create trigger statement
	EXECUTE format('CREATE OR REPLACE TRIGGER field_1_trigger_%I
	                AFTER UPDATE ON %I
					FOR EACH ROW
					WHEN (NEW.field_1 <> OLD.field_1)
					EXECUTE PROCEDURE field_1_update();', table_rec.relname, table_rec.relname);
  END LOOP;
END;
$$


-- Create the field_1 trigger for newly created tables
CREATE OR REPLACE FUNCTION create_field_1_triggers_for_new_tables()
  RETURNS event_trigger
  LANGUAGE plpgsql
AS $$
DECLARE
  obj record;
  table_name name;
  table_schema name;
BEGIN
  
  -- Check if the newly created table has a field_1 column
  FOR obj IN SELECT * FROM pg_event_trigger_ddl_commands()
  LOOP
  SELECT c.relname, n.nspname
  INTO table_name, table_schema
  FROM pg_class c
  JOIN pg_namespace n ON n.oid = c.relnamespace
  inner join pg_attribute as a on a.attrelid = c.oid
  WHERE c.oid = obj.objid AND c.relkind = 'r' and a.attname = 'field_1';
    IF table_schema = 'my_schema_name'
	THEN
	EXECUTE format('CREATE OR REPLACE TRIGGER field_1_trigger_%I
	                AFTER UPDATE ON %I
					FOR EACH ROW
					WHEN (NEW.field_1 <> OLD.field_1)
					EXECUTE PROCEDURE field_1_update();', table_name, table_name);
    END IF;
  END LOOP;
END;
$$;

-- Create event trigger for the field_1 for newly created tables
CREATE EVENT TRIGGER apply_field_1_triggers_on_table_creation
  ON ddl_command_end
  WHEN TAG IN ('CREATE TABLE')
  EXECUTE PROCEDURE create_field_1_triggers_for_new_tables();

```

The trigger will be created on all tables with field_1 column and on all newly created tables on the schema 'my_schema_name'























