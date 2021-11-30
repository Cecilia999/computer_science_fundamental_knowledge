# Database interview questions

## Referrence:

1. https://www.interviewbit.com/sql-interview-questions/

## 1. DBMS VS RDBMS

1. DBMS:
   Provides an organized way of managing, retrieving and storing from collection of logically related information.
2. RDBMS
   Provides the same as that of DBMS, but it provides with relational integrity (mostly stored in the form of tables)

## 2. Explain the terms database and DBMS. Also, mention the different types of DBMS

1. database:
   you can consider database as a large container where all your data present in a logical format. it is basically an organized collection of data, than stored and retrived in a remote/local computer system.
2. DBMS
   A software application which interacts with databases, applications, and users to create/retrive/update/manage database. It ensures that our data is consistent, organized, and is easily accessible.

   - Hierarchal DBMS: has a tree structure, with nodes represent the records and the branches of tree represent the fields.

   - network: support many-to-many relation and mutiple users records can link

   - relational: stores data in the form of a collection of tables, and relations can be defined between the common fields of these tables.

   - object-oriented: records are stored as a object

## 3. what is the advantage of DBMS

1. data independence:
   allows to changing the data structure without affecting any running applications program.
2. sharing of data
   mutiple users can access the same database simultaneously
3. Data Integrity
   can ensure that the data is correct and consistent in all the databases and for all the users. Data Integrity is the assurance of accuracy and consistency of data over its entire life-cycle
4. reduce redundancy
   it supports the mechanism to control the redundancy of data by integrating all the data into a single databse
5. provide backup and recovery facility
   automatically takes care of backup and recovery.

## 4. What do you understand by query optimization?

query does not run in the order of the qeury statement. There is a **query optimizer** which would choose the fast path among all the **query plan**

1. The output is provide faster
2. reduces space and time complexity
3. a larger number of queries can be executed in less time

## 5. Do we consider NULL values the same as that of blank space or zero

Not exactly. We consider NULL value as a value that is not avaliable/unknown. Wheras zero is a number and blank space is a character

## 6. what do you understand atomicity and aggregation

1. atomicity:

   This property states that a database modification must either follow all the rules or nothing at all. So, if one part of the transaction fails, then the entire transaction fails

2. aggregation

   meaning aggregate the collected entities and their relationship
   e.g.
   在数据库技术中，Aggregation function 又称之为 set function，其含义为输入为一个 set，输出为聚合结果。具体包括：COUNT() / AVE() / MIN() / MAX() / SUM()

## 7. what are the different levels of abstraction in the DBMS

1. Physical level
   It is the lowest level of abstraction for DBMS which defines how the data is actually stored, the data-structures and access methods used by the database. (由 design DBMS 的 developer 决定)
2. logcial level
   Logical level is the intermediate level or next higher level.  
   It describes what data is stored in the database and what relationship exists among those data. ------------>>>>>>> contains tables (fields and attributes) and relationships among table attributes.
3. view level
   It is the highest level. In view level, there are different levels of views and every view only defines a part of the entire data. It also simplifies interaction with the user and it provides many views or multiple views of the same database.

## 8. what is entity, entity type and entity set in DBMS

1. entity
   An entity is a real-world object having attributes, which are nothing but characteristics of that particular object
2. entity type
   Entity type is a collection of entities, having the same attributes.
   Generally, an entity type refers to one or more related tables in a particular database
3. entity set
   An entity set is the collection of all the entities of a particular entity type in a database. For example, a set of employees, a set of companies, etc

## 9. what are relationships and mention different types of relationship

A relationship in database is established when one table has a foreign key that references the primary key of another table.

1. One-to-One - can be defined as the relationship between two tables where each record in one table is associated with the maximum of one record in the other table.
   **e.g. each class 只能有一个 professor**
2. One-to-Many & Many-to-One - This is the most commonly used relationship where a record in a table is associated with multiple records in the other table.
   **e.g. 每个学生可以 enroll multiple courses**
3. Many-to-Many - This is used in cases when multiple instances on both sides are needed for defining a relationship.
   **e.g. courses can be taught by different professores & a professor can teach different courses**

## 10. what is join? Mention its types.

Join depicts the relationship between two or more tables. Join is used/is refering to combine different table rows by using a column/field that is common to each of the tables.

1. inner join: (default join) return rows from both tables that satisfy the given condition.

2. outter join

   - left join:
     The LEFT JOIN clause select all the rows in left table and matches each row from the right table based on the condition of the ON clause. It would return all the rows in the left table even there is no match to be found in the right table (with NULL in each column from the right table).

   - right join:
     same as left join but reversely
   - full join:
     combines the results of both left and right outer joins. The joined table will contain all records from both the tables and fill in NULL values for missing matches on either side.

