DB/SQL
This style guide is a list of *dos* and *don'ts* for DB/SQL related code.

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
INSERT INTO page
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
Use upper case for custim data types
```
CREATE TYPE STATE AS ENUM ('CHANGE', 'VALID', 'DELETE');
```

### Tables
* Singular name
* No prfixes like "guideline_..."
* Table name for M:N relations -> M_N (eg. page_detail)

### Constraints
* ...

### Keys
* ...

## Documenting Code
...
