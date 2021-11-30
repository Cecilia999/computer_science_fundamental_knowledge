# transaction - 事务

## 1. 什么是事务

1. A transaction is a logical unit of work that contains one or more SQL statements.

2. Transactions access data using read and write operations. In order to maintain consistency in a database, before and after the transaction, certain properties are followed. These are called ACID properties.

3. 事务通常以 BEGIN TRANSACTION 开始，以 COMMIT 或 ROLLBACK 结束。

## 2. ACID Database Transaction Property - 事务的四个特征

1. Atomicity (原子性）
   A transaction is an atomic unit. The effects of all the SQL statements in a transaction can be either all committed (applied to the database) or all rolled back (undone from the database).

   原子性是指事务包含的所有操作要么全部成功 -> COMMIT，要么全部失败回滚 -> ROLLBACK。 事务的操作如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。

   e.g.

   - 1. A has $100 in its account and send $10 to B. There are two operations here which is A sending B $10 and B receiving $10.
   - 2. If the transaction succeed, A should have $90 left in its account and B should have $10 more in its account.
   - 3. However, if part of the transaction A send B $10 failed, the whole transaction failed, B should not received $10.

2. Consistency
   the integrity of the data should be maintained/preserved

   一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。

   > 拿转账来说，假设用户 A 和用户 B 两者的钱加起来一共是 5000，那么不管 A 和 B 之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是 5000，这就是事务的一致性。

3. Isolation
   The term 'isolation' means separation, which means no data should affect the other one that may occur concurrently. Any changes that occur in any particular transaction will not be seen by other transactions until the change is committed in the memory.

   When mutiple users trying to access to the same table simultaneously, database should create each user a isolated transaction that would not affect each other. e.g. There two concurrent transaction T1 & T2. From the T1's perspective, either T2 was complete before T1 start or T2 start after T1 is complete

   隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。

   即要达到这么一种效果：对于任意两个并发的事务 T1 和 T2，在事务 T1 看来，T2 要么在 T1 开始之前就已经结束，要么在 T1 结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行。  
   **关于事务的隔离性数据库提供了多种隔离级别**

4. Durability
   Durability ensures the permanency of the transaction. Once the transaction is committed, then the modification will permanently exist.
   持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

## 3. How do you check the rows affected as part of previous transactions?

SQL standards state 3 phenomena should be prevented for concurrent transactions. SQL standards define 4 levels of transaction isolations to deal with these phenomena.

### 3.1 Dirty reads 脏读 :

If a transaction reads data that is written due to concurrent uncommitted transaction, these reads are called dirty reads.

脏读是指在一个事务处理过程里读取了另一个未提交的事务中的数据。

当一个事务正在多次修改某个数据，而在这个事务中这多次的修改都还未提交，这时一个并发的事务来访问该数据，就会造成两个事务得到的数据不一致。

> 例如：用户 A 向用户 B 转账 100 元，对应 SQL 命令如下:
> update account set money=money+100 where name=’B’;
> (此时 A 通知 B) update account set money=money - 100 where name=’A’;

当只执行第一条 SQL 时，A 通知 B 查看账户，B 发现确实钱已到账（此时即发生了脏读），而之后无论第二条 SQL 是否执行，只要该事务不提交，则所有操作都将回滚，那么当 B 以后再次查看账户时就会发现钱其实并没有转。

### 3.2 Phantom reads 虚读(幻读)

幻读是事务非独立执行时发生的一种现象。例如事务 T1 对一个表中所有的行的某个数据项做了从“1”修改为“2”的操作，这时事务 T2 又对这个表中插入了一行数据项，而这个数据项的数值还是为“1”并且提交给数据库。而操作事务 T1 的用户如果再查看刚刚修改的数据，会发现还有一行没有修改，其实这行是从事务 T2 中添加的，就好像产生幻觉一样，这就是发生了幻读。

幻读和不可重复读都是读取了另一条已经提交的事务（这点就脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）。

### 3.3 Non-repeatable reads 不可重复读

不可重复读是指在对于数据库中的某个数据，一个事务范围内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了。

> 例如事务 T1 在读取某一数据，而事务 T2 立马修改了这个数据并且提交事务给数据库，事务 T1 再次读取该数据就得到了不同的结果，发送了不可重复读。

不可重复读和脏读的区别是，脏读是某一事务读取了另一个事务未提交的脏数据，而不可重复读则是读取了前一事务提交的数据。

在某些情况下，不可重复读并不是问题，比如我们多次查询某个数据当然以最后查询得到的结果为主。但在另一些情况下就有可能发生问题，例如对于同一个数据 A 和 B 依次查询就可能不同，A 和 B 就可能打起来了……

## 4. 四种隔离级别

1. Read Uncommitted – The lowest level of the isolations. Here, the transactions are not isolated and can read data that are not committed by other transactions resulting in dirty reads.
2. Read Committed – This level ensures that the data read is committed at any instant of read time. Hence, dirty reads are avoided here. This level makes use of read/write lock on the current rows which prevents read/write/update/delete of that row when the current transaction is being operated on.
3. Repeatable Read – The most restrictive level of isolation. This holds read and write locks for all rows it operates on. Due to this, non-repeatable reads are avoided as other transactions cannot read, write, update or delete the rows.
4. Serializable – The highest of all isolation levels. This guarantees that the execution is serializable where execution of any concurrent operations are guaranteed to be appeared as executing serially.

5. Serializable (串行化)：可避免 **脏读、不可重复读、幻读** 的发生。
   > A 事务未提交，B 事务就等待。
6. Repeatable read (可重复读)：可避免 **脏读、不可重复读**的发生。
   > A 只能读取到 B 已经提交的事务。并且 A 事务中两次读到的内容一致，A 事务结束后再读取会读取到 B 提交事务。
7. Read committed (读已提交)：可避免 **脏读** 的发生。
   > A 只能读取到 B 已经提交的事务。并且 A 事务中两次读到的内容不一致，缘由就是 B 提交事务。
8. Read uncommitted (读未提交)：最低级别，任何情况都无法保证。
   > A 可以读取到 B 还未提交的事务。

## 参考

https://www.jianshu.com/p/428d3dea3f25
