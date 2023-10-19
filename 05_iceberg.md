# Iceberg Capabilities 

In this phase: 
   * We will test some cool features of Iceberg in Impala including in-place Table Evolution features - Partition & Schema, and Time Travel
   * Modify the CML Project `Canceled Flight Prediction` to use the airlines data from the Data Lakehouse instead of the data that is included with the AMP that was deployed.  This simulates how you can use the AMPs to get a starting point and modify to fit your use case.


## Prerequisites

1. Please ensure that you have completed [00_prereqs](00_prereqs.md) to deploy the Applied Machine Learning Prototype (AMP) for `Canceled Flight Prediction`.
2. Please ensure that you have completed [01_ingest](01_ingest.md#01_ingest) to ingest the data needed for Visualizations.


## Lab 1: Partition Evolution 

1. Open HUE - SQL Editor

2. 

3. **In-place Partition Evolution Feature**

```
ALTER TABLE ${prefix}_airlines.flights
SET PARTITION spec ( year, month );

SHOW CREATE TABLE ${prefix}_airlines.flights;
```

   * In the output - PARTITIONED BY SPEC is now updated with year and month
   * The data that was currently written to the flights table is **NOT** rewritten
   * When new data is written to this table it will use the new partition to write the data

4. Load Data into Iceberg Table using **NEW** Partition of year and month

```
INSERT INTO ${prefix}_airlines.flights
 SELECT * FROM ${prefix}_airlines_raw.flights
 WHERE year = 2007;
```

2. Explain plan for the following query for year=2006 which will be using the first partition we created on `year`

```
EXPLAIN PLAN
SELECT year, month, count(*) 
FROM ${prefix}_airlines.flights
WHERE year = 2006 AND month = 12
GROUP BY year, month
ORDER BY year desc, month asc;
```

![Original Partition Scans Year](images/.png)
   * In the output you will see that the entire year of 2006 data needs to be scanned, which is ??MB

3. Explain plan for the following query for year=2007 which will be using the **new** partition we created with `year` and `month`

```
EXPLAIN PLAN
SELECT year, month, count(*) 
FROM ${prefix}_airlines.flights
WHERE year = 2007 AND month = 12
GROUP BY year, month
ORDER BY year desc, month asc;
```
![New Partition Leads to Partition Pruning](images/.png)
   * In the output you will see that just the month of December for year 2007 needs to be scanned, which is approximately 11MB


## Lab 2: Schema Evolution 

1. Open HUE - SQL Editor

2. 


## Lab 3: Time Travel

1. Run the
```
DESCRIBE HISTORY ${prefix}_airlines.flights;
```
2. 
-- SELECT DATA USING TIMESTAMP FOR SNAPSHOT
SELECT year, count(*) 
FROM ${user_id}_airlines.flights
  FOR SYSTEM_TIME AS OF '${create_ts}'
GROUP BY year
ORDER BY year desc;

-- SELECT DATA USING TIMESTAMP FOR SNAPSHOT
SELECT year, count(*) 
FROM ${user_id}_airlines.flights
  FOR SYSTEM_VERSION AS OF ${snapshot_id}
GROUP BY year
ORDER BY year desc;

2. 


## Lab 4: Use Data Lakehouse to re-train Model

1. Open CML - SQL Editor

2. Open Project `Canceled Flight Prediction`

3. Click on Files

4. Click on the directory `code`

5. Click on the file `0`

