# Aggregations

> - The way aggregation tables work is simple: we aggregate the data at a particular grain and make it available in the data model. Already aggregating the data that's available in the model usually translates into better performance at runtime. However, the aggregation typically happens at a different level of granularity. Therefore, we change the granularity of the base table. Now, you may be wondering, so what if the business needs to drill down to a lower granular level of data? The answer is that we need to keep the base table available in the data model. 

> - We then implement a control mechanism to detect the level of granularity that the user is at. If the data is available at the aggregated level, the calculation happens in the aggregated table. When the user drills down to the lower grain, the control mechanism runs the calculations in the base table. 
