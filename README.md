# PowerBI  Calculation Groups

> Requirements - Tabular Editor to create and maintain

> Calculation groups can apply **specific calculations** on top of **existing DAX measures**. 

> Offers expediency when time intelligence calculations like year-to-date, year-over-year, and currency conversion have to be applied on a variety of measures.

> Can be used to enable the selection of a measure in an existing report through a slicer.

### CALCULATED ITEMS
> Create a Calculation Group in Tabular Editor and add Calculation Items that act upon the SELECTEDMEASURE()
-Each Calculation Item will act on a SELECTEDMEASURE()
- The Expression of a Calculated Item can go something like 
 `CALCULATE (SELECTEDMEASURE(), SAMEPERIODLASTYEAR(DimDate[Date])`

- But if you have a dedicated measure for the calculation group then it will is advisable to use the IF( ISELECTEDMEASURE....syntax shown below)

- The ordinal of the calculated item detemines the sorting order 

- The formatting of the Calculated Item
  `#,0" or "0.00%" or "\$#,0.00;(\$#,0.00);\$#,0.00"


### Create a measure within the Calculation Group for a dedicated measure for a group
- Set up a Base measure of each calculated group - for example one calculated group works on Sales Amount and the other works on Sales Qty.

- These are dummy (base) measures called [Calc Group Measure Amount] and [Calc Group Measure Qty]
 
 `IF ( ISSELECTED([Calc Group Measure Amount], [Sales Amount], SELECTEDMEASURE())`

` IF ( ISSELECTED([Calc Group Measure Qty], [Sales Qty], SELECTEDMEASURE())`
 
 
 
