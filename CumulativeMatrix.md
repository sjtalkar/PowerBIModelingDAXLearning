## Converting a calculated column formula to a measure 

A calculated column has access to each element in a row. While in a measure placing a CALCULATE prior to an aggregation such as SUM or a MAX, **without any filters** is redundant, as in MEASURE_1 = CALCULATE(SUM(<column name>)), in a calculated column, if you want to derive the "group" max, you HAVE to include the CALCULATE.
  
To convert a foumal such as INT(Sales[Deliver Date] - Sales[Order Date]) from a calculated column, where every row has a delivery date and an order date, in a measure, since we do not have individual rows, we have to use a iterator that WILL provide us with access to each row, such as AVERAGEX, or SUMX...

MEASURE_2 = SUMX(Sales,
               INT(Sales[Deliver Date] - Sales[Order Date])
            ) 
  
