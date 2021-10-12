# DAX Learning 
Variety of exercises covering visualization Python DAX and Oracle DB data

> SQL BI DAX.do exercises
-   [JOINS](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/SQLBITrainingOnDAXDo.md)
-   [TOPN](https://www.youtube.com/watch?v=eoEbnQyianU)


> ### [Performance webinar](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/PerformanceWebinarWithMarcoRusso.md)

> Phil Seamark A
- [Aggregations](https://www.youtube.com/watch?v=KKWdKD4ztYQ)

The files contained here have work involving custom visualizations such as heat maps
There is advanced DAX formula 
It has advanced Excel Power Pivot examples
There is Python visualization as well as Python code that can be used for Fibonacci sequence (recursion) for instance that cannot be handled in DAX

# [Power BI AI Features](https://www.linkedin.com/posts/ali-sharifi-33a60634_top-15-lod-expressions-activity-6840165259098112000-67MF) 
### [AI In a day by BlueGranite](https://www.youtube.com/watch?v=Ol_3hGHKJAw)
#### [BlueGranite AI in a day GitHub](https://github.com/BlueGranite/AI-in-a-Day)
- Automated and transparent (Beyond if statements)
- AI in visuals can be accessed by : right click on a bar in a bar chart
   - Options available : Analyse 
       - Explain Increase
       - Find where this distrubution is different 


# PowerBI  Calculation Groups

[Video with Marco Russo](https://www.youtube.com/watch?v=a4zYT-N-zsU&t=623s)

[Video with Kane Snyder](https://www.youtube.com/watch?v=IH4bcHhxunw)

Definitive Guide to DAX Second Edition


> Requirements - Tabular Editor to create and maintain

> Calculation groups can apply **specific calculations** on top of **existing DAX measures**. 

> Offers expediency when time intelligence calculations like year-to-date, year-over-year, and currency conversion have to be applied on a variety of measures.

> Can be used to enable the selection of a measure in an existing report through a slicer.

### CALCULATED ITEMS
> Create a Calculation Group in Tabular Editor (for example Time Intelligence)  and add Calculation Items that act upon the SELECTEDMEASURE(). 
> - **Definition of a calculated item**  : A calculation item is a DAX expression containing a special placeholder. The placeholder is replaced with a measure by the engine just before evaluating the result. In other words, a calculation item is a variation of an expression that can be applied to any measure.
> - Each Calculation Item will act on a SELECTEDMEASURE()
> - The Expression of a Calculated Item can go something like 
 `CALCULATE (SELECTEDMEASURE(), SAMEPERIODLASTYEAR(DimDate[Date])`
 - But if you have a dedicated measure for the calculation group then it will is advisable to use the IF( ISELECTEDMEASURE....syntax shown below)

> - The ordinal of the calculated item detemines the sorting order 

> - The formatting of the Calculated Item
  `#,0" or "0.00%" or "\$#,0.00;(\$#,0.00);\$#,0.00"`
  
> The Calculated Group is offered as a Tablein reporting and the Items are offered as Columns

  With this definition, the user will see a new table named Time Intelligence, with a column named Time calc containing three values: YTD, QTD, and SPLY. The user can   create a slicer on that column, or use it on the rows and columns of visuals, as if it were a real column in the model.


### Create a measure within the Calculation Group for a dedicated measure for a group
- Set up a Base measure of each calculated group - for example one calculated group works on Sales Amount and the other works on Sales Qty.

- These are dummy (base) measures called [Calc Group Measure Amount] and [Calc Group Measure Qty]
 
 `IF ( ISSELECTED([Calc Group Measure Amount], [Sales Amount], SELECTEDMEASURE())`

` IF ( ISSELECTED([Calc Group Measure Qty], [Sales Qty], SELECTEDMEASURE())`
 
 ### Tabular Editor's Expression Editor
 
 - In the Expression Editor in Tabular Editor, in the Property dropdown list, you can select Format String Expression
 
 ```IF (
      ISSELECTEDMEASURE( [Calc Group Measure Amount] ),
      "\$#,0.00;,(\$3,0.00);\$#,0.00",
      SELECTEDMEASUREORMATSTRING()
      )
      
 ```
  
  - If you do not want labels, use UNICHAR(8203)

 ```IF (
      ISSELECTEDMEASURE( [Calc Group Measure Amount] )
       && SELECTEDVALUE('Data Labels'[Data Labels] ) = "No Bar Labels",
       
       UNICHAR(8203),
      
      IF (
      ISSELECTEDMEASURE( [Calc Group Measure Amount] ),
      "\$#,0.00;,(\$3,0.00);\$#,0.00",
      SELECTEDMEASUREORMATSTRING()
      )
   )
  ```
  
  ### The option to Blank out the measure - Create a Calculated Item called Blank with the Property Expression as:
  ```IF (
      ISSELECTEDMEASURE([]),
      BLANK(),
      SELECTEDMEASURE()
      )
  ``` 
      
 ### Create a Selector with the Calculated Group's Name/Formula
  
  
 ## Going Further
 
 > Allow the user to select from the range of metric that the calculated items apply on 
  
   In this case, the base metrics itself form a calculation group - Each caclulation item is the base metric measure as in the case of 
    [Sales Amount]
    
    [Sales Qty]
    
    [Total Cost]
    
    [Margin]

> When there are multiple calculation groups in the same data model, it is important to define in which order they should be applied by the DAX engine. The __**Precedence**__ property of the calculation group defines the order of application. 


> A calculation group is a simple entity, defined by
1. The calculation group Name. This is the name of the table that represents the calculation group on the client side.
2. The calculation group Precedence. When there are multiple active calculation groups, a number that defines the precedence used to apply each calculation group to a measure reference.
3. The calculation group attribute Name. This is the name of the column that includes the calculation items, displayed to the client as unique items available in the column.

> A calculation item is a much more sophisticated entity, and here is the list of its properties:

1. The calculation item Name. This becomes one value of the calculation group column. Indeed, a calculation item is like one row in the calculation group table.
2. The calculation item Expression. A DAX expression that might contain special functions like SELECTEDMEASURE. This is the expression that defines how to apply the calculation item.
3. The sort order of the calculation item is defined by the Ordinal value. This property defines how the different calculation items are sorted when presented to the user. It is very similar to the sort-by-column feature of the data model. This feature is not available as of April 2019 but should be implemented before calculation groups are released.
4. **Format String.** If not specified, a calculation item inherits the format string of its base measure. Nevertheless, if the modifier changes the calculation, then it is possible to override the measure format string with the format of the calculation item.

## Isolating a Calculation Item

If the Calculated Item is called YTD:

```CALCULATE (    [Sales Amount],    'Time Intelligence'[Time calc] = "YTD")```

## Precedence Property
> The Precedence property is an integer value assigned to a calculation group. A higher value means a higher precedence of application; the calculation group with the higher precedence is applied first. In other words, DAX applies the calculation groups according to their Precedence value sorted in a descending order. The absolute value assigned to Precedence does not mean anything. What matters is how it compares with the Precedence of other calculation groups. There cannot be two calculation groups in a model with the same Precedence.


## Restricting application of calculated item to selected measures
```
IF (
    ISSELECTEDMEASURE (
        [Sales Amount],
        [Gross Amount],
        [Discount Amount],
        [Sales Quantity],
        [Total Cost],
        [Margin]
    ),
    DIVIDE (
        SELECTEDMEASURE (),
        COUNTROWS ( 'Date' )
    )
)
```
**or**
```
IF (
    NOT ( SELECTEDMEASURENAME () = "Margin %" ),
    DIVIDE (
        SELECTEDMEASURE (),
        COUNTROWS ( 'Date' )
    )
)
```

## Calculation groups offer dynamic format strings
[Alberto Ferrari video on Dynamic format strings](https://www.youtube.com/watch?v=PTaZxL8KPzE)

## [Creating custom groups using Calculation Items](https://www.youtube.com/watch?v=l6si02kf4eU) 


# [Dataflows Webinar](https://info.microsoft.com/ww-Thankyou-harness-PowerBI-SelfService-Prep-Dataflows.html?LCID=EN-US&ocid=eml_pg271206_gdc_comm_ba)

Dataflows and direct query is at about 20 mins in.

# Obscure DAX function that has Pandas Describe like functionality
> Connect a PBIX model to DAX studio and run the below and set the output as static Excel file
EVALUATE
COLUMNSTATISTICS()

This will give you statistics for all the tables and columns in the model.
As an exmaple the ouput looks like the below.
You can then import this table into a PBIX and use the column statistics.

Table Name	         Column Name	                                          Min	Max	Cardinality	Max Length
Fact DIAD Response	RowNumber-2662979B-1795-4F74-8F37-6A1BA8059B61			         211	
Fact DIAD Response	Organization	                                       365 Cannabis	Yrcw	118	46
Fact DIAD Response	Department	Accounting	Workforce Optimization Group	47	38
Fact DIAD Response	Overall, how satisfied were you with the event?		   5.0	5	3



![image](https://user-images.githubusercontent.com/7129567/133482119-a854ded7-6ae0-4777-9117-eb1926b7febe.png)



# [Custom Calendar such as 4-4-5](https://www.daxpatterns.com/week-related-calculations/)
   [Older version](https://www.sqlbi.com/articles/week-based-time-intelligence-in-dax/)
   
   A fiscal calendar is based on weeks, this pattern should be used instead of other patterns based on calendar months.
   The assumptions in this pattern are:
     - Every year is a set of complete weeks;
     - Every period within the year (quarter, month) is a set of complete weeks;
     - The fiscal year always starts on the same day of the week, so it does not always start on January 1.
     - The fiscal month and the fiscal quarter always start on the same day of the week, so they do not always start on the first day of a month.
     
     The pattern does not rely on the standard time intelligence functions. Therefore, the Date table does not have the requirements needed for standard DAX time intelligence functions. The formulas are identical whether you have one row for each week or one row for each day. The examples contain one row for each day, in order to create a relationship with the Sales table through the Sales[Order Date] column.
     
     If there is a Date column in the Date table, the **Mark as a Date Table setting is allowed** but not required. The formulas in this pattern do not rely on the automatic REMOVEFILTERS being applied over the Date table when the Date column is filtered. Instead, all the formulas use a specific REMOVEFILTERS over the Date table to get rid of the existing filters, replacing them with the minimum number of filters that guarantee the result.
     
     
     **What's a filter safe column**
     
    > The Year Quarter Number column is a regular column: the formulas in this pattern have the option of changing its value during their computation. To compute the previous quarter, the formulas change the filter context by subtracting one to the value of Year Quarter Number in the filter context. Conversely, the Day of Week column is a filter-safe column. If a user filters Monday to Friday, the formulas do not alter that filter on the day of the week. Therefore, a previous-quarter measure keeps the filter on the day of the week, and replaces only the filter on calendar columns such as year, month, and date.
    
    Calendar columns: Date, Fiscal Year, Fiscal Year Number, Fiscal Quarter, Fiscal Quarter Number, Fiscal Year Quarter, Fiscal Year Quarter Number, Fiscal Week, Fiscal Week Number, Fiscal Year Week, Fiscal Year Week Number, Fiscal Month, Fiscal Month Number, Fiscal Year Month, Fiscal Year Month Number, Day of Fiscal Month Number, Day of Fiscal Quarter Number, Day of Fiscal Year Number.
Filter-safe columns: Day of Week, Day of Week Number, Working Day.

** If your Date table does not contain any filter-safe column, then you can use REMOVEFILTERS instead of ALLEXCEPT in all the measures of this pattern.**


[Syncing Slicers without introducing bidirectional filer (at about 12 mins)](https://www.youtube.com/watch?v=x3m7qzsVJqQ)
In the slier that should be filtered by the first, add a Fileter pane filter for a value that should linmit it as per selection in the first slicer.     

[Getting data using Power BI connector and pushing to a Database](https://www.youtube.com/watch?v=WgIhsF7kEjI)


## [How to find measures using a column in the database](https://exceleratorbi.com.au/getting-started-dax-studio/)
The article in the link shows you how to use DAX Studio to extract all information about Measures: Their Folder, Database columns using the measure, full text of the measure.

In DAX Studio, in the bottom right tab of DMV, you will find MDSCHEMA_MEASURES
In the main pane select the columns you want and export it out to an excel table or you can pivot information in this SQL.
```SQL
select MEASURE_NAME, 
      MEASURE_UNIQUE_NAME, 
      EXPRESSION,
      MEASUREGROUP_NAME,
      MEASURE_DISPLAY_FOLDER
from $SYSTEM.MDSCHEMA_MEASURES
```
![How to use Dax Studio for measure and columns](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/DaxStudioAndMeasuresAndColumns.png)


## [Data Modeling](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/Expert%20Data%20Modeling%20with%20Power%20BI.md)
   [Data Import modes : Composite Models](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/Data%20input%20Modes%20and%20Models.md)
   
   
   
## Workspace Configuration

#### What is a Workspace 

- A workspace is like a shared folder containing reports and datasets that can be shared.  

- Role Based Access Control is employed – Member/Viewer/Contributor/Admin  

- The RBAC is applicable to the entire contents of the Workspace. It is a single sharing unit. 

- With an App you can share a subset of the workspace. 

 

#### Separating Workspaces – when do you create a new workspace 

- Audience :Primary reason to create a new workspace is if the data is entirely different and is handled and visualized very differently for a distinct set of audience.  
    - This can be based on organization functionality and departments 

    - This can be based on the data available in the workspace – who can view and modify that data 

    - This can be based on the reports to be created from the data in the workspace 

          - E.g. Finance and HR 

 

- Split the load : (only applicable for Premium and embedded  

    - Say you have many clients and some clients in particular have a lot of data  

    - Best practice is to split the dataset for this large client into its own Workspace 

 

- Sharing a workspace 

    - Reduce redundancy in a Shared Workspace by creating a Shared dataset or a Dataflow 

    - Date tables and common entities (queries with data common across organizational groups ) can be set up here. 

 
 - Workspaces for mimicking a development pipeline that is automatically available in Premium 

    - You can create a Dev, UAT testing, Production Workspace ( or tailor it to your developemnt needs) 

 ##### ADDITIONAL DETAILED INFO ABOUT RBAC AND WORKSPACES 
- Viewer Role : Access to all reports, dashboards, no access to dataset, dataflow or Analyze in Excel 

- Contributor Role: Create and Edit access to the content, Edit access to all content in the workspace. CANNOT publish App. Typically provided to Developers 

- Member Role: All that a contributor can do And CAN publish App.  Edit access to all content in the workspace. CANNOT publish App.  Typically provided to  Deployment Managers 

- Admin Role:  All access for Member role. And provide Admin access to others 


#[Scraping data](https://www.youtube.com/watch?v=a0FqNLI0VsQ&t=742s)

jsonplaceholder.typicode.com

## [Error Handling in ETL](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/ErrorHandlingInETL.md)


## Subtracting an hour from DateTime in M Query
- Convert the date time to number. 
- All date times at midnight with be a whole number
- All date times other than midnight will be a decimal number
- Subtract 1/24 (a decimal hour) from the date time
- Convert the value back to DateTime

The below was employed when the electicity generate for a day was to be marked as generated for that day. In this manner the electicity produced at midnight fell within the day it was generated.

```
DateTime.From(Number.From([#"Date/Time"]) - 1/24)
```
 
 
 
## Functionalities I was introduced to in Helen Wall's course
- Zoom Slider for line chart and Slider tooltips
- try <column name> for creating a Record out of a column with values and status
- Converting date time into a number and decrementing and incrementing it
- In Line chart Customize Series in the Paint roller to change appearnace of line shown by each measure
- AVERAGE rolling average (watch out for the divide by 7)  
   
   
```
   Wind Rolling Average = CALCULATE (
    [Wind Electricity Generated],
    DATESINPERIOD ( Dates[Date], LASTDATE ( 'Dates'[Date] ), -7, DAY )
) / 7
   
```   

 






   
 
 
 
