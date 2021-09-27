# Expert Data Modeling with Power BI - Soheil Bakshi and Christian Wade

>The purpose of data modeling in Power BI is different from data models in transactional systems. In a transactional system, the goal is to have a model that is optimized for recording transactional data. Nevertheless, a well-designed data model in Power BI must be optimized for querying the data and reducing the dataset size by aggregating that data.

> **Semantic model**
Power BI inherits its blood from Power Pivot and SSAS Tabular models. All of them use the xVelocity engine, an updated version of the VertiPaq engine designed for in-memory data analysis and consisting of semantic model objects such as tables, relationships, hierarchies, and measures, which are stored in memory, leveraging column store indexing. 

## Some question patterns to ask the business before setting up a model for the data

> Scenario
> -  An OData data source with 15 tables. The tables have between 50 and 250 columns.
> - An Excel file with 20 sheets that are interdependent with many formulas.
> - A data warehouse hosted in SQL Server. You need to get data from five dimensions and two fact tables:
> - - Of those five dimensions, one is a Date dimension and the other is a Time dimension. The grain of the Time dimension is hour, minute.
> - - Each of the fact tables has between 50 and 200 million rows. The grain of both fact tables from a date and time perspective is day, hour, minute.
> - - Your organization has a Power BI Pro license.


OData: OData is an online data source, so it could be slow to load the data from the source system.
50 - 250 columns The tables are very wide so it can potentially impact the performance.
**Our organization has a Power BI Pro license, so we are limited to 1 GB file size.**

The following questions should be answered by the business. Without knowing the answers, we may end up building a report that is unnecessarily large with poor performance. This can potentially lead to the customer's dissatisfaction:

(a) Do we really need to import all the columns from those 15 tables?

(b) Do we also need to import all data or is just a portion of the data enough? In other words, if there is 10-years worth of data kept in the source system, does the business need to analyze all the data, or does just 1 or 2 years of data fit the purpose?

Excel: Generally, Excel workbooks with many formulas can be quite hard to maintain. So, we need to ask the following questions of the business:
(a) How many of those 20 sheets of data are going to be needed by the business? You may be able to exclude some of those sheets.

(b) How often are the formulas in the Excel file edited? This is a critical point as modifying the formulas can easily break your data processing in Power Query and generate errors. So, you would need to be prepared to replicate a lot of formulas in Power BI if needed.

Data warehouse in SQL Server: It is beneficial to have a data warehouse as a source system as data warehouses normally have a much better structure from an analytical viewpoint. But in our scenario, the finest grain of both fact tables is down to a minute. This can potentially turn into an issue pretty quickly. Remember, we have a Power BI Pro license, so we are limited to a 1 GB file size only. Therefore, it is wise to clarify some questions with the business before we start building the model:
(a) Does the business need to analyze all the metrics down to the minute or is day-level enough?

(b) Do we need to load all the data into Power BI, or is a portion of the data enough?

We now know the questions to ask, but what if the business needs to analyze the whole history of the data? In that case, we may consider using composite models with aggregations.

Having said all that, there is another point to consider. We already have five dimensions in the data warehouse. Those dimensions can potentially be reused in our data model. So, it is wise to look at the other data sources and find commonalities in the data patterns.

## Star schema (dimensional modeling) and snowflaking

First things first, **star schema and dimensional modeling are the same things**.

**Transactional modeling versus star schema modeling**
In transactional systems, the main goal is to improve the system's performance in creating new records and updating/deleting existing records. So, it is essential to go through the normalization process to decrease data redundancy and increase data entry performance when designing transactional systems. In a straightforward form of normalization, we break tables down into **master-detail tables**.

But the goal of a business analysis system is very different. In business analysis, we need a data model optimized for **querying in the most performant way**.

In a **Star Schema** instead of having fully normalized tables that are then joined/merged and then aggregated to create the sums, average, percents by segments that the business requests,  **we set up a table that is already joined all those tables based on business entities. We aggregate and loaded the data into denormalized tables.**
For instance :
- We can summarize the data at the day level instead of holding it at the second level as it exists in the transactional table.
- The idea of the star schema is to keep all numeric values in separate tables called fact tables and put all descriptive information into other tables called dimension tables.

**Snowflaking** is when you do not have a perfect star schema when dimension tables surround your fact tables. In some cases, you have some levels of descriptions stored in different tables. Therefore, some dimensions of your model are linked to some other tables that describe the dimensions in a greater level of detail. Snowflaking is normalizing the dimension tables.

**Denormalization**
The general rule of thumb is to denormalize your model so that a dimension can describe all the details as much as possible. For instance in Adventure DW,  we have snowflakes of Product Category and Product Subcategory that can be simply denormalized into the Product dimension.

**There are several ways to keep the file size just below the limit, as follows:**

Import the necessary columns only.
Import just a portion of data when possible. Explain the technology limitation to the business and ask whether you can filter out some data. For instance, the business may not need to analyze 10 years of data, so filter older data in Power Query.
Use aggregations. In many cases, you may have the data stored in the source at a very low granularity. However, the business requires data analysis on a higher grain. Therefore, you can aggregate the data to a higher granularity, then import it into the data model. For instance, you may have data stored at a minute level. At the same time, the business only needs to analyze that data at the day level.
Consider disabling auto date/time settings in Power BI Desktop.
Consider optimizing data types.

**SHARED DATASETS**
As the name implies, a shared dataset is a dataset used across various reports in a modern workspace (a new workspace experience) within the Power BI service. Therefore, it is only available in the Power BI Professional and Power BI Premium licensing plans. This feature is quite crucial to data modelers. It provides more flexibility in creating a more generic dataset, covering more business entities in a single dataset instead of having several datasets that may share many commonalities.

**DATAFLOWS**
Power BI Dataflows
Dataflows, also referred to as Power Query Online, provide a centralized data preparation mechanism in the Power BI service that other people across the organization can take advantage of. Like using Power Query in Power BI Desktop for data preparation, we can prepare, clean, and transform the data in dataflows. Unlike Power Query queries, which are isolated within a dataset, when created in Power BI Desktop and then published to the Power BI service, you can share all data preparations, data cleansing, and data transformation processes across the organization with dataflows.








