# 1378. Replace Employee ID With The Unique Identifier

## 題目描述

Table: Employees
```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.
 
```
Table: EmployeeUNI
```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```
(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.
 

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

Return the result table in any order.

The result format is in the following example.




excepted output
```sql
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```


 
## 我的錯誤解法
```
SELECT 
unique_id , 
name
FROM Employees a
LEFT JOIN EmployeeUNI b
ON a.id = b.id
GROUP BY 1,2
```
### 錯誤原因
- 錯誤理解Table: EmployeeUNI的Column Name是對應唯一值
- 因此如果加上group by，會導致同姓名有id和沒id的合併，null就會不見
- 例如以下圖表，如果有group by，那不會返回null, alice，只會有3, alice
```sql
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| 3         | Alice  |
| 1         | Jonathan |
+-----------+----------+
```

## 正確解法
```
SELECT 
unique_id , 
name
FROM Employees a
LEFT JOIN EmployeeUNI b
ON a.id = b.id
```
