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
  

