## MS Sql Server
```sql
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
