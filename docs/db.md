# SQL/PLSQL
This style guide is a list of *dos* and *don'ts* for SQL/PLSQL related code.

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Naming things](#naming-things)
1. [Documenting code](#documenting-code)
1. [Immutability of tuples](#immutability-of-tuples)

## General
* capitalize reserved words (CREATE, FUNCTION, INDEX, TABLE, SELECT, DELETE etc.), datatypes (INTEGER, BIGINT, STATE, etc.)
* main keywords on new line
* always use short meaningful table names, attrs etc.
* don't prefix tables
* table names in plural
* group related statements together

Example:
```sql
DROP TABLE IF EXISTS guidelines;
CREATE TABLE guidelines (
  id BIGSERIAL NOT NULL PRIMARY KEY,
  item_id SMALLINT NOT NULL DEFAULT 1,
  revision SMALLINT NOT NULL DEFAULT 0,
  title TEXT NOT NULL,
  state GUIDELINE_STATE NOT NULL DEFAULT ('CHANGED')::STATE,
  created_by_user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE NO ACTION,
  responsible_user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE NO ACTION,
  review_interval BIGINT NOT NULL DEFAULT (365)::BIGINT,
  date_created TIMESTAMP WITH TIME ZONE
) WITH ( OIDS=FALSE );

CREATE OR REPLACE FUNCTION convertPageVersionState(integer) RETURNS STATE AS $$
  BEGIN
    CASE $1
      WHEN 0 THEN
        RETURN ('CHANGED')::STATE;
      WHEN 1 THEN
        RETURN ('CHANGED')::STATE;
      WHEN 2 THEN
        RETURN ('VALIDATED')::STATE;
      WHEN 3 THEN
        RETURN ('CHANGED')::STATE;
      WHEN 4 THEN
        RETURN ('CHANGED')::STATE;
      ELSE
        RETURN ('DELETED')::STATE;
    END CASE;
  END;
$$ LANGUAGE plpgsql;
```

## Code Formatting
* Indent with 2 spaces
* Add new line at end of files
* Clean up trailing whitespaces at end-of-line

## Naming things
### Custom Datatypes
* Use uppercase for custom data types:
```sql
CREATE TYPE STATE AS ENUM ('CHANGED', 'VALIDATED', 'DELETED');
```

* If the enum is intended for a specific relation (singular) only prefix the relation name to indicate its specificity:
CREATE TYPE GUIDELINE_STATE AS ENUM ('CHANGED', 'IN_VALIDATION', 'VALIDATED', 'DELETED');

### Table
* Names in plural
* No prfixes like "guideline_..."
* Table name for M:N relations -> M_N (eg. guidelines_details)

### Attribute
__Delimiter for multiple words__ in attribute names is '_':
```sql
created_by_user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE NO ACTION,
responsible_user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE NO ACTION,
```

### Constraint
__Prefix UNIQUE constraints__ with 'unique':
```sql
ALTER TABLE guidelines_detail ADD CONSTRAINT unique_guidelines_details_revision UNIQUE (guidelines_id, details_id);
```

__Prefix INDEX constraints__ with 'index':
```sql
CREATE INDEX index_guidelines_details_title ON page_detail USING btree (lower(title));
```

### Key
__PK__ is always named 'id':
```sql
...
id BIGSERIAL NOT NULL PRIMARY KEY,
...
```
__FK__ names consists of the referenced table name and the PK (usually 'id'), with an optional prefix describing the role of the attribue:
```sql
...
guidelines_id BIGINT NOT NULL REFERENCES guidelines(id) ON DELETE CASCADE,
...
```

or:

```sql
...
responsible_user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE NO ACTION,
...
```

### SEQUENCE
__Sequence__ is always named 'seq':
```sql
...
CREATE SEQUENCE guidelines_item_seq START 10000;
...
```

### PLPGSQL/PLV8 Function and Variable
Function and variable names are:
* lowercase
* seperated with '_'
Example:
```sql
DROP FUNCTION IF EXISTS get_current_revision_of_page(has_id INTEGER);
CREATE FUNCTION get_current_revision_of_page(has_id INTEGER) RETURNS INTEGER AS $$
  SELECT COALESCE((SELECT revision FROM page WHERE item_id = (
    SELECT item_id FROM page WHERE id = has_id
  ) ORDER BY revision DESC LIMIT 1), 0)
$$ LANGUAGE SQL;
```

#### Function Name
* Prefix 'assert_' for basic 'unit test like' functions
* Prefix 'test_' for 'test' functions

## Documenting Code
* Use standard SQL comments like '-- '.
* Use the following doc block for functions:
```sql
--
-- STORED PROCEDURE
--   assert_equal_msg(value BOOLEAN, msg TEXT)
--
-- DESCRIPTION
--   Raises a NOTICE upon success and an EXCEPTION in case of an error
--
-- PARAMETERS
--   @value
--     * RAISE an NOTICE when TRUE, RAISE an EXCEPTION when FALSE
--   @msg
--     * Message will be added upon RAISE of event
--
-- RETURN VALUE
--   VOID
--
```

## Immutability of tuples
DB tuples are immutable! In general this means we do not update tuples - instead we add a new tuple, with an incremented revision number (but the same item_id).
