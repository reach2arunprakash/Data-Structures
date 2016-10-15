# Second Highest Salary

[Leetcode](https://leetcode.com/problems/second-highest-salary/)

題意：

Write a SQL query to get the second highest salary from the Employee table.
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the second highest salary is 200. If there is no second highest salary, then the query should return null.

解題思路：

從所有小於最大值的所有元素中選出最大的即可。

```sql
SELECT MAX(Salary) 
    FROM Employee 
    WHERE Salary < (SELECT MAX(Salary) FROM Employee)
```


---
###Reference
1. https://leetcode.com/discuss/23759/my-tidy-soution