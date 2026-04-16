

# Operational vs Analytical systems :


## Operational System : 

They are also know as application engineers.

1. Consist of backend services and data infrastructure where data is created. 

2. Here, the application code both read and modifies the data in its databases based on the action performed by the users.

3. Does not allow users to directly interact with the data layer with SQL, instead they run queries defined by the application layer.




## Analytical System :

1. servers the need of business analysts and data scientists.

2. They contain read only copy of the data from the operational systems.
Data is optimized for data processing and analytics.

3. analytical databases usually give their users the freedom to write arbitrary SQL queries by hand, or to generate queries automatically using a data visualization or dashboard tool such as Tableau, Looker, or Microsoft Power BI.

Data teams often work together, but each role has a different focus.  
Some people build the systems that move and store data. Some people study that data to find business insights. Others use advanced math and machine learning to make predictions from it.

Understanding these roles makes it easier to see how data turns into decisions.

## 1) Data Engineer

A **data engineer** builds and maintains the systems that collect, store, and process data. Their job is to make data reliable, available, and easy to use for others.

### Core responsibilities

- **Pipeline development (ETL/ELT):** They move data from different sources such as APIs, databases, and third-party tools into a warehouse or lake.
- **Data architecture:** They design and manage data warehouses for structured data and data lakes for raw or unstructured data.
- **Performance optimization:** They tune queries, partitions, and storage so large datasets can be processed quickly.

In simple terms, a data engineer makes sure the data pipeline works smoothly.

## 2) Data Analyst

A **data analyst** studies data to answer business questions. They focus on reporting, trends, and insights that help teams make decisions.

### Core responsibilities

- **Data cleaning and preprocessing:** They fix missing values, remove errors, and prepare data for analysis.
- **Data analysis and interpretation:** They use statistical methods to find patterns, trends, and useful insights.
- **Reporting and visualization:** They create dashboards, charts, and reports for non-technical stakeholders.
- **Business optimization:** They track KPIs and suggest improvements based on the data.

In simple terms, a data analyst explains what the data is saying.

## 3) Data Scientist

A **data scientist** uses statistics, mathematics, and machine learning to solve complex problems and make predictions.

### Core responsibilities

- **Machine learning and predictive modeling:** They build and test models for problems like churn prediction, fraud detection, or pricing.
- **Advanced statistical analysis:** They design experiments and A/B tests to check whether a change really works.
- **Feature engineering:** They turn raw data into useful inputs for machine learning models.
- **Handling unstructured data:** They work with text, images, audio, and other data that is not neatly organized.

In simple terms, a data scientist tries to predict what may happen next.

## Simple comparison

- **Data engineer:** builds the data infrastructure
- **Data analyst:** studies the data and reports insights
- **Data scientist:** builds models and predictions

You can think of it like this:

- The **data engineer** brings water into the house.
- The **data analyst** checks the water and tells you what it means.
- The **data scientist** predicts how much water will be needed in the future.

## Conclusion

These three roles are different, but they work best together.  
Data engineers make data usable, data analysts turn it into business insight, and data scientists use it for prediction and automation.


## OLTP vs OLAP


### What does Online mean in OLTP and OLAP ?

In the early decades of computing, data operations were primarily executed in batches. Transactions were queued throughout the day and processed sequentially overnight by a mainframe. "Online" was introduced to describe systems where users (whether customers or analysts) interact directly with the database and receive immediate, synchronous responses.

- "Online" means that transactions are executed immediately upon request, enabling real-time state changes for the application.

- "Online" analytical processing means an analyst can execute a complex aggregation query against a massive data warehouse and receive the results in seconds or minutes, allowing for iterative, exploratory analysis.


### OLTP :

- Refers to the system that supports transactions and queries on real time.
- Foucs on fast response, high concurrency and strict ACID properites.
- Each operations are handled as a single transaction.
- Operations mainly involve INSERT, UPDATE, and DELETE on small amounts of data.
- Queries are short and fast, not complex analytical ones.

### OLAP :

- Enables fast, multidimensional analysis of large data volumes from data warehouses.
- used for analytics purpose.
- Allows to query comples data over large amount of data.
- Few queries by complex


## Data Warehousing

A data warehouse is a centralized, analytical repository that aggregates massive volumes of structured and  semi-structured data from operational systems.
- Contains only read only data which are periodically dumped from OLTP or throught continuous stream.
- They are first transformed into analysis friendly form by data engineer.
- They are designed for comlex query processing and analysis at real time.
- It typically uses extract, transform and load (ETL) or extract, load and transform (ELT) processes to clean, prepare and organize data for business intelligence (BI) and other data analytics use cases.
![[Pasted image 20260405215559.png]]

### Why we need data warehousing when there are Operational databases ?

- The data of interest may be spread across multiple operational systems, making it difficult to combine those datasets in a single query (a problem known as data silos). 
- The kinds of schemas and data layouts that are good for OLTP are less well suited for analytics.
- Analytical queries can be quite expensive, and running them on an OLTP data base would impact the performance for other users.
- The OLTP systems might reside in a separate network that users are not allowed to directly access, for security or compliance reasons.


### Data lakes :

A data warehouse often uses a relational data model that is queried through SQL, perhaps using specialized BI software. This model works well for the types of queries that business analysts need to make, but it is less well suited to the needs of data scientists performing tasks such as these: 
- Transforming data into a form that is suitable for training an ML model. This often requires turning the rows and columns of a database table into a vector or matrix of numerical values called features. The process of performing this transformation in a way that maximizes the performance of the trained model is called feature engineering, and it commonly requires custom code that is difficult to express using SQL. 
- Using natural language processing (NLP) techniques on textual data (e.g., reviews of a product) to try to extract structured information from it (e.g., the sentiment of the author, or which topics they mention). Similarly, data scientists might need to extract structured information from photos by using computer vision techniques.

A **Data Lake** is a centralized storage repository designed to hold vast amounts of raw data in its native format until it is needed for analytics. It accommodates structured (relational databases), semi-structured (JSON, CSV, logs), and entirely unstructured data (images, audio, raw text) at massive scale.

While a Data Warehouse requires data to be heavily processed, cleaned, and structured before ingestion (Schema-on-Write), a Data Lake ingests data exactly as it is generated by the source system.





# Cloud vs Self-hosting



## cloud native systems :

They are the softwares built from ground up for cloud environment eg aws aurora. 
They are designed to take advantages of cloud model.

The Pillars of cloud native architecture : 
1. microserices
2. containerization
3. dynamic orchestration
4. **Declarative APIs and Automation:** Infrastructure and system configurations are defined as code (IaC). Instead of executing sequential commands to build a server, engineers declare the desired end-state of the system in configuration files (e.g., YAML). Automated CI/CD (Continuous Integration/Continuous Deployment) pipelines read these declarations and reconcile the actual system state to match the desired state.

# Distributed vs single-node systems

