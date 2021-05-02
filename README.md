# A tech case for the Dennemeyer Octimine Team


The challenge below is meant to assess skills that we think are crucial for this data engineering position at CareerFoundry.

## General info
1 The technical case
We have a shared folder on FTP server where our data provider every week uploads patent data
updates in XML format (Each document is represented as <exch:exchange-document>).
Example package of such files can be found here:
https://drive.google.com/file/d/1u0BNdA2xIRF_FzovAXYGS-wCkrchcy46/view?usp=sharing
Please design a proof-of-concept for a data processing pipeline to parse the input files and
import them into relational database for further analysis and processing.
Elements to extract from the input files:
• country
• doc-number
• kind
• doc-id
• date-publ
• family-id
• classification-ipcr (only text)
• exch:applicant-name (only name)
• exch:invention-title
• exch:abstract
Output:
• Relational tables in the database (feel free to define the schema as you see fit).

## Technologies
* jupyter notebook
* pandas dataframe
* GCP cloud function 
* cloud scheduler 

## Setup
Install python3 and jupyter notebook to run the notebooks. To access BiqQuery table, you have to specify inside code: 
    PROJECT_ID = os.getenv("GCP_PROJECT")
    BQ_DATASET = "your_dataset_name"
    BQ_TABLE = "your_Preferred_table_name"


## Code Examples
- PROJECT_ID = os.getenv("GCP_PROJECT")
- print(PROJECT_ID)
- BQ_DATASET = "test_dataset"
- BQ_TABLE = "patentDocument"

## Jupyter Notebook
Jupyter notebook has been created for testing purpose. you can examining how data has been extracted and transformed. I also added some Statistical Insights on tranformed dataframe.

## Deployment on GCP as a Cloud Function 
You can download the zip source code "function-source.zip" and upload it into your cloud function 

We can use the above code together with function cloud_function_entry_point as cloud function to automate the extract, transform and load process as cloud function
![xml_bq_workflow](https://user-images.githubusercontent.com/11519103/116811736-1ea5a080-ab4b-11eb-8878-4c262f42cafa.jpg)


* step 1. Create cloud function
* step 2. populate the requirments.txt with required packages
* step 3. enable http trigger type
* step 4. allow Authentication
* step 5. increase memory allocated to 2GB
* step 6. integrate code by uploading zip folder or paste all code inline Editor
** step 6.1. set entry point cloud_function_entry_point
* step 7. add packages in requirement.txt
* step 8. deploy the function
* step 9. test the function
* step 10. check the table schema and data in biqQuery table

The cloud function can be scheduled using Cloud Scheduler to trigger the cloud function on defined scheduled

    scheduling requires unix-cron format.
    
##  Other Deployment Option

### Using Airflow DAG

As an alternate approach, we can create Dag and deploy the DAG in Google Cloud composer. The DAG can be triggered using cloud function or defined schedule
Deployment workflow will be as follows:

    - extract data from API
    - store data as a raw data in **raw_zone_storage_bucket**
    - extract data from **raw_zone_storage_bucket** and transform the data
    - store the process data into a **clean_zone_storage_bucket** as parquet file format
    - create **bigQueryExternalTable** for the stored parquet file format
    - the bigqueryExternalTable keep only reference to the parquet file not the actual data
    - Only query will charge in this case

AirFlow DAG Tasks

    - extract data from API using  **airflow.operators.http_operator**
    - transform data by using python code with  **airflow.operators.python_operator**
    - load data in to BigQuery using **airflow.providers.google.cloud.operators.bigquery**
    - 
## Status
Project is: _finished_, 

## Inspiration
I would like to thanks Dennemeyer Octimine team for sending such an interesting task to worked on

## Contact
* Created by Sahib Jan [jaanhunzai.512@gmail.com]- feel free to contact me!
* submitted on: 01.05.2021
