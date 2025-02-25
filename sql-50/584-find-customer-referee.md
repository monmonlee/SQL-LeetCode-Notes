# 584. Find Customer Referee

## 題目描述
找出所有不是由ID=2的客戶推薦的客戶名稱。

## 表格結構
```sql
Customer表:
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
```
## 我的錯誤解法
```
SELECT name FROM Customer WHERE referee_id != 2;
```
###錯誤原因
忽略了NULL值的特殊處理
在SQL中NULL不等於任何值，甚至不等於NULL自己
WHERE referee_id != 2 不會匹配referee_id為NULL的記錄

## 正確解法
```
SELECT name FROM Customer WHERE referee_id != 2 OR referee_id IS NULL
```
