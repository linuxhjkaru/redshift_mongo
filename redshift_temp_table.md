# Query for count total
```sql
SELECT total_pv, cast(total_pv as float) / total_visit as pv_avr ,
   cast(total_dwell_time_total as float) / total_visit as dwell_time_total_avr ,
   total_start, cast(total_bounce as float) / total_visit as bounce_avr ,
   cast(total_leave_count as float) / total_visit as leave_count_avr
FROM (
  SELECT sum(total_visit) as total_visit , sum(total_pv) as total_pv ,
    sum(total_dwell_time_total) as total_dwell_time_total ,
    sum(total_start) as total_start , sum(total_bounce) as total_bounce ,
    sum(total_leave_count) as total_leave_count
  FROM temporary_page
) AS TOTAL_PAGE
```

# Query for count number of url
```sql
SELECT COUNT(*) FROM temporary_page
```

# Query for graph
```sql
SELECT url, total_pv
FROM temporary_page
ORDER BY total_pv DESC
LIMIT 10
```
