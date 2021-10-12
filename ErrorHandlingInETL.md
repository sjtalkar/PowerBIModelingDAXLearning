#Error Handling in ETL Operations

1. Duplicate the query that shows errors in columns.
2. Select the columns with Error indicated.
3. You can select multiple columns with Ctrl+click
4. In Home menu tab click on Keep Rows -> Keep Errors
5. Now Add a Custom Column for each of the columsn with an error:
    - Call the Custom columns <columnname>-Error
    - in the formula, type 
  
    try <columnname>
  
  - This creates a **Record** for each of the rows 
  - Expand the record a level at a time (make sure to load more to get more cells of the record)
    
   
    
![Final Output](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/Errorlogs.png) 
  
