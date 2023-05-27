# Context

This project was made during a workshop provided by [The Plumbers](https://theplumbers.com.br/) and I did it along with the instructor. The goal here is to use Modern Data Stack such as: Airbyte, Snowflake and DBT to create a data pipeline, also I used MongoDB as a source for the data. The data pipeline is partitioned as follows:

 * MongoDB: Source containing the JSON files;
 * Airbyte: As a EL tool 
 * Snowflake: As a DataWarehouse 
 * DBT: To help with the SQL transformations, and data lineage as well 

# Technologies

This section focus on explain better about the tecnologies used in this project.

## MongoDB

MongoDB is a noSQL databased document oriented which means that it stores data in the semi-structured or unstructured way, instead of table as a "traditional" database. By doing so, it gain:

 * Schema Flexibility: Meaning that it hasn't a fixed schema and each document have it own structured even within a collection. In other words, a document can have one specific field and the others don't.
 * Horizontal scalability: This type of databased is designed to scale horizontally, meaning that it can be distributed along differents nodes, gaining fault tolerance and high performance. A perfect match for a Big Data environment!

## Airbyte

A open source data integration platform designed to simplify the process of extract and load data from different sources. Basically it's a tool that works only for the E and L from ETL. It has connectors that works as a "connector as a code", meaning that there's no need for writing code in order to connect the source and destination, the UI do it for you. 

Airbyte has other features such as: Scheduling and Orchestration; monitoring and alearting. The first one means that you can schedule a data and/or hour to run your pipeline and the tool runs for you. The second indicates that Airbyte will monitoring your pipeline and alert you in case of failture.

## Snowflake

A cloud based datawarehouse that provides a scalable, flexible, and high-performance solution for storing and process data. Snowflake, nowadays, it's what's called a Modern Data Warehouse, instead of only store data it can scale it's computation process(since it's cloud based), it competes with redshift, databricks, and synapse.

Snowflake's solution can provide multi-cluster data access, helping teams to work on the same data(eliminating the need to copy data); cloud native architecture meaning that it was created to run on the major cloud providers; and a ecosystem integration, indicating that can be used along with different BI tolls.

## DBT

DBT or (Data Build Tool) is a command line tool and workflow engine created to transforming and modelling data in a data warehouse, as said before DBT focus only in the T of ETL(or ELT). What is fascinating in this tool is that it runs on top of the datawarehouse that it's connected(in this case Snowflake), and all the transformation made on the data, will automatically be replicated into snowflake's tables. 

DBT also has a data lineage feature, indicating that you can check the previous and next tables that your currently table is attached to. In the real world this tool can be very helpful, considering that a company has lots of tables.

# Project

As said above the goal here is to show a Modern Data Pipeline working from scratch. Bearing that in mind, the project can be stated as: 

<img width="797" alt="Screen Shot 2023-05-27 at 14 37 15" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/64201e66-f8ac-40e0-b795-60cf62c68287">

First I have a mongoDB collection with the data from JSON Files

<img width="1389" alt="Screen Shot 2023-05-27 at 14 39 17" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/ad3e0cd7-eeaa-40d0-b1b3-30756defa471">

In this project only the subscription, user and payments collections will be used. 

Second Airbyte, and here there's need only for create a connector that can move data from mongoDB to Snowflake

<img width="1322" alt="Screen Shot 2023-05-27 at 14 42 48" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/2788f04e-3731-485c-816a-d9ba8ba7fdca">

With this connection made, the next step is check in snowflake if the data had been delivered

<img width="1161" alt="Screen Shot 2023-05-27 at 14 45 22" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/95dd9c0d-fa6b-42aa-9a12-9ce0d3d62dce">

The cool thing here is notice that I have extracted a JSON file(hence, unstructured), but when it loads to snowflake the data is already in tabular format(although some columns here received a dictionary, because of nested documents), also we can see files the begin with "airbyte", this files are metadata from airbyte. 

Ok, now moving on to DBT:

<img width="992" alt="Screen Shot 2023-05-27 at 14 52 12" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/5f5d8c6c-dc3a-4999-a47c-32c307ed55c2">

This image shows a SQL query being used to create a view using a previous table as base. Is interested to notice that in the first 2 lines I'm configuring this query to return a view. Also there's a YAML file that I can use to test the data and once the test does not pass the DBT scheduler will notice me.

<img width="1073" alt="Screen Shot 2023-05-27 at 14 57 21" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/793fe49b-8ecb-4b04-a40c-df09bec6886e">

We can see that I'm checking for null values in the column "credit_card_type" and in other table ("user") I'm checking for accepted_values in the gender column.

Finally, once this data has runned I can see it in snowflake(as I stated before, DBT is only a shell)

<img width="1151" alt="Screen Shot 2023-05-27 at 14 59 51" src="https://github.com/LucasbFontes/modern_data_stack/assets/68716835/b00d20a3-74b8-414c-bacd-6f0294ee8466">

The data is now cleaned, and ready to be served to the analytics team / data science team!
