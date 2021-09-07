# [Performance Webinar with Marco Russo](https://www.youtube.com/watch?v=C63Jq5-C5go)

### Live query versus Direct Query
- Live Query is faster since you do not have another Analysis Services database as a middleman
- Direct query allows for an Analysis Services that is local into which you can bring in data from other sources

### Question on performance in a dataset with 250 M rows AT 20 mins into above video
  - Do not move to Direct Query, it will make the performace worse.
  - Partition and introduce incremental refresh
  - Set a partition strategy - set data into ranges based on some column

### Performance in Power Query
  - Look into tranformations in Power Query
  - Sometimes the values are cached and the performance downgrade may not be evident

### Power query or SQL transformations AT 33 mins into video
  - If you are a developer perform most of your transformations in a view rather than in Power Query
  - Performance in Power Query can never be better than what you can get in the database. 
  - If possible and allowed move all your tansformations as view into the database.
  - Power query is a simpler tool mainly for non-developers and when you are connecting to non-relational database sources.
  

