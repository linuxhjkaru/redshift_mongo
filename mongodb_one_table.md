
# Query for count number of url
```ruby
db.totalize_pages.aggregate([
 {$match: {
   client_id: "c16012900556", date: {$gte: ISODate("2016-02-12T00:00:00Z")}
 }},
 {$group: {
   _id: {url: "$url"},
  }},
  {
    $group: {
      _id: null,
      count: {$sum: 1}
    }}
], {allowDiskUse: true})
```

#Query form count total
```sql
db.totalize_pages.aggregate([
 {$match: {
   client_id: "c16012900556", date: {$gte: ISODate("2016-02-12T00:00:00Z")}
 }},
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

# Query get data for graph
```sql
db.totalize_pages.aggregate([
 {$match: {
   client_id: "c16012900556", date: {$gte: ISODate("2016-02-12T00:00:00Z")}
 }},
 {$group: {
   _id: {page: "$url", title: "$title"},
   pv: {$sum: "$pv"},
  }},
  {$sort: {pv => -1}},
  {$limit: 10}
], {allowDiskUse: true})
```
