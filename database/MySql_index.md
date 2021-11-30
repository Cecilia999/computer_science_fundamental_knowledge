# MySQL - index

## 1. MySQL - index 介绍

1. 文字版: https://blog.csdn.net/wangfeijiu/article/details/113409719
2. 视频版: https://www.bilibili.com/video/BV1ov41137Vo?p=2&spm_id_from=pageDriver

## 2. SQL index interview questions and answers

1. https://www.interviewbit.com/sql-interview-questions/
2. https://www.tutorialcup.com/interview/sql-interview-questions/indexes-sql.htm#1_What_is_an_index

## 3. Index types difference

### 3.1 unique vs non-unique index

### 3.2 clustered index vs non-clustered index:

聚簇索引的顺序就是数据的物理存储顺序，而对非聚簇索引的索引顺序与数据物理排列顺序无关。举例来说，你翻到新华字典的汉字“爬”那一页就是 P 开头的部分，这就是物理存储顺序（聚簇索引）；而不用你到目录，找到汉字“爬”所在的页码，然后根据页码找到这个字（非聚簇索引）。

**一文了解什么 clustered index 和 non-clustered index 的区别：https://blog.csdn.net/wen_3370/article/details/56667906**

1. structure & preserve order: Clustered index store the same way/order as how records are physically stored in a database (based on the indexed column). Non-Clustered Index is similar to the index of a book, which has index stored in a place and data stored in another place.

2. speed: fetching records from the non-clustered index is relatively slower than from Clustered index.

3. 数量: In SQL, a table can have a single clustered index whereas it can have multiple non-clustered indexes. 一个表只能有一个 clustered index（聚簇索引），但可以有 multiple non-clustered index

### 3.3 mysql 的 clustered index vs non-clustered index:

MySql would create clustered index for PRIMARY KEY automatically.

1. 中文介绍：https://www.freesion.com/article/78641406633/
2. mysql 文档：https://dev.mysql.com/doc/refman/5.7/en/innodb-index-types.html
