
In MySql/Workbench style:

```sql

-- Table creation
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50)
);

-- View creation
CREATE VIEW active_employees AS
SELECT id, name
FROM employees
WHERE department = 'IT';
```

## MS Sql Server

```sql

-- Table creation
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);

-- Insert data
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

-- Update
UPDATE table_name
SET column1 = value1
WHERE condition;


-- Delete
DELETE FROM table_name
WHERE condition;

-- View creation
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;

-- Drop View
DROP VIEW view_name;




ALTER TABLE [DatabaseName].[mapping].[GroupRetailUnit]

-- Adding new columns
ADD [ColName] VARCHAR( 25 ) NULL;
ADD [ColName] BIT NOT NULL DEFAULT 0;

-- Changing existing columns
ALTER COLUMN [ColName] TINYINT;
DROP COLUMN [ColName];

```

```sql
-- Get col names
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE 0=0 
AND table_schema = 'upload' -- 'mapping'
AND table_name = 'TableName'


-- Substring search
SELECT TOP (100)
FROM [DatabaseName].[upload].[TableName]
WHERE Thing like '%substring%'
ORDER BY [Thing] DESC

-- Update values
UPDATE [DatabaseName].[upload].[TableName]
SET Thing = 1
WHERE 0=0
AND ... some args


SELECT
	TABLE_SCHEMA, COLUMN_NAME, DATA_TYPE, COLUMN_DEFAULT, IS_NULLABLE 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_schema = 'upload'
AND table_name = 'GeoHierarchyMappingV1'
AND COLUMN_NAME IN ('State', 'IsRCM', 'InnVisionProper')
UNION
SELECT
	TABLE_SCHEMA, COLUMN_NAME, DATA_TYPE, COLUMN_DEFAULT, IS_NULLABLE 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_schema = 'mapping'
AND table_name = 'GroupRetailUnit'
AND COLUMN_NAME IN ('State', 'IsRCM', 'InnVisionProper')
;
```
