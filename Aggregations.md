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
```New OrderDateKey = (CONVERT(Integer, ([OrderDateKey]/100)) * 100) + 1```
```Table.ReplaceValue(#"Removed Other Columns",each [OrderDateKey], each (Int64.From([OrderDateKey]/100) * 100) + 1,Replacer.ReplaceValue,{"OrderDateKey"})```
- Summarize on this new key

**CONS TO CONSIDER **
- The summary tables will increase memory and storage consumption.
- The summary tables will also increase the data refresh time.
- The development time also increases as we need to create the summary table, create the relationships, and create the measures.

 
 

