# Iceberg Capabilities 

In this phase: 
   * We will test some cool features of Iceberg in Impala including in-place Table Evolution features - Partition & Schema, and Time Travel
   * Modify the CML Project `Canceled Flight Prediction` to use the airlines data from the Data Lakehouse instead of the data that is included with the AMP that was deployed.  This simulates how you can use the AMPs to get a starting point and modify to fit your use case.


## Prerequisites

1. Please ensure that you have completed [00_prereqs](00_prereqs.md) to deploy the Applied Machine Learning Prototype (AMP) for `Canceled Flight Prediction`.
2. Please ensure that you have completed [01_ingest](01_ingest.md#01_ingest) to ingest the data needed for Visualizations.

## Lab 1: Schema Evolution 

1. Open HUE - SQL Editor
steps...

2. In the table browser viewer (left side of screen) navigate to your `<prefix>_airlines` database
   * Click `<` to the left of `default` database

   * In the list under `Databases` click on your `<prefix>_airlines` database
      * If you don't see your database, click the refresh button to the right of `Databases`


3. Click `airlines` table under `Tables` to see columns in this table
   * The current table has 2 columns: `code` and `description`


4. **In-place Schema Evolution Feature**

```
ALTER TABLE ${prefix}_airlines.airlines ADD COLUMNS(status STRING, updated TIMESTAMP);
```
   * The existing table data is **not modified** with this statement

5. Refresh table browser to see new columns added
   * Click on the refresh button to the right of `Tables`
   * Click `airlines` table to see the new columns: `status` and `updated`


6. Add data into the new schema for `airlines` table

```
INSERT INTO ${prefix}_airlines.airlines
VALUES("Z999","Adrenaline Airways","NEW",now());
```

7. Query `airlines` table to see old and new schema data

```
SELECT * FROM airlines WHERE code > "Z";
```
   * As you scroll through the results you will see the 2 columns that we added will contain "NULL" values for the data that was already in the table and the new record we inserted will have value in the new columns `status` and `updated`


## Lab 2: Partition Evolution 

1. Remain in the SQL Editor

2. See current Partition details

```
DESCRIBE FORMATTED ${prefix}_airlines.flights;
```


3. **In-place Partition Evolution Feature**

```
ALTER TABLE ${prefix}_airlines.flights
SET PARTITION spec ( year, month );

DESCRIBE FORMATTED ${prefix}_airlines.flights;
```

   * In the output - PARTITIONED BY SPEC is now updated with year and month
   * The data that was currently written to the flights table is **NOT** rewritten
   * When new data is written to this table it will use the new partition to write the data

4. Load Data into Iceberg Table using the **NEW Partition** of year and month

```
INSERT INTO ${prefix}_airlines.flights
 SELECT * FROM ${prefix}_airlines_raw.flights
 WHERE year = 2007;
```

5. Check to see the new data for year=2007 has loaded

```
SELECT year, count(*) 
FROM ${prefix}_airlines.flights
GROUP BY year
ORDER BY year desc;
```
   * You will see I didn't have to do anything special to the SQL, u

6. Explain plan for the following query for year=2006 which will be using the first partition we created on `year`

```
EXPLAIN
SELECT year, month, count(*) 
FROM ${prefix}_airlines.flights
WHERE year = 2006 AND month = 12
GROUP BY year, month
ORDER BY year desc, month asc;
```

![Original Partition Scans Year](images/.png)
   * In the output you will see that the entire year of 2006 data needs to be scanned, which is ~127MB

3. Explain plan for the following query for year=2007 which will be using the **new** partition we created with `year` and `month`

```
EXPLAIN
SELECT year, month, count(*) 
FROM ${prefix}_airlines.flights
WHERE year = 2007 AND month = 12
GROUP BY year, month
ORDER BY year desc, month asc;
```
![New Partition Leads to Partition Pruning](images/.png)
   * In the output you will see that just the month of December for year 2007 needs to be scanned, which is approximately 11MB
   * What happened in this query is we were able to leverage partition pruning to eliminate all of the partitions outside of `year=2007 and month=12` (ie. all data in years 1995 to 2006 and all months in 2007, except December).  This can lead to considerable gains in performance


## Lab 3: Time Travel

1. Use `DESCRIBE HISTORY` to bring back details for all Snapshots that have been created

```
DESCRIBE HISTORY ${prefix}_airlines.flights;
```

2. Explore Time Travel using a relative Date Time

```
SELECT year, count(*) 
FROM ${user_id}_airlines.flights
  FOR SYSTEM_TIME AS OF '${create_ts}'
GROUP BY year
ORDER BY year desc;
```

3. Explore Time Travel using the Snapshot ID

```
SELECT year, count(*) 
FROM ${user_id}_airlines.flights
  FOR SYSTEM_VERSION AS OF ${snapshot_id}
GROUP BY year
ORDER BY year desc;
```

2. 


## Lab 4: Use Data Lakehouse to re-train Model

1. Open CML

2. Open Project `Canceled Flight Prediction`

3. Explore the Data Processing code used to train the `Canceled Flight Prediction` Model
   * Click on Files

   * Click on the directory `code`

5. Open the file `3_data_processing.py` in preview
   * This is the code used to pre-process the data by wrangling the data as needed into the format to train the model

6. Scroll to the bottom of the code until you see the following

`
if __name__ == "__main__":

    if os.environ["STORAGE_MODE"] == "external":
        main()
    else:
        print(
            "Skipping 3_data_processing.py because excution is limited to local storage only."
        )
        pass
`

   * From this code we can see that it is checking the Environment variable `STORAGE_MODE`

7. Scroll up to the line `def main():`
   * Within this code block  you can see that it is using Evironment Variable we set when deploying the AMP
   * There are 2 in particular the DW_DATABASE and DW_TABLE, if you remember we set these to our Data Lakehouse `flights` Iceberg table
   * Please familiarize yourself with the rest of the code to see some of the data wrangling that is done here

8. Switch to `External` mode
   * Click on Project Settings on the left nav
   * Click on the `Advanced`
   * Under `Evnrionment Variables` change the value for the `STORAGE_MODE` to `external`

9. Go
   * Click on Files

   * Click on the directory `code`

   * Open the file `3_data_processing.py` in preview

10. Start New Session - replace &lt;prefix> with your prefix you've been using
   * Name: &lt;prefix>-data-processing-session
   * Under `Runtime`
      * Editor: Workbench (however, other Editors are available)
      * Kernel: Python 3.9
      * Edition: Standard
      * Version: leave default value here
   * Enable Spark: make sure this is active, and select Spark 3.2.3 (minimum: Spark 3 is required for Iceberg functionality)
   * Resource Profile: select a larger profile, if available select the one for 4 vCPUs and 8 GiB Memory
   * Click `Start Session`

11. Click on Run > Run All
![Run Code](images/.png)

   * IN the output window on the right half of the screen, you should see 
      * The schema of the `<prefix>_airlines.flights` table after it is queried into a DataFrame
      * The number of records returned from the `flights` table, value will be over 82 million
      * The schema of the input data that will be used to train the prediction model with our Data Lakehouse data


**Conclusion:** not sure what to put here yet 

