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
