# data-analyst-Itunuoluwa
# Descriptive Analysis: Cultural Spaces in Vancouver (DAP Implementation)

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

 User folder
![image](https://github.com/user-attachments/assets/53e0a204-c503-40b7-8165-26135f31231e)

System Folder
![image](https://github.com/user-attachments/assets/da66d024-f58f-48ac-8924-0370ebf3b51b)

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

# Data Wrangling Report: UCW Academic Hiring Dataset

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

# Data Quality Control

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
   
## Deliverables
   • A working Data Quality Control pipeline using AWS Glue
![image](https://github.com/user-attachments/assets/dc6fc41d-1276-491a-b5b0-a2162b8e3574)

   • Separate passed/failed dataset outputs organized in Amazon S3
   
   • Screenshots and documentation of validation rules and outcomes
   
   • Data profiling summary highlighting key field completeness and uniqueness
   
   • A clear record of data failures (e.g., degree values below uniqueness threshold) for further review

## Timeline

   • Total duration of implementation and testing: 8 weeks
   
   • Activities included: Dataset review, rule setup, ETL build, execution, result validation, and reporting
   
## Conclusion

This Data Quality Control initiative has successfully established a clear, rule-based validation process for the academic hiring dataset. It provides a reliable foundation for maintaining clean records and supports better decision-making. The passed/failed folder structure ensures traceability, while the Glue pipeline offers repeatability and room for future enhancements such as degree standardization or automated correction. This initiative helps the department maintain high data integrity and ensures that recruitment decisions are supported by clean, accurate, and trustworthy data.













