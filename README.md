# data-analyst-Itunuoluwa
**Descriptive Analysis: Cultural Spaces in Vancouver (DAP Implementation)
Project Description**

This project focuses on designing and implementing a Data Analytic Platform (DAP) for the City of Vancouver, specifically analyzing Cultural Spaces. The goal is to process and analyze data related to cultural venues, ownership types, seating capacity, and space distribution using AWS cloud services.

**Project Title**

Understanding Cultural Spaces in Vancouver

**Project Objective**

The objective of this project is to ingest, clean, process, and analyze the cultural spaces dataset from the City of Vancouver Open Data Portal to:

•	Identify the most common ownership types of cultural spaces.

•	Assess how ownership impacts venue size and accessibility.

•	Determine seating capacity variations across different ownerships.

•	Generate insights that can support future cultural planning and investments.

**Project Requirements**

1.	Design and Implement a Data Analytic Platform
2.	Develop a cloud-based platform for data analytics using AWS
3.	Implement the five-step descriptive analysis process:
   
o	Step 1: Data Ingestion

o	Step 2: Data Profiling

o	Step 3: Data Cleaning

o	Step 4: Data Cataloging

o	Step 5: Data Summarization

**Dataset**

•	Source: City of Vancouver Open Data Portal

•	Description: The dataset contains information on cultural venues, including ownership type, seating capacity, square footage, and local area.

•	Format: CSV

**Data Fields:**

•	Cultural Space Name: Unique name of the venue

•	Ownership Type: Whether the venue is private, government, or city-managed

•	Number of Seats: Maximum seating capacity of the venue

•	Square Footage: Total space size in square feet

•	Local Area: Geographic region of the cultural space

•	Website: Official website link 

**Methodology**

Step 1: Data Ingestion

•	Process: The raw dataset was accessed and downloaded from the City of Vancouver Open Data Portal.

•	Storage: The data was uploaded to Amazon S3 (File Bucket) for structured storage.

•	Folder Structure: 

o	s3://cps-raw-itu/ → Raw data storage

o	s3://cps-trf-itu/ → Processed data storage

o	s3://cps-cur-itu/ → Final curated dataset

•	Tools Used: AWS S3, AWS Glue

AWS S3 Data Ingestion Setup
![image](https://github.com/user-attachments/assets/b064d4ac-e0f9-4cbc-8b8f-46fd3551dbdc)
Note. Screenshots from AWS console indicating successful data ingestion in S3 bucket.

Step 2: Data Profiling

•	Process: AWS Glue DataBrew was used to profile the dataset, checking for missing values, data types, and statistical summaries.

•	Findings: 

o	271 rows, 13 columns

o	Missing values in Website (9%), Local Area (3%), Ownership (1%)

o	Weak correlation between Square Footage & Seating Capacity

•	Action: Missing values were marked for correction in the cleaning stage.

Data Profiling Summary from AWS Glue DataBrew

![image](https://github.com/user-attachments/assets/95d488c1-a5a8-4413-b14e-e2016154d466)

Step 3: Data Cleaning

•	Handling Missing Values: 

o	Website: Filled with the most frequent value

o	Seats & Square Footage: Used median imputation

•	Standardization: Renamed columns for readability, e.g., Cultural_Space_Name → culturalSpaceName

•	Output Storage: 

o	CSV stored in s3://cps-trf-itu/cps-data/

o	Parquet stored in s3://cps-trf-itu/cps-data/system/

Data Cleaning Steps in AWS Glue

![image](https://github.com/user-attachments/assets/49c2dee3-00f6-46af-8944-ae0303a83d79)

Step 4: Data Cataloging

•	AWS Glue Data Catalog was used to create structured metadata storage for easy querying.

•	Table Created: cps_metrics with key attributes like: 

o	Max Seats

o	Min Square Footage

o	Report Date

o	Ownership Types

•	Crawler: An AWS Glue Crawler was used to refresh the schema whenever updates occur.

•	Partitioning: Data was partitioned by Report Date & Ownership Type for optimized querying.

AWS Glue Data Catalog Setup

![image](https://github.com/user-attachments/assets/9d928053-f6ff-49ec-baeb-87775f6102e7)

Step 5: Data Summarization

•	Summarization Pipeline: 

o	Filtered venues with ≤ 300 seats to analyze smaller cultural spaces

o	Aggregated data by ownership type

o	Extracted key insights using AWS Glue Visual ETL

•	Findings: 

o	Government-owned venues had the largest capacity (up to 2,188 seats).

o	Privately owned venues had a max seating of 650 seats, making them more suited for small-scale events.

o	Park Board/COV venues had the largest minimum space (5,000 sq. ft.), while some private venues were as small as 650 sq. ft..

Summarization Pipeline in AWS Glue Studio
![image](https://github.com/user-attachments/assets/18fe9b84-4641-4ed3-81d4-7eba6bd579bd)

AWS Cost Estimation

•	Estimated Monthly Cost: $25

•	Breakdown: 

o	S3 Glacier Instant Retrieval: $5

o	Glue DataBrew Jobs: $10

o	Glue Crawler & Queries: $10

•	Cost Optimization Plan: 

o	Reduced query frequency

o	Partitioned data for efficient storage retrieval

AWS Cost Estimation Calculation
![image](https://github.com/user-attachments/assets/f474d1af-a0b8-4dcd-a8aa-45086dbeb9d2)

Actual Discoveries (Key Insights from the Data)

1.	Government-Owned Cultural Spaces Dominate Capacity
   
o	Government-owned cultural venues have the highest seating capacity, reaching up to 2,188 seats per venue.

o	City of Vancouver-managed venues accommodate even larger audiences, with a maximum of 2,500 seats.

o	This suggests a significant public investment in large-scale cultural infrastructure.

3.	Privately-Owned Spaces are Smaller & More Intimate
   
o	Private venues rarely exceed 650 seats.

o	This makes them better suited for small-scale or niche events rather than large public gatherings.

o	The gap between private and government spaces suggests a need for mid-sized venues.

5.	Park Board / COV Venues Have the Largest Minimum Space
   
o	Public venues start at 5,000 sq. ft., while private spaces can be as small as 650 sq. ft..

o	This indicates that privately-owned spaces operate in a more constrained physical environment, which may limit accessibility for larger audiences.

7.	Cultural Infrastructure Funding is Unevenly Distributed
8.	
o	Vancouver’s Cultural Spaces Grant Program provides up to $250,000 per project for expansion, acquisition, or renovation of arts and cultural facilities.

o	However, most of these grants favor publicly-owned venues.







