# data-analyst-Itunuoluwa
#Project 1- Descriptive Analysis: Cultural Spaces in Vancouver (DAP Implementation)

## Project Description

   This project focuses on designing and implementing a Data Analytic Platform (DAP) for the City of Vancouver, specifically analyzing Cultural Spaces. The goal is to process and analyze data related to cultural venues, ownership 
   types, seating capacity, and space distribution using AWS cloud services.

## Project Title

Understanding Cultural Spaces in Vancouver

## Project Objective

The objective of this project is to ingest, clean, process, and analyze the cultural spaces dataset from the City of Vancouver Open Data Portal to:

      •	Identify the most common ownership types of cultural spaces.
      
      •	Assess how ownership impacts venue size and accessibility.
      
      •	Determine seating capacity variations across different ownerships.
      
      •	Generate insights that can support future cultural planning and investments.

## Project Requirements

   1.	Design and Implement a Data Analytic Platform
   2.	Develop a cloud-based platform for data analytics using AWS
   3.	Implement the five-step descriptive analysis process:
   
         o	Step 1: Data Ingestion
         
         o	Step 2: Data Profiling
         
         o	Step 3: Data Cleaning

         o	Step 4: Data Cataloging
         
         o	Step 5: Data Summarization

## Dataset

      •	Source: City of Vancouver Open Data Portal
      
      •	Description: The dataset contains information on cultural venues, including ownership type, seating capacity, square footage, and local area.
      
      •	Format: CSV

## Data Fields:

      •	Cultural Space Name: Unique name of the venue
      
      •	Ownership Type: Whether the venue is private, government, or city-managed
      
      •	Number of Seats: Maximum seating capacity of the venue
      
      •	Square Footage: Total space size in square feet
      
      •	Local Area: Geographic region of the cultural space
      
      •	Website: Official website link 

## Methodology

### Step 1: Data Ingestion

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

### Step 2: Data Profiling

      •	Process: AWS Glue DataBrew was used to profile the dataset, checking for missing values, data types, and statistical summaries.
      
      •	Findings: 
      
      o	271 rows, 13 columns
      
      o	Missing values in Website (9%), Local Area (3%), Ownership (1%)
      
      o	Weak correlation between Square Footage & Seating Capacity
      
      •	Action: Missing values were marked for correction in the cleaning stage.

Data Profiling Summary from AWS Glue DataBrew

