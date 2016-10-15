# SQL

1) What is the difference between inner and outer join? Explain with example.

	- Inner join returns rows when there is at least one match in both tables
	- Outer join will return matching rows from both tables as well as any unmatched rows from one or both the tables 
	
2) What is the difference between JOIN and UNION?

	- SQL JOIN allows us to “lookup” records on other table based on the given conditions between two tables. 
	- UNION operation allows us to add 2 similar data sets to create resulting data set that contains all the data from the source data sets. Union does not require any condition for joining.
	
3) What is the difference between UNION and UNION ALL?	
	- UNION operation returns only the unique records from the resulting data set  
	- UNION ALL will return all the rows, even if one or more rows are duplicated to each other.

4) What is the difference between WHERE clause and HAVING clause?
	- WHERE clause can only be applied on a static non-aggregated column 
	- we will need to use HAVING for aggregated columns.
	
5) What is the difference among UNION, MINUS and INTERSECT?
	- UNION combines the results from 2 tables and eliminates duplicate records from the result set.
	- MINUS operator when used between 2 tables, gives us all the rows from the first table except the rows which are present in the second table.
	- INTERSECT operator returns us only the matching or common rows between 2 result sets.
	
6) How can we transpose a table using SQL (changing rows to column or vice-versa) ?
	- The usual way to do it in SQL is to use CASE statement or DECODE statement
	
7) How to generate row number in SQL Without ROWNUM
	- SELECT name, sal, (SELECT COUNT(*)  FROM EMPLOYEE i WHERE o.name >= i.name) row_num
		FROM EMPLOYEE o 
		order by row_num
	
8) How to select first 5 records from a table?
	sql server -> SELECT TOP 5 * FROM EMP;
	Oracle -> SELECT * FROM EMP WHERE ROWNUM <= 5;
	Generic -> SELECT  name  FROM EMPLOYEE o
			   WHERE (SELECT count(*) FROM EMPLOYEE i WHERE i.name < o.name) < 5
			   
======================= SQL server questions ==========================================
9) What are DMVs? - 
	Dynamic Management Views (DMVs), are functions that give you information on the state of the server. DMVs, for the most part, are used to monitor the health of a server. They really just give you a snapshot of what’s going on inside the server. They let you monitor the health of a server instance, troubleshoot major problems and tune the server to increase performance			   

10) Define a temp table 
	In a nutshell, a temp table is a temporary storage structure. What does that mean? Basically, you can use a temp table to store data temporarily so you can manipulate and change it before it reaches its destination format.	
	
11) What’s the difference between a local  temp table and a global temp table? 
	Local tables are accessible to a current user connected to the server. These tables disappear once the user has disconnected from the server. Global temp tables, on the other hand, are available to all users regardless of the connection. These tables stay active until all the global connections are closed.
	
12) Describe the difference between truncate and delete 
	Delete command removes the rows from a table based on the condition that we provide with a WHERE clause. 
	Truncate will actually remove all the rows from a table and there will be no data in the table after we run the truncate command.
	
13) What is a view?
	A view is simply a virtual table that is made up of elements of multiple physical or “real” tables. Views are most commonly used to join multiple tables together, or control access to any tables existing in background server processes.	
	
14) What is the default port number for SQL Server? -
	Basically, when SQL Server is enabled the server instant listens to the TCP port 1433.	It can be changed from the Network Utility TCP/IP properties.

15) What are the difference between clustered and a non-clustered index?
	A clustered index is a special type of index that reorders the way records in the table are physically stored. Therefore table can have only one clustered index. The leaf nodes of a clustered index contain the data pages.
	
	A non clustered index is a special type of index in which the logical order of the index does not match the physical stored order of the rows on disk. The leaf node of a non clustered index does not consist of the data pages. Instead, the leaf nodes contain index rows.
	
16) What is PRIMARY KEY?
	A PRIMARY KEY constraint is a unique identifier for a row within a database table. Every table should have a primary key constraint to uniquely identify each row and only one primary key constraint can be created for each table. The primary key constraints are used to enforce entity integrity.
	
17) What is FOREIGN KEY?

	A FOREIGN KEY constraint prevents any actions that would destroy links between tables with the corresponding data values. A foreign key in one table points to a primary key in another table. Foreign keys prevent actions that would leave rows with foreign key values when there are no primary keys with that value. The foreign key constraints are used to enforce referential integrity.
	
	16) What's the difference between a primary key and a unique key?
	Both primary key and unique key enforces uniqueness of the column on which they are defined. 
	But by default primary key creates a clustered index on the column, where are unique creates a non-clustered index by default. 
	Another major difference is that, primary key doesn't allow NULLs, but unique key allows one NULL only.	

18) What are the advantages of using Stored Procedures?
	Stored procedure can reduced network traffic and latency, boosting application performance.
	Stored procedure execution plans can be reused, staying cached in SQL Server's memory, reducing server overhead.
	Stored procedures help promote code reuse.
	Stored procedures can encapsulate logic. You can change stored procedure code without affecting clients.
	Stored procedures provide better security to your data.	
	
