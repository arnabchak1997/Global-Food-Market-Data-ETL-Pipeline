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
The FastAPI application would load the raw data to Kinesis stream which will stream it to Firehose , Firehose processed these logs in real-time, using AWS Lambda to convert them into CSV format for improved readability and compatibility. The transformd csv data was loaded to another S3 bucket by the Firehose. 

<br>





