/****** Script for SelectTopNRows command from SSMS  ******/
SELECT TOP (10) *
  FROM [InnVision_DEV].[mapping].[GroupRetailUnit]

## ALTERs
```
ALTER TABLE [InnVision_DEV].[mapping].[GroupRetailUnit]
ADD [State] VARCHAR( 25 ) NULL;

ALTER TABLE [InnVision_DEV].[mapping].[GroupRetailUnit]
ADD [IsRcm] BIT NOT NULL DEFAULT 0;

ALTER TABLE [InnVision_DEV].[mapping].[GroupRetailUnit]
ALTER COLUMN [IsRcm] TINYINT;

ALTER TABLE [InnVision_DEV].[mapping].[GroupRetailUnit]
DROP COLUMN [InnVisionProper];

```

```
/****** Script for SelectTopNRows command from SSMS  ******/


SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE 0=0 
AND column_name = 'State'
AND table_schema = 'upload'
AND table_name = 'GeoHierarchyMappingV1'


-- ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
-- ADD COLUMN [State] VARCHAR(25) NULL AFTER [ZoneSize],
-- ADD [IsRcm] BIT NOT NULL DEFAULT 0 AFTER [State],
-- ADD [InnVisionProper] BIT NOT NULL DEFAULT 0  AFTER[IsRcm];


-- ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
-- DROP COLUMN State;


/*
ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
ADD [State] VARCHAR( 25 ) NULL;
ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
ADD [IsRcm] BIT NOT NULL DEFAULT 0;
ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
ADD [InnVisionProper] BIT NOT NULL DEFAULT 0;
*/

ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
ALTER COLUMN [IsRcm] TINYINT;

ALTER TABLE [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
ALTER COLUMN [InnVisionProper] TINYINT;

SELECT 
TOP (100)
*
  FROM [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
  WHERE Version like '%03-20%'
ORDER BY [Version] DESC
;


SELECT * FROM [InnVision_DEV].[upload].[GeoHierarchyMappingV1]
WHERE 0=0
AND Version = 'Spectrum_Mapping2_17_2022'
AND GroupRetailUnitCode = 'SLWC'
AND EclipseInstance = 'CHCP'
;

UPDATE 
[InnVision_DEV].[upload].[GeoHierarchyMappingV1]
SET IsRcm = 1
WHERE 0=0
AND Version = 'Spectrum_Mapping2_17_2022'
AND GroupRetailUnitCode = 'SLWC'
AND EclipseInstance = 'CHCP'
;
```