//SQL INTERVIEW QUESTIONS
1. What id Denormalization?
A: It is the process of improving the performance of the database by adding redundant data.
2. What is Normalization?
A: It is the process of eliminating redundant data and maintaining data dependencies.
3. ACID Properties?
A: Atomicity: It ensures all-or-none rule for database modifications.
   Consistency: Data values are consistent across the database.
   Isolation: Two transactions are said to be independent of one another.
   Durability: Data is not lost even at the time of server failure.
4. Different types of Triggers?
A: They are three different types of triggers:
   1. DML Triggers: These are of two kinds:
   		1. Instead of Triggers: These are invoked in place of the triggering action such as insert, update or delete.
   		2. After Triggers: These are invoked after the triggering action such as insert, update or delete.
   2. DDL Triggers: These are invoked against DDL statements. These are always After Triggers.
   3. Logon Triggers: These are invoked when a Logon event occurs and before the user session is established.
5. What is a linked server?
A: Linked server facilitates the ease of linking with heterogenous servers. Using linked servers, you can manipulate data on the remote servers and even integrate with local data. Stored procedures: sp_linkedservers gives you the list of linked servers available on the server.
6. What is a cursor?
A: Cursor is a database object used to manipulate data in row-by-row basis. Steps involved: Declare a cursor -> Open a cursor -> Fetch row from the cursor -> Process the row fetched -> Close the cursor -> Deallocate the cursor.
7. Difference between user defined functions and stored procedures?
A: User defined functions can be used anywhere in the queries i.e. within where/having/select section where as stored procedures cannot be used. Note: Stored procedures can be used with insert statements. UDFs can also be used in join operations and UDFs can be used to return tables which can be joined with other tables.
8. How does truncate and delete operation effect Identity?
A: Truncate resets the Identity to its base value where as delete doesnot reset it to the base value.
9. How does primary key constraint and unique key constraint effect null?
A: Primary key constraints allow no null values in the specified column where as unique key constraint allows a single null value not multiple null values.
10. What is DEFAULT?
A: Default allows to add values to the column if the value of that column is not set. Default can be defined on number and datetime fields they cannot be defined on timestamp and IDENTITY columns.
11. What is Optimistic Locking and Pessismist locking?
A: In optimistic locking, once you read a record, you verify the version number associated with the record is not changed when you write the record back. If the version number associated with the record is changed, then the transaction needs to be restarted and new values needs to be inserted.
Pessismist locking is locking the record exclusively. They are of 4 isolation levels associated: readuncommitted, readcommitted, repeatable read and serializable. Serializable is the highest isolation level and its more advanced compared to optimistic locking.
12.Difference between exclusive lock and update lock?
A: In case of exclusive lock, no other lock can be acquired on that row or table. Every process has to wait until the process which holds the lock releases it.
In case of update lock, while reading the row or a record, you can have any other lock associated with that row or record. In case of updating the record, update lock changes itself to exclusive lock and no other process can obtain a lock on that row until the lock is released.
13. What is collation?
A: Collation defines a set of rules that determines how data is sorted and compared. Once the collation has been defined, you cannot change the collation rules until you re-create it or drop the entity.
14. How to check collation associated with a database?
A: SELECT collation_name from sys.databases where name = 'DATABASENAME'
15. What are DMV's and DMF's?
A: Data management views and data management functions provide information about the state of sql server in other words they are responsible for providing information about health of the sql server.
16. What are statistics?
A: Statistics define how well a query can be executed with low resource consumption.
17. What is difference between UNION and UNION ALL?
A: UNION is used for fetching distinct records across tables. UNION ALL displays all the records including duplicates. The processing time for UNION is more when compared to UNION ALL.
18. What is blocking?
A: Sql server blocking occurs when one connection holds a lock on a record and other connection tries to fetch the record or update the record.
19. How deadlock is resolved?
A: Deadlock is automatically resolved by Sql server. Sql server identifies the process which has less overhead and accordingly it rollbacks the transaction associated with that process.
20. What are row constructors?
A: Row constructors allow you to insert multiple rows of data with a single insert statement.
21. What is OUTPUT clause?
A: OUTPUT clause is used for displaying the records effected by the use of the DML statements.
22. Requirements of a sub query?
A: Sub query has following requirements: A sub query needs to be enclosed in parenthesis. 2. A subquery cannot have order by clause. 3. A query can have more than one sub-query. 4. A sub query needs to be on right hand side of the comparison operator.
23. What is a filegroup?
A: Filegroup is a collection of datafiles which are managed as a single unit. You can have one primary filegroup per database and many user defined filegroups. Log files cannot be part of a filegroup due to difference in structure.
24. Is table size reduced, when you delete data from the table?
A: No the table size is not reduced, indeed the sql server marks those rows as free rows. Once you insert the new data, the free rows will get updated and then the size of the table is changed based on the data insertion. If the data is not inserted, then after a while, the rows are eliminated.
25. Difference between GETDATE and SYSDATETIME()?
A: GETDATE uses the precision in milliseconds and SYSDATETIME in 100 nanoseconds.
