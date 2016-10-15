# Nth Highest Salary

[Leetcode](https://leetcode.com/problems/nth-highest-salary/)

**題意：**

Write a SQL query to get the nth highest salary from the Employee table.
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

**解題思路：**

需要注意的就是我們先定義一個變數m，表示取出的資料中，要跳過M個之後，再選1個出來，即是我們要的數。

>LIMIT N-1, 1 ：表示跳過n-1個數之後１挑1個數出來。

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  DECLARE M INT;
  SET M = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT M, 1
  );
END
```

---
###Reference
1. https://leetcode.com/discuss/21311/accpted-solution-for-the-nth-highest-salary