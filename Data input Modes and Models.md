# NOTES FOR DATA INPUT MODES AND  MODELING

[Marco Russo's Video on Composite Models](https://www.youtube.com/watch?v=9lkVk4t2qL0)

1. In Options and Settings: Options
    Preview Features : Select Direct Quesry for PBI Datasets and AS (Analysis Services) - Save and restart
    This wil result in a message at the lower bottom corner of the file which will say: Make changes to this model.
    If you opt to change the model then a Popup will appear.
    The Pop up tells you that if you change the model then the love connection will change into Direct Query and and then you will have a local model will be added to the PBIX.
    Also this change is permanent.


![Source]https://www.youtube.com/watch?v=L3uT-cn_eO8

Points noted:
- Keep the Fact table narrow
- Millions of rows can be imported
- Dimensions with millions of rows can be direct queried
- Incremental refreshes can be performed on tables with millions of rows
