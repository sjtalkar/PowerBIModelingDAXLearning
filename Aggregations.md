# Aggregations

> - The way aggregation tables work is simple: we aggregate the data at a particular grain and make it available in the data model. Already aggregating the data that's available in the model usually translates into better performance at runtime. However, the aggregation typically happens at a different level of granularity. Therefore, we change the granularity of the base table. Now, you may be wondering, so what if the business needs to drill down to a lower granular level of data? The answer is that we need to keep the base table available in the data model. 

> - We then implement a control mechanism to detect the level of granularity that the user is at. If the data is available at the aggregated level, the calculation happens in the aggregated table. When the user drills down to the lower grain, the control mechanism runs the calculations in the base table. 

### When is it useful
- valuable when one of our data sources is big data. For instance, we may have billions of rows hosted in Azure Synapse Analytics

### How can it be implemented
- 1. The first method is for implementing aggregation tables for the data sources that do not support DirectQuery mode, such as Excel files. Therefore, we need to implement the aggregation table and take care of the control mechanism to switch between the aggregated table and the base table when the user drills down to a higher grain of data.

- 2.  Manage aggregations. When it comes to using the Manage aggregations feature, the data source of the base table must support **DirectQuery mode.**
[List of sources that support Direct Query](https://docs.microsoft.com/en-us/power-bi/connect-data/power-bi-data-sources?WT.mc_id=?WT.mc_id=DP-MVP-5003466.)
