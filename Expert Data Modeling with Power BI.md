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


**POWER QUERY EDITOR**
**Enable load to report:** When enabled, data will be loaded into the data model from the source system(s). As you see in Figure 3.15, we merged the Product query with two other queries. Each query may come from a different data source. When this option is disabled, data will not be loaded into the data model. However, if other queries reference this query, data will flow through all the transformation steps applied to this query.
Include in report refresh: In some cases, we need data to be loaded into the model just once, so we do not need to include the query in the report refresh. When this option is enabled, the query gets refreshed whenever the data model is refreshed. We can either refresh the data model from Power BI Desktop when we click the Refresh button or we can publish the report to the Power BI service and refresh data from the service. Either way, if this option is disabled for a query, that query is no longer included in future data refreshes.
IMPORTANT NOTE

**The Include in report refresh** option is dependent upon the Enable load to report option. Therefore, if Enable load to report is disabled, then Include in report refresh will also be disabled.

It is a common technique in more complex scenarios to disable Enable load to report for some queries that are created as transformation queries. The other queries then reference these queries.

**Query parameters **
Data Source
Filter Rows
Keep Rows
Remove Rows
Replace Rows

**In Chapter 3 you can find this function to rename column names of a table using Table.TransformColumnNames **
```
let
   fnRename = (ColumnName as text) as text =>
      let
            SplitColumnName = Splitter.SplitTextByCharacterTransition({"a".."z"}, {"A".."Z"})(ColumnName)
        in
    Text.Combine(SplitColumnName, " ")
 in
    fnRename
```
Note that the above function RETURNS a function
To call the function:

Now, let's call the fnRenameColumns function in the Product table, as follows:
Enable Formula Bar if it is not enabled already from the View tab,
Select the Product table from the Queries pane.
From the Formula Bar, click the Add Step button () to add a new step. This is quite handy as it shows the last step name, which we will use next. 
We now use the Table.TransformColumnNames() function, which transforms column names of a given table by a given name-generator function. This table comes from the previous step, and the name-generator function is the fnRenameColumns function we created earlier. So, the function will look like this:
Table.TransformColumnNames(#"Renamed Columns", fnRenameColumns)
After committing to the running of this step, all columns in the Product table rename immediately.  


## Star Schema and Data Modeling Common Best Practices

### Bridge tables
In relational data modeling, unlike in Power BI, we can only implement the many-to-many relationship using a bridge table regardless. In classic relational data modeling, we put the primary keys of both tables participating in the relationship into an intermediary table referred to as a bridge table. The bridge tables usually are available in the transactional source systems. For instance, there is always a many-to-many relationship between a customer and a product in a sales system. A customer can buy many products, and a product can end up in many customers' shopping bags. What happens in the sales system is that when we go to the cashier to pay for the products we bought, the cashier scans each product's barcode. So the system now knows which customer bought which product.

In data modeling using the Star Schema approach, a bridge table is a table created specifically for managing many-to-many relationships. The many-to-many relationships usually happen between two or more dimensions. However, there are some cases when two fact tables are involved in a many-to-many relationship. In data modeling in the Star Schema, the fact tables containing the foreign keys of the dimensions without any other additive values are called factless fact tables.

The below is an example of a bridge table linked through a composite key or order id and line item id that handles a Many-to-Many relationship that exists between the two tables
![Bridge](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/BridgeTableusingcompositekeys.PNG)
