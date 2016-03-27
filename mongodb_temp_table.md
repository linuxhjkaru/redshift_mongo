# Query for count number of url

```sql
db.temporary_table.aggregate([
  {
    $group: {
      _id: null,
      count: {$sum: 1}
    }}
], {allowDiskUse: true})
```

# Query for count total
```sql
db.temporary_table.aggregate([
  {$group: {
   _id: null,
   pv: {$sum: "$pv"},
   cv: {$sum: "$cv"},
   dwell_time_total: {$sum: "$dwell_time"},
   bounce: {$sum: "$bounce"},
   visit: {$sum: "$visit"},
   start: {$sum: "$start"},
   leave: {$sum: "$leave"}
  }},
{$project: {
    pv: 1,
    cv: 1,
    visit: 1,
    start: 1,
    dwell_time_total_rate: {$cond: [{$eq: ["$visit",0]},
      0, {$divide: ["$dwell_time_total", "$visit"]}]},
    bounce_rate: {$cond: [{$eq: ["$start",0]},
      0, {$divide: ["$bounce", "$start"]}]},
    pv_avr: {$cond: [{$eq: ["$visit",0]},
      0, {$divide: ["$pv", "$visit"]}]},
    leave_rate: {$cond: [{$eq: ["$pv",0]},
      "$leave", {$divide: ["$leave", "$pv"]}]},
    cv_rate: {$cond: [{$eq: ["$pv",0]},
      "$cv", {$divide: ["$cv", "$pv"]}]}
  }}
], {allowDiskUse: true})
```
# Query for graph
```sql
db.temporary_table.aggregate([
  {$sort: {pv: -1}},
  {$limit: 10}
  ], {allowDiskUse: true})
```
