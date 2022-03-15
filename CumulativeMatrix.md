## Cumulative matrix

### Converting a calculated column formula to a measure 

[WATCH](https://www.youtube.com/watch?v=f5IdCDF1fj4)
[PBIX](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/Beginner%20guide%20to%20DAX%20by%20examples%20(Final).pbix)

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

  #### Aside : Create a table from a list of elements. The list is by default given the column name Value. Rename it using SELECTCOLUMNS.
  This table contains the delivery duration days categories required in the matrix. 
  
```  
  Days = 
        SELECTCOLUMNS (
            { 1, 2, 3, 5, 7, 10, 15 },
            "Days", [Value]
        )
```  

  For a cumulative matrix, we need to add up all orders prior to a category of days. For instance Category of Day 1 will contain orders delivered within or before 1 days, Category of Day 2 will contain order delivered within or before 2 days and so on.
  
  The below DAX picks up orders delivered within 7 days or earlier and computes the number of orders measure seen above (that respects at filter it is used under).
  
  ```
  Delorders = 
    CALCULATE (
        [#Orders],
        Sales[Delivery Days] <= 7
    )
  ```                             
         
  We now want to iterate over the categories of days and create a measure that uses each of the days and computes the number of orders delivered within or before these days. We cannot use an iteration function but we can grab the ccategory column value in SELECTEDVALUE. We can then dynamically compute the orders prior to a Delivery Days category. 
   
To calculate the percentage divide these cumulative orders by teh total number of orders.
              
                                
                                
```                                
Del AM   =
VAR DelDays = SELECTEDVALUE( Days[Days] )
                                
VAR Allorders = [#Orders]
VAR DelordersPerc = 
    DIVIDE (
        CALCULATE (
            [#Orders],
            Sales[Delivery Days] <= DelDays
        ),
        AllOrders 
    )
VAR PrevDelDays = CALCULATE ( MAX ( Days[Days] ), Days[Days] < DelDays )
VAR DelordersPercPrev = 
    DIVIDE (
        CALCULATE (
            [#Orders],
            Sales[Delivery Days] <= PrevDelDays
        ),
        AllOrders 
    )
RETURN IF ( DelOrdersPercPrev < 1, DelOrdersPerc )
```                                
The above DAX also display all cumulative values as they add up to 1 and once 1 is reached then repeatendly show 100% in the next set of delivery duration days.                         
                                
  
  
  

  
  
