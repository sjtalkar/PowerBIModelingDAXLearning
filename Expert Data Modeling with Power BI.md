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


