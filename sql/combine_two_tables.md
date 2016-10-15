# Combine Two Tables

[Leetcode](https://leetcode.com/problems/combine-two-tables/)

**題意：**

**Table: Person**
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
```
PersonId is the primary key column for this table.

**Table: Address**
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
```
AddressId is the primary key column for this table.

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:
```
FirstName, LastName, City, State
```

**解題思路：**

在這裡使用left join來幫助我們解這道題，left join表示以左邊的表格為主，加上用on 的keyword把符合條件的合起來。

```sql
SELECT Person.FirstName, Person.LastName, Address.City, Address.State 
FROM Person LEFT JOIN Address ON Person.PersonId = Address.PersonId;

```


---
###Reference
1. https://leetcode.com/discuss/21216/its-a-simple-question-of-left-join-my-solution-attached