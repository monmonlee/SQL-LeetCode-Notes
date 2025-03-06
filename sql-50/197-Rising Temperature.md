# 197. Rising Temperature

## 題目描述
找到那些比昨天熱的日期ID

## 表格結構
```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.
 
```

重點解法：self join / date 函數 / LAG(only in BQ) 


## 我的錯誤解法

我不知道要如何兩兩互減

## 正確解法
```
SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature
'''
OR:
ON w2.recordDate = DATE_SUB(w1.recordDate, INTERVAL 1 DAY)
''' 
```

## 額外筆記_日期函數
1. DATE_SUB(w1.recordDate, INTERVAL 1 DAY)：找recordDate的前一天
2. DATE_ADD(w1.recordDate, INTERVAL 1 DAY)：找recordDate的後一天
3. DATE_DIFF("2022-01-03", "2022-01-01")：2(時間差，前減後)

## 額外筆記_LAG()：ONLY IN BQ，效能最好
```
SELECT id 
FROM(
  SELECT 
    id, 
    temperature,
    recordDate,
    LAG(temperature, 1) OVER (ORDER BY recordDate) AS prev_temperature,
    LAG(recordDate, 1) OVER (ORDER BY recordDate) AS prev_date,
  FROM Weather
)
WHERE temperature > prev_temperature
AND DATE_DIFF(recordDate, prev_date, DAY) = 1
# BQ 的語法有第三個參數DAY, MYSQL沒有
```