![image](https://github.com/user-attachments/assets/95d488c1-a5a8-4413-b14e-e2016154d466)

### Step 3: Data Cleaning

      •	Handling Missing Values: 
      
      o	Website: Filled with the most frequent value
      
      o	Seats & Square Footage: Used median imputation
      
      •	Standardization: Renamed columns for readability, e.g., Cultural_Space_Name → culturalSpaceName
      
      •	Output Storage: 
      
      o	CSV stored in s3://cps-trf-itu/cps-data/
      
      o	Parquet stored in s3://cps-trf-itu/cps-data/system/

Data Cleaning Steps in AWS Glue

![image](https://github.com/user-attachments/assets/49c2dee3-00f6-46af-8944-ae0303a83d79)

### Step 4: Data Cataloging

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

### Step 5: Data Summarization

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

User Folder in S3

![image](https://github.com/user-attachments/assets/6570de29-bf71-4bd6-9c63-461bb6066df9)

System Folder in S3

![image](https://github.com/user-attachments/assets/419a85a1-d743-4197-a9b6-8fe6ca81c4e9)



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

## Actual Discoveries (Key Insights from the Data)

1.	Government-Owned Cultural Spaces Dominate Capacity
   
      o	Government-owned cultural venues have the highest seating capacity, reaching up to 2,188 seats per venue.
      
      o	City of Vancouver-managed venues accommodate even larger audiences, with a maximum of 2,500 seats.
      
      o	This suggests a significant public investment in large-scale cultural infrastructure.

2.	Privately-Owned Spaces are Smaller & More Intimate
   
      o	Private venues rarely exceed 650 seats.
      
      o	This makes them better suited for small-scale or niche events rather than large public gatherings.
      
      o	The gap between private and government spaces suggests a need for mid-sized venues.

3.	Park Board / COV Venues Have the Largest Minimum Space
   
      o	Public venues start at 5,000 sq. ft., while private spaces can be as small as 650 sq. ft..
      
      o	This indicates that privately-owned spaces operate in a more constrained physical environment, which may limit accessibility for larger audiences.

4.	Cultural Infrastructure Funding is Unevenly Distributed
   
      o	Vancouver’s Cultural Spaces Grant Program provides up to $250,000 per project for expansion, acquisition, or renovation of arts and cultural facilities.
      
      o	However, most of these grants favor publicly-owned venues.
      
      o	Privately-owned venues receive less funding, even though they serve niche cultural needs.

5.	Ownership Type Affects Venue Accessibility

      o	Publicly-funded venues are more accessible for large events.
      
      o	Privately-owned venues, due to smaller space sizes and lower funding, may have higher rental costs, making them less accessible for grassroots cultural initiatives.

Data Insights Visualization

 ![image](https://github.com/user-attachments/assets/11acc3f6-a398-4917-8526-998144e6c48b)

##  Recommendations (Actionable Insights Based on Findings)
 
1.	Increase Funding for Mid-Sized Venues
   
      o	The gap between small private spaces and large government venues suggests the need for mid-sized cultural venues (750-1,500 seats).
      
      o	The Cultural Spaces Grant Program should allocate funding to expand and support mid-sized cultural spaces.

2.	Encourage Private-Public Partnerships (PPP)
   
      o	The city should collaborate with private venue owners to subsidize rentals for community events.
      
      o	This can make small cultural spaces more accessible to local artists and performers.

3.	Optimize Venue Utilization through Data-Driven Planning
   
      o	The City of Vancouver should leverage this data to identify underutilized venues.
      
      o	Unused or low-occupancy public spaces could be repurposed for community events instead of building new infrastructure.

4.	Support Cultural Infrastructure in Underserved Areas
   
      o	The data should be used to map cultural spaces by region.
      
      o	Investments should prioritize areas with limited cultural venues, ensuring equal access across neighborhoods.

5.	Introduce Tax Incentives for Private Cultural Spaces
   
      o	Offering tax breaks or incentives to private venue owners could help reduce operational costs and encourage more affordable event hosting.

6.	Enhance Accessibility of Data for Policy Makers & Community Groups
   
      o	Create an open-access visualization tool using AWS QuickSight or Power BI so policy-makers, researchers, and event organizers can interactively explore venue distribution, occupancy, and accessibility.

Tools & Technologies Used

   •	AWS S3 → Data storage (Raw, Processed, Curated)
   
   •	AWS Glue → Data Cataloging & ETL
   
   •	AWS Glue DataBrew → Data Cleaning & Profiling
   
   •	AWS Pricing Calculator → Cost Estimation for AWS Services
   
   •	Draw.io → Architecture Diagram

Architecture Diagram in Draw.io

![image](https://github.com/user-attachments/assets/f066f4d4-2b9f-4b1b-8d50-088c76674b07)

## Deliverables

   •	AWS Data Analytic Platform Implementation
   
   •	Architecture Diagram (Draw.io)
   
   •	Descriptive Analysis Report
   
   •	Screenshots of Implementation Steps
   
   •	AWS Cost Estimation Report

The Cultural Spaces DAP provides a comprehensive overview of Vancouver’s cultural infrastructure, highlighting key ownership, capacity, and accessibility trends.
By leveraging these findings, government and private stakeholders can make data-driven decisions to improve venue accessibility, funding allocation, and cultural policy planning.

# PROJECT 2: Cloud-Based Descriptive Analytics on Vancouver Cultural Spaces Using AWS Services

# Project Title
**Analyzing and Managing Cultural Space Data in Vancouver through AWS Athena and S3**

---

## Objective
This project aims to perform a detailed **descriptive analysis** on the cultural spaces dataset in Vancouver by leveraging AWS cloud tools. The analysis focuses on answering predefined business questions regarding the number, ownership, size, and capacity of venues. Beyond analysis, the project also implements **data security**, **governance**, **quality control**, and **monitoring**, establishing a robust cloud-based analytics pipeline.

---

## Dataset
The dataset comprises cleaned cultural space records, previously transformed using AWS Glue DataBrew and stored in Amazon S3 (`cps-trf-itu` bucket). Key fields include:

- `ownership`  
- `squarefeet`  
- `numberofseats`  
- `geom` (geometry)  
- Additional attributes used in filtering and profiling

---

## Methodology

### 1. Query Configuration and Setup (Amazon Athena + S3)
- Configured **Athena Query Result Location** to S3: `s3://cps-cur-itu/`
- Ensured all SQL results are automatically stored and versioned in S3 for auditability
  
  ![image](https://github.com/user-attachments/assets/1d164a9b-8c94-467f-8cd1-134b41564300)


### 2. Descriptive SQL Analysis – Business Questions Answered

| Business Question | Description |
|------------------|-------------|
| **Total cultural spaces** | Used `COUNT(*)` to confirm completeness of the dataset (271 spaces) |
| **Ownership distribution** | Grouped by `ownership` to identify top categories like Private (95) and City (69) |
| **Minimum venue size** | Queried `MIN(squarefeet)` – smallest venue is 600 sq ft |
| **Maximum seating capacity** | Queried `MAX(numberofseats)` – largest venue has 2,756 seats |
| **Combined insights** | Identified the most common ownership and matched with min/max capacity stats |

Total cultural spaces** | Used `COUNT(*)` to confirm completeness of the dataset (271 spaces)

![image](https://github.com/user-attachments/assets/e492ba1e-5d3b-4c4a-8950-1131709a48eb)

Ownership distribution** | Grouped by `ownership` to identify top categories like Private (95) and City (69)

![image](https://github.com/user-attachments/assets/7a60c07b-723f-4105-91a9-de79ba2872a3)

Minimum venue size** | Queried `MIN(squarefeet)` – smallest venue is 600 sq ft

![image](https://github.com/user-attachments/assets/57426461-1353-416c-b40c-512dc230f99a)

Maximum seating capacity** | Queried `MAX(numberofseats)` – largest venue has 2,756 seats

![image](https://github.com/user-attachments/assets/7b426d60-a87b-4a58-8de4-02d9319992f7)

Combined insights** | Identified the most common ownership and matched with min/max capacity stats

![image](https://github.com/user-attachments/assets/1cf36d32-e50b-4cdf-9b1a-492f026c2103)

- Compared results with Project One (which had a `numberofseats <= 300` filter), noting significant insight differences between filtered and full datasets.

### 3. Data Security Configuration (Amazon S3 + AWS KMS)
- Created **customer-managed KMS key** `cultural-spaces-key-itu`
  
![image](https://github.com/user-attachments/assets/4c36f45c-9633-42f9-bb9a-456c7053b2a1)

- Enabled **SSE-KMS encryption and bucket versioning** on raw data bucket `cps-raw-itu`

![image](https://github.com/user-attachments/assets/a92377eb-e83e-4f05-bcb4-9b57c5c61eb0)

- All uploaded data is now automatically encrypted at rest

### 4. Cross-Region Replication Setup (High Availability)
- Enabled **cross-region replication** from:
  - `cps-raw-itu` and `cps-trf-itu` (Vancouver) → US East (Virginia)
![image](https://github.com/user-attachments/assets/e865ba3d-3cd0-486a-84b4-b4c8976dd1d2)

- Configured IAM role permissions via `LabRole`
- Ensured replication includes **KMS-encrypted files**
  
![image](https://github.com/user-attachments/assets/2c91f47c-0d3c-4bf7-a8f4-44b08c9e85bd)

### 5. Data Quality Control (AWS Glue Data Quality Jobs)
- Built **AWS Glue job**: `cultural-spaces-QC-itu`
![image](https://github.com/user-attachments/assets/adca073a-54ce-4352-9f99-29fef4b01b18)

- Implemented **two rules**:
  - Completeness on `ownership` ≥ 95%
  - Uniqueness on `geom` ≥ 85%
![image](https://github.com/user-attachments/assets/04bc5a13-141a-4740-b7aa-558d1417b3fd)

- Separated results into:
  - `Passed` folder: Valid, clean records
 ![image](https://github.com/user-attachments/assets/41d9a824-9780-4d1e-911b-a9662ef2ce56)

  - `Failed` folder: Duplicates or missing geometry (esp. 2015–2017 records)
 ![image](https://github.com/user-attachments/assets/baa7be2f-412e-4db3-9d5f-f7db3dd3f333)

- Ensured QC results are stored cleanly for future transformation or exclusion
  

### 6. Monitoring and Alerts (CloudWatch + CloudTrail)
- Created **CloudWatch Alarm** `cultural-spaces-size-alm` to monitor `BucketSizeBytes`
  - Threshold: 40,000 bytes
![image](https://github.com/user-attachments/assets/947da42b-f886-4a1c-ab4d-caaeb4b9fa70)
  - Action: Email alert via **SNS** to `Itunuoluwa.moses@myucwest.ca`
![image](https://github.com/user-attachments/assets/a6a27962-0437-4fc3-8822-f0a1cc5cf13f)

- Created **CloudWatch Dashboard**: `cultural-spaces-MCR-itu`
  - Includes widgets to monitor bucket size and resource usage
![image](https://github.com/user-attachments/assets/b06ac6f3-376a-4423-bf94-9a1c47cf7af2)

- Enabled **CloudTrail multi-region logging**:
  - Trail: `cultural-spaces-trail-itu`
  - Logs stored in: `aws-cloudtrail-logs-9033-7951-7541`
  - Tracks who accessed data and what services were used
![image](https://github.com/user-attachments/assets/e4436106-d464-4f2c-92fe-0d34b07741e5)

---

## Insights and Findings
- Dataset includes **271 cultural venues** across various ownerships  
- **Privately owned** venues are most common and span a wide range of sizes  
- **Smallest venue**: 600 sq ft | **Largest seating**: 2,756  
- Historical records (2015–2017) tend to have more quality issues (duplicate coordinates)  
- Filtering data can significantly affect insights—as shown in the contrast with Project One

---

## Recommendations
- City stakeholders can use these findings to **balance funding** between large venues and accessible community spaces  
- Consider stricter **data entry quality control** for ownership and geolocation fields  
- Leverage encryption + monitoring setups as a model for other municipal datasets to ensure **compliance and security**

---

## Tools and Technologies Used
- **Amazon Athena** – SQL querying  
- **Amazon S3** – Storage and result output  
- **AWS Glue DataBrew & Studio** – Cleaning and data quality jobs  
- **AWS KMS** – Encryption  
- **AWS CloudWatch** – Monitoring, dashboard, and alerts  
- **AWS CloudTrail** – Pipeline activity tracking  
- **SNS** – Email alerting

---

## Deliverables
- SQL queries and results stored in versioned S3 buckets  
- Glue data quality job output (passed & failed datasets)  
- Security configuration with KMS  
- Monitoring setup (dashboard, alerts, CloudTrail logs)  
- This full descriptive report for analysis and portfolio inclusion



#Project 3:  Data Wrangling Report: UCW Academic Hiring Dataset

## Project Title
Data Wrangling for UCW Academic Hiring Data

## Project Objective

The objective of this project is to clean, transform, and structure the UCW Academic Hiring dataset to ensure data accuracy, consistency, and readiness for analysis. The key goals include:

•	Handling missing values in faculty application records

•	Removing duplicate entries to avoid data redundancy

•	Standardizing formats for consistency in applicant information

•	Structuring data for optimized querying using AWS services

•	Aligning dataset cleaning with faculty hiring criteria from UCW policy

## Project Description

This project leverages AWS cloud services for data profiling, cleaning, and structuring. The dataset, stored in Amazon S3, contains faculty hiring application records, including applicant details, qualifications, and hiring decisions. Using AWS Glue DataBrew, AWS Glue Catalog, and AWS S3, the data is transformed to meet quality and analysis requirements while aligning with UCW-5008 Hiring and Appointment of Faculty Policy.

## Data Source

•	UCW Hiring and Appointment of Faculty Policy (UCW-5008)

•	Document Link: UCW Faculty Policy PDF

•	Description:

The policy document outlines the faculty hiring process, eligibility criteria, appointment types, and evaluation standards at the University Canada West (UCW). This dataset is structured based on faculty hiring applications and qualifications, ensuring alignment with the policy’s hiring framework.

## Dataset Description

The dataset contains faculty hiring application records, structured into four categories, each stored in a separate folder:
Folder Name	Description

      academic-user-log/:    Logs of faculty application activity
      
      application-list/ :    List of applicants and their hiring status
      
      course-list/: 	        Courses applied for by faculty members
      
      degree-list/ :          Educational qualifications of applicants

## Data Ingestion Process

Overview of Data Ingestion

Data ingestion is the process of collecting, importing, and storing raw data from multiple sources into a centralized system for processing. In this project, Amazon S3 was used as the primary storage location for ingesting and managing the raw UCW Academic Hiring dataset before cleaning and transformation.

## Sources of Data

The data was collected from the UCW Hiring and Appointment of Faculty Policy (UCW-5008) and structured into an application dataset containing information on faculty hiring records, qualifications, and appointment decisions.
The dataset was provided in CSV format and stored in Amazon S3 for further processing.
Steps in the Data Ingestion Process

Data Collection & Organization in Amazon S3

   •	The raw dataset was uploaded into an Amazon S3 bucket to facilitate structured storage and access for the data wrangling process.
   
   •	A folder structure was established within S3 to categorize different data elements, ensuring efficient retrieval and management.

### Amazon S3 Bucket Structure

      s3://academics-raw-itu/hiring/academic-user-log/:  Logs of faculty application activities
      
      s3://academics-raw-itu/hiring/application-list/:   List of applicants and their hiring status
      
      s3://academics-raw-itu/hiring/course-list/:        Courses applied for by faculty members
      
      s3://academics-raw-itu/hiring/degree-list/:        Educational qualifications of applicants

Each dataset was stored as a CSV file in the respective folders.

![image](https://github.com/user-attachments/assets/5b11ec54-03ef-473a-93ae-354ff0938f7a)


## Data Access and Initial Exploration

      •	The data was accessed from S3 using AWS Glue DataBrew to examine the structure and ensure all files were properly formatted.
      
      •	The dataset was checked for inconsistencies, such as corrupted rows, incorrect delimiters, and encoding issues.

## Findings from Initial Data Exploration

      •	Some fields contained unexpected null values, which required further cleaning.
      
      •	The degree-list dataset had inconsistencies in degree naming conventions (e.g., “Ph.D.” vs. “PhD” vs. “Doctorate”).
      
      •	The applicant IDs in application-list had varying formats, requiring standardization.

Storage and Data Integration for Processing

After validation, the raw datasets were:

1.	Copied to a transformation bucket (s3://academics-trf-itu/) to apply cleaning and structuring.
3.	Integrated into AWS Glue DataBrew for profiling and cleaning.
4.	Prepared for further transformation before being cataloged in AWS Glue.
   
## Updated Storage Path after Ingestion

      s3://academics-raw-itu/: 	Raw data storage (original files)
      s3://academics-trf-itu/:	Transformation bucket (cleaning process)
      s3://academics-cur-itu/:	Final curated dataset storage

## Data Profiling (Quality Check)

   •	Used AWS Glue DataBrew to profile the dataset.
   
   •	Identified missing values, duplicate rows, and incorrect data types.
   
   •	A correlation matrix was generated to examine relationships between variables (e.g., Years of Experience and Publications).

## Findings:

   •	Total Rows: 50
   
   •	Total Columns: 10
   
   •	Data Types: 

      o	Integer: 4 columns (Years of Experience, Publications, etc.)
      
      o	String: 6 columns (Name, Degree, Hiring Status, etc.)

   •	Missing Values: 

      o	3% missing in "Degree"
      
      o	1% missing in "Hiring Status"

   ![image](https://github.com/user-attachments/assets/5896b5ad-0514-4a50-b0ac-436b3f2fd3d5)

## Data Cleaning (Error Handling and Standardization)

•	Handling Missing Values:

      o	Degree column: Missing values were filled with the most frequent degree (e.g., "PhD"), aligning with UCW faculty hiring requirements.
      
      o	Hiring Status column: Missing values were set to "Pending," ensuring unreviewed applications are properly classified.
      
      o	Courses Applied For: Missing values were filled with "General Teaching" if no course was provided.

•	Handling Duplicates:

      o	Duplicate Applicant IDs were detected and removed.
      
      o	Checked for duplicate names and verified against application dates.

•	Standardizing Formats:

      o	Applicant ID: Ensured a uniform format ("UCW-XXXXXX").
      
      o	Degree Names: Converted to a consistent format ("PhD," "Master's," "Bachelor's") to match UCW hiring categories.
      
      o	Experience Years and Publications: Rounded to the nearest whole number for consistency in reporting.

![image](https://github.com/user-attachments/assets/41291fee-7b56-46b1-ba1a-ff5de831aeaa)


## Final Outcome and Insights

### Key Outcomes

•	Dataset cleaned and structured for easy querying.

•	Missing values handled using imputation techniques.

•	Data stored in AWS Glue Catalog for efficient access.

•	Standardized Applicant IDs and Degree Names for uniformity.

## Insights Discovered

•	Higher education correlates with hiring approvals, aligning with UCW hiring policies.

•	Applicants with more than five years of experience have a higher acceptance rate.

•	Courses in high demand received more faculty applications.

## Conclusion and Recommendations

### Final Thoughts

The UCW Academic Hiring dataset was successfully cleaned, transformed, and structured using AWS services. The wrangling process ensures that the data is accurate, consistent, and optimized for hiring analysis, aligning with UCW faculty hiring policies.

## Recommendations

•	Implement automated data validation in AWS Glue to detect errors in real-time.

•	Enhance data partitioning strategies to improve query performance.

•	Integrate Amazon QuickSight to visualize hiring trends dynamically.

## Deliverables

•	Cleaned dataset stored in Amazon S3

•	AWS Glue Data Catalog for querying

•	Screenshots of each step

•	Final report and insights

# Project 4:  Data Quality Control

## Project Title: Implementation of Data Quality Control Measures for the Academic Hiring Dataset

## Project objectuve

The primary objective of this project is to implement a focused Data Quality Control (DQC) framework for the academic hiring dataset used by the department. This framework ensures the accuracy, completeness, uniqueness, and reliability of faculty application data, thereby improving the quality of downstream analytics, reporting, and hiring-related decision-making.
Background
The Academic Hiring Department relies heavily on accurate and clean data for processing applications and supporting faculty recruitment decisions. As application records increase over time, maintaining data quality becomes more critical. Preliminary reviews of the current dataset highlighted common challenges such as duplicate entries, repeated degree values, and a need for ongoing quality validation. This initiative introduces a structured DQC process using AWS Glue Studio, DataBrew, and Amazon S3 to manage and monitor the quality of this dataset effectively.

## Scope

The project focuses on the following key areas:

   • Data Profiling: Analyzing application data to assess its completeness and uniqueness
   
   • Data Validation: Applying rules to enforce basic quality thresholds
   
   • Monitoring and Reporting: Routing and tracking data into passed/failed quality folders
   
   • Process Documentation: Outlining steps and outputs for institutional reporting
   Methodology

## Current State Assessment

   •	Analyzed the department’s hiring dataset stored in s3://academics-raw-itu/hiring/application-list/
   
   •	Identified that the dataset includes key fields like Name, Applicant ID, Degree, and Department
   
   •	Preliminary review indicated issues such as non-unique degrees and a need for systematic completeness validation

## Data Profiling

   •	Used AWS Glue Visual ETL and DataBrew to evaluate data structure
   
   •	Applied profiling checks to three primary fields: name, applicant ID, and degree
   
   •	Key profiling results:

      - Name: ≥ 80% complete – Passed
      - Applicant ID: ≥ 45% unique – Passed
      - Degree: ≥ 75% unique – Failed
      Establish Data Quality Metrics
•	Defined rule-based thresholds for quality control:

      - Name completeness ≥ 80%
      - Applicant ID uniqueness ≥ 45%
      - Degree uniqueness ≥ 75%

All metrics were evaluated within Glue Studio’s rule framework, and records were routed accordingly
![image](https://github.com/user-attachments/assets/f9752127-a860-4c1f-ac69-414a9ac86bd8)

## Data Cleansing Processes

   •	No transformations or corrections were applied directly
   
   •	Failed records were isolated into a separate S3 folder for manual review
   
   •	Duplicate records were automatically filtered out during the ETL process
   
   •	No standardization (e.g., formatting of degrees or names) was conducted at this stage
   Validation Rules and Procedures
   
   •	Glue Studio nodes enforced rule logic for completeness and uniqueness
   
   •	Records meeting all validation criteria were routed to a 'Passed' output
   
   •	Records failing any rule were routed to a 'Failed' output for transparency
   Monitoring and Reporting
   
   •	Amazon S3 was used to separate clean vs. problematic records:

      - s3://academics-trf-itu/.../QualityCheck/Passed/
      - s3://academics-trf-itu/.../QualityCheck/Failed/

![image](https://github.com/user-attachments/assets/536957ea-1825-4648-baa5-3939af0e0a7a)
This folder structure enables ongoing tracking of issues and supports transparency during reporting

Training and Best Practices

   •	As part of follow-up, it is recommended that staff who handle hiring data be introduced to basic validation principles
   
   •	A data entry guide can help ensure consistent field formats (e.g., degrees or applicant IDs), especially as new records are added

Feedback Mechanism

   •	Validation output logs and failed records provide a clear feedback loop
   •	Manual review of failed records supports process improvement and readiness for future automation
   
## Tools and Technologies

   • AWS Glue Studio – For building and executing Visual ETL workflows with rule logic
   
   • AWS Glue DataBrew – For dataset profiling and insight generation
   
   • Amazon S3 – For storing raw, processed, passed, and failed datasets
   
   • Draw.io – For pipeline architecture illustration
![image](https://github.com/user-attachments/assets/a82b7b29-1200-4fb2-b81f-47c6c52b59c7)
![image](https://github.com/user-attachments/assets/dc6fc41d-1276-491a-b5b0-a2162b8e3574)
   
## Deliverables
   • A working Data Quality Control pipeline using AWS Glue

   • Separate passed/failed dataset outputs organized in Amazon S3

   • Screenshots and documentation of validation rules and outcomes
   
   • Data profiling summary highlighting key field completeness and uniqueness
   
   • A clear record of data failures (e.g., degree values below uniqueness threshold) for further review

## Timeline

   • Total duration of implementation and testing: 8 weeks
   
   • Activities included: Dataset review, rule setup, ETL build, execution, result validation, and reporting
   
## Conclusion

This Data Quality Control initiative has successfully established a clear, rule-based validation process for the academic hiring dataset. It provides a reliable foundation for maintaining clean records and supports better decision-making. The passed/failed folder structure ensures traceability, while the Glue pipeline offers repeatability and room for future enhancements such as degree standardization or automated correction. This initiative helps the department maintain high data integrity and ensures that recruitment decisions are supported by clean, accurate, and trustworthy data.


#Project 5:  **Data Quality Control for City of Vancouver- Cultural spaces**

### **Project Description:**  
Data Quality Control Initiative for Vancouver Cultural Spaces Dataset

---

### **Project Title:**  
Implementation of Data Quality Control Measures using AWS Glue for Cultural Space Data

---

### **Objective:**  
The objective of this project is to establish a robust Data Quality Control (DQC) framework for the Vancouver cultural spaces dataset. This framework ensures data completeness and uniqueness, enabling reliable descriptive analysis and policy planning while reinforcing the integrity of cloud-based data pipelines.

---

### **Background:**  
As the dataset for Vancouver’s cultural venues expanded across multiple sources and years (2015–2020+), issues such as missing ownership data and duplicated location points became evident. These data quality challenges can lead to misleading insights and hinder public infrastructure planning. This project addresses these concerns using AWS-native services to enforce quality rules and isolate invalid records.

---

### **Scope:**  
The project covers the following core quality areas:
- **Data Profiling:** Assess the structure, validity, and uniqueness of venue records.  
- **Data Validation:** Implement field-level rules to check for missing values and duplicates.  
- **Data Segmentation:** Separate valid records from problematic ones using Glue branching.  
- **Storage and Logging:** Store passed and failed outputs separately in S3 for traceability.  
- **Monitoring and Reporting:** Maintain full transparency of rule outcomes for auditing.

---

### **Methodology:**

#### **1. Current State Assessment**
- Reviewed transformed datasets stored in S3 (`cps-trf-itu`)
![image](https://github.com/user-attachments/assets/816ee1b7-b821-4697-8910-ca24aa85002d)

- Identified inconsistencies in `ownership` and repeated `geo_point_2d` values from years like 2015–2017


#### **2. Data Profiling**
- Used AWS Glue Studio’s visual data preview to evaluate field fill rates and uniqueness  
- Focused on two main fields: `ownership` and `geom`
![image](https://github.com/user-attachments/assets/51fc4afd-5fd4-451c-b5da-eaad978ce1b8)


#### **3. Establish Data Quality Metrics**
- **Completeness Rule:** `ownership` must be ≥ 95% non-null  
- **Uniqueness Rule:** `geom` must be ≥ 85% distinct across records
  
![image](https://github.com/user-attachments/assets/6d9b87d1-68be-47e2-add1-5704d5bbe13f)

#### **4. Data Validation Rules**
- Defined and applied rules in AWS Glue Data Quality transform:
  ```python
  Rules = [
    Completeness "ownership" >= .95,
    Uniqueness "geom" > .85
  ]
  ```
- Configured branching: valid records directed to `Quality_passed`, invalid to `default_group`

#### **5. Output Segmentation & Cleansing**

**-AWS gkue ETL Pipeline**
![image](https://github.com/user-attachments/assets/ded8cdc3-0b68-4156-b936-ec68fcc8e9d1)

- **Passed records:** Saved in  
  `s3://cps-trf-itu/cps-data/theatreperformance/Quality Check/Passed/`  
  File: `run-1742615306095-part-r-00000`
![image](https://github.com/user-attachments/assets/50651c6a-1075-4a1c-801b-88b663587010)
  

- **Failed records:** Saved in  
  `s3://cps-trf-itu/cps-data/theatreperformance/Quality Check/Failed/`  
  File: `run-1742615285164-part-r-00000`
![image](https://github.com/user-attachments/assets/350157b4-d483-4d3a-b9a1-4fe0f87b8862)


#### **6. Monitoring and Reporting**
- AWS Glue Studio provided evaluation feedback for each rule  
- Visual previews helped compare records tagged “Passed” vs. “Failed”  
- Detected most failures from years 2015, 2016, and 2017

#### **7. Training and Best Practices**
- Process steps documented to enable repeatable application across other datasets  
- Promotes consistent handling of quality rules in future Glue pipelines

#### **8. Feedback Mechanism**
- Continuous refinement of rules and thresholds based on inspection of failed data  
- Historical records flagged for possible correction or exclusion

---

### **Tools and Technologies:**
- **AWS Glue Studio** – Data quality rule configuration and visual job orchestration  
- **Amazon S3** – Data storage and rule-based output separation  
- **Athena (indirect)** – Downstream analytics on cleaned dataset  
- **Python (underlying Glue logic)** – Rule definitions and field transformations

---

### **Deliverables:**
- AWS Glue Job: `cultural-spaces-QC-itu`  
- Quality rule definition and evaluation reports  
- Passed and failed datasets stored in distinct S3 folders  
- Documentation of metrics and rule thresholds  
- Optional integration into a broader analytics pipeline

---

### **Timeline:**
- Completed in ~1 week within broader descriptive analysis timeline  
- Included data assessment, rule design, job implementation, and result validation

---

This Data Quality Control initiative significantly enhanced the **accuracy** and **reliability** of the cultural spaces dataset, supporting trustworthy analysis and informed decision-making for municipal and public planning use.

---

Let me know if you want this in **Markdown (`.md`)**, **Word (`.docx`)**, or added into your current GitHub `README` file.

# AWS COMPLETION BADGE

![image](https://github.com/user-attachments/assets/4f87af54-a801-43d2-bbc8-fdebdcef0437)









