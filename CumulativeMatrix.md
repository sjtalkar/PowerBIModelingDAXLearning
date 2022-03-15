## Cumulative matrix

### Converting a calculated column formula to a measure 

[WATCH](https://www.youtube.com/watch?v=f5IdCDF1fj4)

A calculated column has access to each element in a row. While in a measure placing a CALCULATE prior to an aggregation such as SUM or a MAX, **without any filters** is redundant, as in MEASURE_1 = CALCULATE(SUM(<column name>)), in a calculated column, if you want to derive the "group" max, you HAVE to include the CALCULATE.
  
To convert a foumal such as INT(Sales[Deliver Date] - Sales[Order Date]) from a calculated column, where every row has a delivery date and an order date, in a measure, since we do not have individual rows, we have to use a iterator that WILL provide us with access to each row, such as AVERAGEX, or SUMX...

```
MEASURE_2 = SUMX(Sales,
               INT(Sales[Deliver Date] - Sales[Order Date])
            ) 
``` 
  
### Filter contexts with FILTER and advantages of replacing it with CALCULATE
  
Note the usage of RELATED since Sales is being filtered but the filter criteria comes from Stores table   

```
  Average delivery days = 
     AVERAGEX ( 
        FILTER ( 
            Sales,
            RELATED('Store'[Name]) = "Online store"
        ),
        Sales[Delivery Date] - Sales[Order Date] 
    )
  
```
Convert this into a CALCULATE DAX expression and note there is no need for a RELATED. The calculate will take the context in place, either from row or from a slicer.
  
```
  CALCULATE (
              AVERAGEX(
                    Sales
                    Sales[Delivery Date] - Sales[Order Date] 
              )
              'Store'[Name] = "Online store"
  )
```            

### What if the order we are averaging over, contains multiple number of order items?
  - Then the above would be incorrect and we will need to remove the line item granularity.
  - For an order with all items having the same order and delivery date, the below will work
 
```  
 Avg Delivery (days) = 
                CALCULATE (
                    AVERAGEX ( 
                        SUMMARIZE ( 
                            Sales,
                            Sales[Order Number],
                            Sales[Order Date],
                            Sales[Delivery Date]
                        ),
                        Sales[Delivery Date] - Sales[Order Date] 
                    ),
                    'Store'[Name] = "Online store"
                ) 
```

  Now if we create a filter based on duration (number of days betwen delivery and order dates), for each categorical number of days, we meed an order count.
  So create a measure:
 ```
    #Orders = 
        CALCULATE ( 
            DISTINCTCOUNT( Sales[Order Number] ),
            Store[Name] = "Online Store"
        )
 ``` 
```
  #All Orders = CALCULATE (DISTINCTCOUNT(Sales[Order Number]), ALL())
  
  OR if we are considering the above #Orders as being against the categorical duartions, remove the duration filter
  
  #All Orders = 
        CALCULATE ( 
            DISTINCTCOUNT( Sales[Order Number] ),
            Store[Name] = "Online Store",
            REMOVEFILTERS(Sales[Delivery Days])
  
        )
  ```
  
  To get a percentage : divide the number of order by the number of all orders
  
  ```
    Pct Orders = DIVIDE ([#Orders], [#All Orders])
  ```
  
  
  
  
  
  
  
```  
  
  
  
  
  
  

  
  
