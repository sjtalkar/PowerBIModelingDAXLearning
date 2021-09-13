> [**Thinking in terms of a table**](https://www.youtube.com/watch?v=Hqs0lJJq1YA)
To get the first Sales date of a sales of a product 
- To note below:
-   When the Product table is summarized, the aggregated values is obtained through a Calculate - since the grouping is a group table.
-   The measure starts with creating this table of Product Key and minimum sales amount for each product key
-   It has to return a  Scalar value
-   It applies a context transition and uses this table in a Calculate to return the Sales amount

DEFINE
    MEASURE 'Sales'[SalesOnFirstSalesDate] =
        VAR ProductAndFirstDate =
            ADDCOLUMNS (
                SUMMARIZE ( 'Product', 'Product'[ProductKey] ),
                ---Note that having an expression aggregation breaks the lineage
                "@FirstSale", CALCULATE ( MIN ( Sales[Order Date] ) )
            ) --Note: Restore the lineage        
        VAR ProductsAndFirstDateWithLineage =
            TREATAS ( ProductAndFirstDate, 'Product'[ProductKey], Sales[Order Date] )
        RETURN
            CALCULATE ( [Sales Amount], ProductAndFirstDate )
EVALUATE
ADDCOLUMNS (
    VALUES ( 'Product'[Brand] ),
    "SalesOnFirstSaleDate", [SalesOnFirstSalesDate]
)




> **[NATURAL LEFTOUT and INNER joins](https://www.youtube.com/watch?v=eoEbnQyianU)**

> - Lineage broken through concatenation with & ""
> - TREATAS To select specific values
DEFINE
    VAR StoresByCountry =
        SELECTCOLUMNS (
            TREATAS ( { "Armenia", "Australia", "Denmark" }, Store[CountryRegion] ),
            "Country", Store[CountryRegion] & "",
            "NumOfStore", CALCULATE ( COUNTROWS ( Store ) )
        )

 VAR CustomersByCountry =
        SELECTCOLUMNS (
            TREATAS ( { "Armenia", "Australia", "Denmark" }, Customer[CountryRegion] ),
            "Country", Customer[CountryRegion] & "",
            "NumOfStore", CALCULATE ( COUNTROWS ( Customer ) )
        )
EVALUATE
StoresByCountry
EVALUATE
CustomersByCountry

EVALUATE NATURALLEFTOUTERJOIN (StoresByCountry, CustomersByCountry)

EVALUATE NATURALINNERJOIN (StoresByCountry, CustomersByCountry)
