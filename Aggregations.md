# Aggregations

> - The way aggregation tables work is simple: we aggregate the data at a particular grain and make it available in the data model. Already aggregating the data that's available in the model usually translates into better performance at runtime. However, the aggregation typically happens at a different level of granularity. Therefore, we change the granularity of the base table. Now, you may be wondering, so what if the business needs to drill down to a lower granular level of data? The answer is that we need to keep the base table available in the data model. 

> - We then implement a control mechanism to detect the level of granularity that the user is at. If the data is available at the aggregated level, the calculation happens in the aggregated table. When the user drills down to the lower grain, the control mechanism runs the calculations in the base table. 

### When is it useful
- valuable when one of our data sources is big data. For instance, we may have billions of rows hosted in Azure Synapse Analytics

### How can it be implemented
- 1. The first method is for implementing aggregation tables for the data sources that do not support DirectQuery mode, such as Excel files. Therefore, we need to implement the aggregation table and take care of the control mechanism to switch between the aggregated table and the base table when the user drills down to a higher grain of data.

- 2.  Manage aggregations. When it comes to using the Manage aggregations feature, the data source of the base table must support **DirectQuery mode.**
[List of sources that support Direct Query](https://docs.microsoft.com/en-us/power-bi/connect-data/power-bi-data-sources?WT.mc_id=?WT.mc_id=DP-MVP-5003466.)

### Create a table summarized on a category that the user will typically aggregate the numeric values on
-  Summarize the large Fact table on the column(s) that the user will typically request summaitions/aggregations on
-  When you do this you will lose the ability to summarize on any other category as they do not exist in the summarized table. 
-  For example if you have Region and Product and Product color in the Fact table and you summarize om [Region, Product] combination, then you lose the ability to aggregate on color.
-  So you retain the base table and if the queries are not supported by Direct Query, then you Manage the switch in the measures from the Aggregated/Summarized table to the base table, manually.
 - Typical DAX functions that can aid with this switch:

    - ISFILTERED
    - ISINSCOPE
    - HASONEFILTER
    - HASONEVALUE
- You also write measres at the granularity of both the summarized and the base tables and using the above functions, you can select the wrie measure for the filter seelcted by the user that indicates granularity.

For example this is a CONTROL MEASURE:

```
Internet Sales Total =
 IF(
        OR(ISFILTERED('Product'), ISFILTERED('Customer'))
        , [Internet Sales]
       , [Sales Agg]
        )
```


**TIP : ** To convert a table at date granularity and summarize it on Year month granularity

- Create a new date key with year and month
```
New OrderDateKey = (CONVERT(Integer, ([OrderDateKey]/100)) * 100) + 1
```

```
Table.ReplaceValue(#"Removed Other Columns",each [OrderDateKey], each (Int64.From([OrderDateKey]/100) * 100) + 1,Replacer.ReplaceValue,{"OrderDateKey"})
```
- Summarize on this new key


**CONS TO CONSIDER **
- The summary tables will increase memory and storage consumption.
- The summary tables will also increase the data refresh time.
- The development time also increases as we need to create the summary table, create the relationships, and create the measures.


## Managing aggregations is the most significant scalability feature and is also one of the most powerful data modeling features available within Power BI Desktop, since it unlocks the usage of Big Data in Power BI. 

It is only supported for Query Sources that support Direct Query mode

Here is the process of managing aggregations in Power BI Desktop:

- We connect to the data source.
- We load all the necessary tables in DirectQuery mode:
    - The data source may include the aggregation table already.
    - If the summary table is not already in the data source, we can use either Power or DAX to create an aggregation table.
- We create relationships between tables.
- We change the storage mode of the summary table to Import mode. Again, we are mindful that this is an irreversible action. We must also change the storage mode of all the dimensions with an active relationship with the summary table to Dual mode.
- We configure Manage aggregation.
- We test the aggregation to make sure our queries hit the aggregation.


** 
 IMPORTANT NOTES ABOUT AGGREGATIONS**

The detail table (base table) must be in DirectQuery mode.

Precedence in the Manage aggregations window prioritizes the aggregation hits when we have more than one aggregation table. The bigger the Precedence number, the higher the priority. For example, if we have another aggregation table that we would like to get hit before Sales_Agg, we must set a larger Precedence number for that aggregation table. The xVelocity engine tries to solve the underlying aggregation queries based on their Precedence value.

The data types of the aggregation columns from the aggregation table and the corresponding columns from the detail table must be the same; otherwise, we cannot complete the Manage aggregations.

