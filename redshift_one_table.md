# Query for count total
```sql
SELECT total_pv, cast(total_pv as float) / total_visit as pv_avr ,
  cast(total_dwell_time_total as float) / total_visit as dwell_time_total_avr , total_start, cast(total_leave_count as float) / total_visit as leave_count_avr , cast(total_bounce as float) / total_visit as bounce_avr
FROM (
  SELECT sum(visit) as total_visit , sum(pv) as total_pv , 
    sum(dwell_time_total) as total_dwell_time_total , sum(leave_count) as 
    total_leave_count , sum(bounce) as total_bounce , sum(start) as total_start
  FROM test_test
  WHERE date BETWEEN '2016-02-12' AND '2016-03-25' AND client_id = 'c16012900556'
  HAVING sum(visit) > 0
);
```

# Query for graph
```sql
SELECT url, total_pv
FROM (
  SELECT sum(pv) as total_pv, url
  FROM page_logs
  WHERE date BETWEEN '2016-02-12' AND '2016-03-25' AND client_id = 'c16012900556'
  GROUP BY url
)
ORDER BY total_pv DESC
LIMIT 10
```

# Query for count number url
```sql
SELECT COUNT(8)
FROM (
  SELECT url
  FROM test_test
  WHERE date BETWEEN '2016-02-12' AND '2016-03-25' AND client_id = 'c16012900556'
  GROUP BY url
)
```
