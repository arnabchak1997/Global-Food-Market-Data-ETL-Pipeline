# Global-Food-Market-Data-ETL-Pipeline

## Summary: <br>
The aim of this project is to analyze real-time global market data using AWS Kinesis and Snowflake. We utilize CSV datasets extracted via API calls, stream them through Kinesis Firehose, and transform them with Snowflake. Our agile workflow ensures
efficiency, providing a one stop comprehensive solution for real-time data insights. AWS services include S3 for storage, Kinesis Firehose for streaming, Lambda for serverless
computing, AppRunner for deployment of API, Snowpipe for auto ingestion of raw data from destination S3 bucket, Snowpark for processing raw data into cleaned data and
Power BI for data visualization.
<br>

## Dataset Description: <br>
The dataset constitutes a comprehensive view of the global food market across 36 countries and 2,200 markets spanning from January 1, 2007, to May 1, 2023. It includes
detailed information on various aspects such as country specifics, market details, and diverse food items with associated price points (low, high, open, close), inflation rates, and trust indicators for each item.
<br>

## Tech Stack: <br>
Programming : Python (pandas), SQL <br>
AWS Services : AWS Kinesis, AWS App Runner, AWS Kinesis Firehose, AWS S3, AWS LAMBDA <br>
Warehouse: Snowflake <br>
Visualization : Power BI <br>
<br>

## Architecture: <br>
![image](https://github.com/user-attachments/assets/19ece9f3-8a56-43e0-b94c-063a65d10f4e)
<br>

## Execution Steps: <br>

### Creaton of Fast API for data extraction: <br>
The dataset was initially stored in an AWS S3 bucket for efficient storage and accessibility.A FastAPI application was created using AWS App Runner service to establish an interface for accessing the data securely in S3 bucket. In order to access the data securely, the AWS access key and secret key id were passed as environment variables in the API service configuration. The API takes input as year, country and market. The code and requirements file for the FastAPI application is present in below repo.<br>
https://github.com/arnabchak1997/food-streaming-api-repo
<br>

### Creation of ingestion script to load data to Kinesis:<br>
For fetching the data from API and loading to kinesis , we executed a python script in Google Collab which created a kinesis client , fetched the raw data from the API application endpoint and loaded to the stream using the kinesis client. <br>

### Setting up of Kinessis Stream, Firehose, Lambda function: <br>
The FastAPI application would load the raw data to Kinesis stream which will stream it to Firehose , Firehose processed these logs in real-time, using AWS Lambda to convert them into CSV format for improved readability and compatibility. Following data processing by AWS Kinesis Firehose, a new S3 bucket was designated as the destination for storing transformed data. Kinesis Firehose then automatically loaded the processed data into this bucket, ensuring smooth and uninterrupted data flow.
<br>

### Setting up Snowflake: <br>
The csv data from S3 bucket was loaded to snowflake table which we named as RAW DATA Table, for that an external stage was created pointing to the S3 bucket file location. Snowpipe, an automated data ingestion service in Snowflake, was configured to ingest raw data directly from the specified S3 bucket to the RAW DATA Table. 

### Trnasformation in snowflake: <br>
Snowpark sessions in Python were utilized to cleanse and transform raw data, preparing it for insertion into TRANSFORMED DATA Snowflake table. 
The raw data table was converted to a simpler table with columns "COUNTRY", "MARKET_DATES", "DATES", "ITEM", "OPEN","CLOSE", "LOW", "HIGH", "INFLATION", "TRUST". Another table was created to keep track of the row count and index so that only the incoming data in the raw data table is transformed and loaded to the final transformed data table. 
The python code was deployed as stored procedure so that it can be called when the transformation is required. To automate the transformation , a snowlflake task was created whch will call the stored procedure at a particular time everyday (12 PM UTC in this project). During the execution of the project, it was observed that for some loads, the default virtual warehouse was not able to process the transformation and running out of memory. In that case, a snowpark optimized warehouse was created which will run the transformation.
<br>

### Loading data into PowerBI and performing visualization: <br>
The final transformed data table was loaded to PowerBI for visualization purpose, below are the visualizations created from the transformed data. <br>

1. General overview of the countries , markets and items in tablular format like country and number of items and markets, country and markets, country and items. The country, market and items can be choosen using a slicer.
2. Candle stick charts for High-low comparison and inflation-trust comparisons, the charts will give comparsion for a sigle country, market and item at a time.
3. Multiple countries, markets and items comparison based on open, close, high, low, inflation and trust. For these visualizations, a custom filter was created using the average values for the open , close, high, low, inflation and trust parameters.