3. cross join： Cross join can be defined as a cartesian product of the two tables included in the join.

4. natural join: combines tables based on columns with the same name and data type.

https://learnsql.com/blog/sql-join-interview-questions-with-answers/

## 11. how many types of database languages exist?

1. DDL– Data Definition Language which includes CREATE, ALTER, DROP.
2. DML– Data Manipulation Language which includes SELECT, UPDATE, INSERT, etc.
3. DCL– Data Control Language which consists of GRANT and REVOKE.
4. TCL– Transaction Control Language such as COMMIT and ROLLBACK.

## 12. [what is ACID transation features](transaction.md)

## 13. What is database index(索引)? Explain its types:

A database index is a data structure that provides a quick lookup of data in a column or columns of a table. It enhances the speed of operations to access data from a database table at the cost of additional writes and memory to maintain the index data structure.

1. a database can have many index and each index can be created by more than one column
2. [MySQL index explaination](MySql_index.md)

- Unique and Non-Unique Index:
- Clustered and Non-Clustered Index(also called secondary index):

## 14. what are the importance of partitioning in DBMS

1. Table Partition is to split a large table into a collection of smaller parts.
   e.g. We have 100 million records in student table, and we can partition the table by the student register year, etc.
2. The importance of partition is that it improves the performance and allows mutiple operating system(each operating system need a separate patition of its own)

## 15. filtering in SQL

1. WHERE
2. GROUP BY + HAVING

WHERE clause cannot filter aggregated records, it is processed before COUNT(\*) and GROUP BY, in this can if you want to filter the data you can only use HAVING

```SQL
SELECT last_name, COUNT(*)
FROM employee
WHERE COUNT(*)>1  //error: group function is not allow here;
GROUP BY last_name:
```

```SQL
SELECT last_name, COUNT(*)
FROM employee
GROUP BY last_name
HAVING COUNT(*)>1:
```

## 16. [what is cursor and how to use it?](https://www.cnblogs.com/xiongzaiqiren/p/sql-cursor.html)

## 17. what is normalization? what is denormalization? what are the various forms of normalization?

### 17.1. what is normalization

- a technique to organize data in database, which reduce redundacy data and improves data intergrity, ensure data indenpendancy based on some rules
- normalization came into existence because of the problem of data anomalies. If a table is not properly normalized and has data redundancy, it would eat up extra memory and make the data difficult to handle and update.
- insertion / updation / deletion anomaly

### 17.2. what is denormalization?

Denormalization is the inverse process of normalization, when we over normalized data schema.

### 17.3. what are the various forms of normalization?

1. 1NF(First Normal Form)
   First Normal Form describes that every attribute in that relation / in the relation of 1NF is a single-valued attribute 不能一个 row 存两个 records

2. 2NF
   A relation is in second normal form if it satisfies 1NF and does not contain any **partial dependency**.

   - all the candidate key should be set to primary key
   - **CANDIDATE KEY** is a set of fields(columns/attributes) that can uniquely identify records in a table. Candidate Key is a SUPER KEY with no repeated attributes.
   - For 2NF: 如果一个 table 的 candidate key 不是他的 primary key，应该把它分成两个 or 多个 table，使得 candidate key 都是 primary key

3. 3NF
   A relation is said to be in the third normal form, if it satisfies the conditions for the second normal form and there is no **transitive dependency between the non-prime attributes**
   e.g.
   student_id student_name subject_id subject_name;
   student_id determines subject_id and subject_id determines subject_name;  
   this is not a 3NF, 应该 split table to student_id student_name subject_id vs subject_id subject_name

4. BCNF(Boyce-Codd Normal Form/ 4NF)
   a relation in BCNF satisfy 3NF and every functional dependency A->B (A determines B), A should be the SUPER KEY of that particular table
   - **SUPER KEY**: is a set of one or more attributes (columns/fields), which can uniquely identify a row in a table. 即 CANDIDATE KEY 中的一个或多个，都叫做 SUPER KEY

## 18. Can you explain the architecture of PostgreSQL?

The architecture of PostgreSQL follows the client-server model.
The server side comprises of background process manager, query processer, utilities and shared memory space which work together to build PostgreSQL’s instance that has access to the data. The client application does the task of connecting to this instance and requests data processing to the services. The client can either be GUI (Graphical User Interface) or a web application. The most commonly used client for PostgreSQL is pgAdmin.

## 19. What do you understand by multi-version concurrency control - MVCC?

MVCC or Multi-version concurrency control is used for avoiding unnecessary database locks when 2 or more requests tries to access or modify the data at the same time.