```
CREATE OR ALTER PROCEDURE [upload].[CompareGeoHierarchyMappingV1]
	@Version1 varchar(50) = '',
	@Version2 varchar(50) = ''
AS
/*
############################################################################# 
Name					: upload.CompareGeoHierarchyMappingV1
Description		: 
Author        : Innovar
Created			  :
Last Modified	: 10/13/2024
-----------------------------------------------------------------------------
Log:
-----------------------------------------------------------------------------

#############################################################################
*/
BEGIN

SET NOCOUNT ON;

 --DECLARE @Version1 varchar(50);
 --DECLARE @Version2 varchar(50);

 --SET @Version1 = '';
 --SET @Version2 = 'Spectrum-Mapping_Master_11-27-18';

 IF OBJECT_ID('tempdb..#temp1') IS NOT NULL DROP TABLE #temp1;
 IF OBJECT_ID('tempdb..#temp2') IS NOT NULL DROP TABLE #temp2;

 SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [NielsenDMA]
	, [DMACD]
	, [MktCD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStart]
	, [EffectiveStop]

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
INTO #temp1
FROM upload.GeoHierarchyMappingV1
WHERE 1 = 0;

SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [NielsenDMA]
	, [DMACD]
	, [MktCD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStart]
	, [EffectiveStop]

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
INTO #temp2
FROM upload.GeoHierarchyMappingV1
WHERE 1 = 0;

 IF @Version1 <> ''
 BEGIN
 INSERT INTO #temp1
SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [NielsenDMA]
	, [DMACD]
	, [MktCD]
	-- , [HistoricalMVPD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStart]
	, [EffectiveStop]

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
FROM
	[upload].[GeoHierarchyMappingV1]
WHERE
	[Version] = @Version1
AND
	EclipseInstance in (SELECT EclipseInstance FROM etl.vwEclipseInstance WHERE isShowInApp = 1)
 END
 ELSE
 BEGIN

 INSERT INTO #temp1
 SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [Nielsen_DMA] AS NielsenDMA
	, [DMA_CD] AS DMACD
	, [MktCD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStartDate] as EffectiveStart
	, [EffectiveStopDate] as EffectiveStop

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
FROM
	[mapping].[GroupRetailUnit]
WHERE
	1 = 1
AND
	EclipseInstance in (SELECT EclipseInstance FROM etl.vwEclipseInstance WHERE isShowInApp = 1)
 END

 IF @Version2 <> ''
 BEGIN
 INSERT INTO #temp2
SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [NielsenDMA]
	, [DMACD]
	, [MktCD]
	-- , [HistoricalMVPD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStart]
	, [EffectiveStop]

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
FROM
	[upload].[GeoHierarchyMappingV1]
WHERE
	[Version] = @Version2
AND
	EclipseInstance in (SELECT EclipseInstance FROM etl.vwEclipseInstance WHERE isShowInApp = 1)
 END
 ELSE
 BEGIN
 INSERT INTO #temp2
 SELECT
	[EclipseInstance]
	, [GroupType]
	, [GroupRetailUnitCode]
	, [GroupRetailUnitName]
	, [GroupRetailUnitSysCode]
	, [Nielsen_DMA] AS NielsenDMA
	, [DMA_CD] AS DMACD
	, [MktCD]
	, [PartnerName]
	, [SalesGeo]
	, [TimeZone]
	, [TerritoryName]
	, [MarketName]
	, [MarketSize]
	, [ZoneSize]
	, [GeoRegionName]
	, [DivisionName]
	, [EffectiveStartDate] as EffectiveStart
	, [EffectiveStopDate] as EffectiveStop

	
    , [State]
    , [IsRcm]
    , [InnVisionProper]
FROM
	[mapping].[GroupRetailUnit]
WHERE
	1 = 1
AND
	EclipseInstance in (SELECT EclipseInstance FROM etl.vwEclipseInstance WHERE isShowInApp = 1)
 END


;WITH
add_cte
AS
(
SELECT
	*
FROM
	#temp2
EXCEPT
SELECT
	*
FROM #temp1
)
, delete_cte
AS
(
SELECT
	*
FROM
	#temp1
EXCEPT
SELECT
	*
FROM
	#temp2
)
,
total_cte
AS
(
SELECT
'DELETE' as Action
, *
FROM delete_cte a
WHERE
NOT EXISTS(SELECT 1 FROM #temp2 m WHERE a.[EclipseInstance] = m.EclipseInstance AND a.GroupRetailUnitCode = m.GroupRetailUnitCode AND ((a.EffectiveStart is null AND m.EffectiveStart is null) or (a.EffectiveStart = m.EffectiveStart)))
UNION
SELECT
'CHANGE' as Action
, *
FROM add_cte a
WHERE
EXISTS(SELECT 1 FROM #temp1 m WHERE a.[EclipseInstance] = m.EclipseInstance AND a.GroupRetailUnitCode = m.GroupRetailUnitCode AND ((a.EffectiveStart is null AND m.EffectiveStart is null) or (a.EffectiveStart = m.EffectiveStart)))
UNION
SELECT
'ADD' as Action
, *
FROM add_cte a
WHERE
NOT EXISTS(SELECT 1 FROM #temp1 m WHERE a.[EclipseInstance] = m.EclipseInstance AND a.GroupRetailUnitCode = m.GroupRetailUnitCode AND ((a.EffectiveStart is null AND m.EffectiveStart is null) or (a.EffectiveStart = m.EffectiveStart)))
UNION
SELECT
'MASTER' as Action
, *
FROM #temp1
)
SELECT
*
FROM
total_cte
order by 1 desc

IF OBJECT_ID('tempdb..#temp1') IS NOT NULL DROP TABLE #temp1;
 IF OBJECT_ID('tempdb..#temp2') IS NOT NULL DROP TABLE #temp2;
END
```

