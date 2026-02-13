----------NYC Jobs Data Engineering Assessment--------
### Overview

This project analyzes the NYC Jobs dataset using PySpark to perform data exploration, transformation, feature engineering, and KPI analysis.

The objective of this assessment was not only to compute required KPIs, but also to design a scalable and production-ready data processing pipeline using distributed Spark processing.

The solution is structured in modular steps:

Data Exploration

Data Cleaning & Preprocessing

Feature Engineering

KPI Computation

Visualization

Processed Data Storage

### Data Exploration

The dataset contains job postings from the City of New York, including both internal and external postings.

Schema Observations

Most columns were initially loaded as string types.

Salary fields required cleaning and numeric casting.

Posting date required conversion to proper date format.

Several columns were categorical (Agency, Job Category, etc.).

Job Description and Minimum Qualification fields were unstructured text.

Column Classification

Numerical Columns (after cleaning):

salary_from

salary_to

avg_salary

Of Positions

Categorical Columns:

Agency

Job Category

Posting Type

Level

Salary Frequency

Text Columns:

Job Description

Minimum Qual Requirements

Preferred Skills

Data Quality Checks

Verified duplicate Job IDs.

Calculated null percentage per column.

Identified salary fields requiring transformation.

Validated schema before performing aggregations.

### Data Processing
Cleaning & Preprocessing

The following transformations were applied:

Removed special characters from salary fields.

Cast salary columns to double.

Converted posting date to Spark date format.

Created normalized salary feature (avg_salary).

All transformations were implemented using Spark-native APIs to ensure distributed execution.

### Feature Engineering

At least three feature engineering techniques were applied:

### Average Salary

Created avg_salary as the midpoint between salary_from and salary_to.

### Degree Encoding

Education level was encoded numerically:

0 → Not specified

1 → Bachelor

2 → Master

3 → PhD

This enabled correlation analysis between education level and salary.

### Skill Extraction

Technical skills (Python, SQL, AWS, Spark, Azure) were extracted from job descriptions using keyword matching.
Binary flags were created for each skill.

### KPIs Implemented
KPI 1 – Top 10 Job Categories

Calculated the number of job postings per category and identified the top 10.

KPI 2 – Salary Distribution per Job Category

Computed mean, min, max, and standard deviation of salaries per job category.

KPI 3 – Correlation Between Degree and Salary

Used Spark’s correlation function to measure relationship between education level and salary.

KPI 4 – Highest Salary per Agency

Used window functions to identify the highest paying job within each agency.

KPI 5 – Average Salary per Agency (Last 2 Years)

Filtered postings dynamically for the last 24 months and calculated average salary per agency.

KPI 6 – Highest Paid Skills

Computed average salary for postings containing specific technical skills and ranked them.

### Visualization

Visualization was performed using Seaborn and Matplotlib.

Important design decision:
Only aggregated Spark DataFrames were converted to Pandas before plotting.
This ensures that the driver memory is not overloaded.

Charts created:

Top 10 Job Categories (Bar Chart)

Salary Distribution (Boxplot)

Degree vs Salary (Bar Chart)

Highest Paid Skills (Horizontal Bar Chart)

Agency Salary Comparison (Bar Chart)

### Test Cases

Basic data validation tests were implemented:

No negative salary values

Salary columns properly cast to numeric

No duplicate Job IDs

Degree encoding column exists

Date column successfully converted

These tests help ensure data quality before analytics.

### Deployment Proposal

If deployed in production, the following approach would be used:

Step 1: Convert Notebook to Python Script

Refactor logic into modular scripts (main.py, transformations.py, kpis.py).

Step 2: Submit Spark Job

Use:

spark-submit main.py

This allows distributed execution on a Spark cluster.

Step 3: Store Output in Data Lake

Write processed data in Parquet format:

Efficient

Compressed

Optimized for analytics

Partitioning by year can be applied for better performance.

### Trigger Strategy
Recommended: Event-Driven Trigger

When a new CSV file is uploaded to object storage:

Storage event triggers Airflow DAG

DAG executes Spark job

Processed output is stored automatically

Benefits:

Cost efficient

Automated

Near real-time

Alternative: Scheduled Batch

If data refresh is periodic:

Run job daily using Airflow or cron

### Assumptions

Salary values represent annual compensation.

Degree requirements inferred via keyword matching.

NYC dataset used as proxy for US public job market.

Skill detection based on keyword search.

### Challenges

Salary fields stored as formatted strings.

Education level embedded in free text.

Skill extraction from unstructured job descriptions.

Missing values in some categorical fields.

Data skew across agencies.

### Considerations

Avoided collect() on large datasets.

Used window functions for scalable ranking.

Used dynamic date filtering.

Ensured schema validation before aggregation.

Used Parquet format for efficient storage.

### Learnings

Practical use of Spark window functions.

Importance of datatype casting before analysis.

Efficient distributed data processing.

Modular pipeline design improves maintainability.

Visualization should be done only after aggregation.

### Conclusion

This solution was designed with scalability, maintainability, and production-readiness in mind.
All heavy computations were performed using Spark-native APIs to leverage distributed processing.

The pipeline can be deployed as a batch Spark job and orchestrated via Airflow for automation.
The design ensures clean modular structure, efficient processing, and meaningful analytical insights from the NYC jobs dataset.