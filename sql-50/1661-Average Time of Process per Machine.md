# 1661-Average Time of Process per Machine

## 題目描述

```
Table: Activity
```sql
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
```
There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

Return the result table in any order.



excepted output
```sql
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------+
```


 
## 我的錯誤解法
```
WITH start_type AS (
    SELECT machine_id, process_id, timestamp
    FROM Activity
    WHERE activity_type = "start"
),

end_type AS (
    SELECT machine_id, process_id, timestamp
    FROM Activity
    WHERE activity_type = "end"
),

sub_time AS (
    SELECT 
        a.machine_id, 
        a.process_id,
        (b.timestamp - a.timestamp ) AS processing_time
    FROM start_type a
    JOIN end_type b
    ON a.machine_id = b.machine_id  AND a.process_id = b.process_id
)

SELECT 
    machine_id, 
    ROUND((processing_time)/COUNT(DISTINCT process_id), 3) AS processing_time
FROM sub_time

```
### 錯誤原因
- 沒有加總就除，平均的公式應該是：加總的執行時間/程式總數量，所以應該是SUM(processing_time)/COUNY(process_id)
- 沒有加上group by
- 其實也可以先算好加總後，使用AVG(SUM_processing_time)


## 正確解法
```
WITH start_type AS (
    SELECT machine_id, process_id, timestamp
    FROM Activity
    WHERE activity_type = "start"
),

end_type AS (
    SELECT machine_id, process_id, timestamp
    FROM Activity
    WHERE activity_type = "end"
),

sub_time AS (
    SELECT 
        a.machine_id, 
        a.process_id,
        (b.timestamp - a.timestamp ) AS processing_time
    FROM start_type a
    JOIN end_type b
    ON a.machine_id = b.machine_id  AND a.process_id = b.process_id
)
SELECT 
    machine_id, 
    ROUND(SUM(processing_time)/COUNT(process_id), 3) AS processing_time
FROM sub_time
GROUP BY 1
```
