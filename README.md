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




   
 
 
 
