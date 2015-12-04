# SQL/PLSQL
This style guide is a list of *dos* and *don'ts* for SQL/PLSQL related code.

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Naming things](#naming-things)
1. [Documenting code](#documenting-code)

## General
* capitalize reserved words (CREATE, FUNCTION, INDEX, TABLE, SELECT, DELETE etc.), datatypes (INTEGER, BIGINT, STATE, etc.)
* main keywords on new line
* always use short meaningful table names, attrs etc.
* don't prefix tables
* table names singular
* group related statements together

Example:
```
DROP TYPE IF EXISTS STATE;
CREATE TYPE STATE AS ENUM ('CHANGE', 'VALID', 'DELETE');

DROP SEQUENCE IF EXISTS page_item_id;
CREATE SEQUENCE page_item_id START 1;

DROP TABLE IF EXISTS page;
CREATE TABLE page (
  page_id BIGSERIAL NOT NULL PRIMARY KEY,
  item_id SMALLINT NOT NULL DEFAULT 1,
  revision SMALLINT NOT NULL DEFAULT 0,
  title TEXT NOT NULL,
  state state NOT NULL DEFAULT ('CHANGE')::STATE,
  copiedfrom BIGINT,
  createdby BIGINT NOT NULL, 
  responsibleauthor BIGINT NOT NULL, 
  datereviewed TIMESTAMP WITH TIME ZONE,
  datecreated TIMESTAMP WITH TIME ZONE
) WITH ( OIDS=FALSE );

ALTER TABLE page ADD CONSTRAINT unique_page_item_revision UNIQUE (item_id, revision);

-- Note: convert indices to lower case for case-insensitice search
CREATE INDEX index_page_title ON page USING btree (lower(title));

CREATE INDEX index_page_state ON page USING btree (state);

CREATE OR REPLACE FUNCTION convertPageVersionState(integer) RETURNS STATE AS $$
  BEGIN
    CASE $1
      WHEN 0 THEN
        RETURN ('CHANGE')::STATE;
      WHEN 1 THEN
        RETURN ('CHANGE')::STATE;
      WHEN 2 THEN
        RETURN ('VALID')::STATE;
      WHEN 3 THEN
        RETURN ('CHANGE')::STATE;
      WHEN 4 THEN
        RETURN ('CHANGE')::STATE;
      ELSE
        RETURN ('DELETE')::STATE;
    END CASE;
  END;
$$ LANGUAGE plpgsql;

TRUNCATE TABLE page;
INSERT INTO
  page
SELECT
  id AS page_id,
  nextval('page_item_id') AS item_id,
  majorversion AS revision,
  title,
  convertPageVersionState(state) AS state,
  copiedfrom,
  changedby,
  responsibleauthor,
  reviewinterval,
  datereviewed,
  datecreated
FROM
  guidelines.guidelines_pages;

DROP FUNCTION IF EXISTS convertPageVersionState(integer);
```

## Code Formatting
* Indent with 2 spaces
* Add new line at end of files
* Clean up trailing whitespaces at end-of-line

## Naming things
### Custom Datatypes
Use upper case for custim data types:
```
CREATE TYPE STATE AS ENUM ('CHANGE', 'VALID', 'DELETE');
```

### Table
* Singular name
* No prfixes like "guideline_..."
* Table name for M:N relations -> M_N (eg. page_detail)

### Constraint
* Prefix UNIQUE constraints with 'unique':
```
ALTER TABLE page_detail ADD CONSTRAINT unique_page_detail_revision UNIQUE (page_id, detail_id, revision);
```

* Prefix INDEX constraints with 'index':
```
CREATE INDEX index_page_detail_title ON page_detail USING btree (lower(title));
```

### Key
* Suffix primary keys with '_id':
```
...
page_detail_id BIGSERIAL NOT NULL PRIMARY KEY,
...
```

* Use same attribute name for freign keys in referencing table as referenced table:
```
...
page_id BIGINT NOT NULL REFERENCES page ON DELETE CASCADE,
detail_id BIGINT NOT NULL REFERENCES detail ON DELETE CASCADE,
...
```
###PLPGSQL/PLV8 Function and Variable
Lowercase, words seperated with '_'
```
DROP FUNCTION IF EXISTS get_current_revision_of_page(has_id INTEGER);
CREATE FUNCTION get_current_revision_of_page(has_id INTEGER) RETURNS INTEGER AS $$
  SELECT COALESCE((SELECT revision FROM page WHERE item_id = (SELECT item_id FROM page WHERE page_id = has_id) ORDER BY revision DESC LIMIT 1), 0)
$$ LANGUAGE SQL;
```

####Function Name
* Prefix 'assert_' for basic 'unit test like' functions
* Prefix 'test_' for 'test' functions

## Documenting Code
* Use standard SQL comments like '-- '.
* Use the following doc block for functions:
```
--
-- STORED PROCEDURE
--   get_current_revision_of_detail_item_with_state(has_id INTEGER, has_state STATE)
--
-- DESCRIPTION
--   Retrieve latest revision of a detail item which has a specific state (ENUM('CHANGE', 'VALID', 'DELETE'))
--
-- PARAMETERS
--   @has_id
--     * id of page
--   @has_state
--     * filter state
--
-- RETURN VALUE
--   INTEGER
--
-- Example
--   SELECT get_current_revision_of_page_with_state(53, ('VALID')::STATE) AS REVISION
--
```
