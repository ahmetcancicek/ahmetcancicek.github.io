+++

title = "Transaction Isolation Level"
date = "2024-05-17"
description = "The concepts of concurrency anomalies and transaction isolation levels are related terms in database management systems."
[taxonomies]
tags = ["transaction", "isolation", "acid","concurrency","level","dirty-read","nonrepeatable-read","phantom-read","read-uncommitted","read-committed","repeatable-read","serializable"]

+++

## INTRODUCTION

The concepts of concurrency anomalies and transaction isolation levels are related terms in database management systems. Concurrency anomalies refer to consist problems when a large number of database transactions execute concurrently. During execution concurrently, transactions threaten the data integrity of each other. To aim of entrusting data integrity, it is used different transaction isolation levels attributed to the degree of isolation between transactions. Consequently, choosing the appropriate isolation level mitigates these concurrency anomalies.   

In a database management system, important properties known as ACID (Atomicity, Consistency, Isolation, Durability) must be considered carefully. These properties should be provided in order to ensure data integrity, reliability, and transaction consistency. In the scope of this article, the concurrent transaction anomalies and isolation, which is one of the four properties are explained.

## ISOLATION

When transactions execute concurrently, if there is no mechanism to control the operation, it is not possible to provide data integrity, reliability, and consistency in a database management system and they threaten the data integrity in the system. That's why reason, the term isolation, which refers to what level of how executed concurrently transactions affect each other, is proposed. 

Specifically, each transaction should run independently from other transactions in database management systems. if there is no independence, each transaction's execution affects the running of the others. As a result, there is a danger of not providing data integrity and reliability.

For instance, assuming that a group of transactions runs on the same resources such as a row in the database. In this scenario, when a transaction updates a row, and another transaction deletes the same row at the same time, there might be concurrency problems probably because of affecting each other. Indeed, the concurrency problems occur because the transactions depend on each other. 

To avoid concurrence problems in a database management system, it should be chosen an appropriate level of dependence for different scenarios. This dependence is achieved through the concept of isolation. The isolation level is provided and implemented by the database management system.

## CONCURRENCY ANOMALIES

While more one than transaction is executed simultaneously in a system to access and modify the same data, it might occur several unexpected behaviors or inconsistencies. They are generally known as concurrency anomalies. In the scope of database transactions, there are three types of concurrency anomalies such as dirty read, non-repeatable read, and phantom read. 

### DIRTY READ

A dirty read occurs when a transaction reads data that has not yet been committed. For instance, suppose that there are two transactions, named Transaction 1 and Transaction 2. In this scenario, Transaction 1 updates a row, and Transaction 2 reads the updated row before Transaction 1 commits. In this situation, if Transaction 1 rolls back the change, Transaction 2 will read data that is effectively nonexistent.

| **Transaction 1** | **Transaction 2** |
| :---------------: | :---------------: |
|        \|         |        \|         |
|     write(x)      |        \|         |
|        \|         |        \|         |
|        \|         |      read(x)      |
|        \|         |        \|         |
|     rollback      |        \|         |
|        \|         |        \|         |
|         v         |         v         |

### NON-REPEATABLE READ

In a system in which many transactions run simultaneously when a transaction updates and commits a row that another transaction has previously read if the first transaction reads the same row, it encounters different data probably. This situation is known as the non-repeatable read anomaly. For example, assume that there are two transactions such as Transaction 1 and Transaction 2. In this example, while Transaction 1 reads any row, Transaction 2 updates or deletes that row and commits. In this step, if Transaction 1 re-reads the same row, it gets a different value from what has been previously read or cannot receive the data because the row has been deleted. 

| **Transaction 1** | **Transaction 2** |
| :---------------: | :---------------: |
|        \|         |        \|         |
|      read(x)      |        \|         |
|        \|         |        \|         |
|        \|         |     write(x)      |
|        \|         |        \|         |
|        \|         |      commit       |
|        \|         |        \|         |
|      read(x)      |        \|         |
|        \|         |        \|         |
|         v         |         v         |

### PHANTOM READ

The phantom read happens when a transaction executes the same query more than once and gets a different set of rows for each read. For example, imagine that a group of transactions runs simultaneously on any system. In this system, Transaction 1 executes a query from the database to retrieve a set of rows, which contains 10 records. However, Transaction 2 adds 15 rows and commits before Transaction 1 executes the query once more. In this situation, if Transaction 1 executes the query again to get data from the database, the set of rows returned may differ attributed to the query. In short, Transaction 1 gets a different result on each read. 

| **Transaction 1** | **Transaction 2** |
| :---------------: | :---------------: |
|        \|         |        \|         |
|    read(x>10)     |        \|         |
|        \|         |        \|         |
|        \|         |    write(x=15)    |
|        \|         |        \|         |
|        \|         |      commit       |
|        \|         |        \|         |
|    read(x>10)     |        \|         |
|        \|         |        \|         |
|         v         |         v         |


## TRANSACTION ISOLATION LEVELS

One transaction how isolated from the effects of other concurrent transactions and how interact with each other refers to transaction isolation level. There are four levels of transaction isolation:

### READ UNCOMMITTED

It is considered the lowest isolation level and allows all transactions to see uncommitted changes. A transaction reads data that has not been yet committed by another transaction, which means dirty read. This isolation level doesn't guarantee data consistency.

For example, suppose that there are two transactions named Transaction 1 and Transaction 2 like the example in the section of dirty read. In this example, before Transaction  1 updates a row but is not committed, transaction 2 reads the updated row. In this step, if Transaction 1 rolls back the change, read data by Transaction 2 is considered never to have existed. 

### READ COMMITTED

In this isolation level, a transaction can only read data that has been committed. To enforce this behavior, this isolation level uses locking mechanisms. For this mechanism, there are two locks named read-lock (shared lock) and write-lock (exclusive lock). 

{% admonition(type="info", title="Note") %}
It is important that what is the difference between shared lock and exclusive lock. Concurrency mechanism in a database management system is provided by the concept of these locks. Namely, shared locks and exclusive locks have a pivotal role in transaction isolation levels.
{% end %}

To prevent dirty reads, the isolation level uses locking. Locking mechanisms ensure that a transaction reads only committed data. With the help of the locking mechanism, a transaction can hold a read-lock (if it only reads the row) or write-lock (if it updates or deletes the row) on a specific row to prevent the effects of other transactions. 

When a transaction reads data, it acquires the read-lock (shared lock). This lock allows other transactions to acquire read-lock on the same data.  Namely, it enables concurrent reads. However, read-lock prevents other transactions from acquiring exclusive locks on the same data. It is important to note that shared locks otherwise known as read-lock are released immediately after the read operation. The lock is not held throughout the entire transaction. As soon as the read operation, the lock is released. 

On the other hand, when a transaction carries out operations such as inserting, updating, or deleting, it must acquire the write-lock (exclusive lock) in this isolation level. With the write-lock, the transactions prevent other transactions from acquiring read-lock or write-lock on the same data. Thus, it ensures data integrity during write operations.

Let's review the isolation level step by step with an example. Consider a database table accounts with the below data. 

| id   | balance |
| ---- | ------- |
| 1    | 1000    |
| 2    | 1500    |

In this example, T1 and T2 are one of the transactions that operate on this table:

1. Before T1 reads the balance column of account 1, it first acquires a shared lock on that row.
2. After T1 acquires a shared lock, it reads and then releases the shared lock.
3. Before T2 updates the balance column of account 1 to 1200, it acquires an exclusive lock on that row. 
4. After T2 commits, it releases the exclusive lock.
5. To T1 reads the balance column of account 1 again, it acquires the shared lock. After acquiring the shared lock, T1 reads the updated value of 1200 and then releases the shared lock.

It would be best for a retail application to display the available inventory of products. However, it is important to note that some inconsistencies between reads are acceptable in this situation. For example, when displaying available inventory to customers, some inaccuracies due to concurrent updates can be tolerated, provided that the data remains up-to-date.

### REPEATABLE READ

It means that a transaction retrieves the same result for re-reading multiple times in the same transaction, even if other transactions modify or delete that row. 

For instance, a transaction executes the SQL statement for a SELECT operation. During this process, it uses read-locks to lock the rows as the application fetches them. The locks prevent other transactions from modifying the data until the reading transaction is completed. However, the read-locks allow other transactions to read the same data concurrently. 

On the other hand, if the transaction executes the SQL statement for DELETE, it uses the write-locks to lock the rows as it deletes them. Thus, the current transaction prevents any non-repeatable reads, as other transactions cannot update, delete, and read these rows. 

For a financial app that calculates the total balance of a user's accounts, this isolation level would be the best option. It ensures that a transaction consistently retrieves the same result when read several times in the same transaction. Thus, in a financial app, the user's balance does not change during the calculation process due to the lock mechanism. 

### SERIALIZABLE

It is considered the highest level of isolation and can prevent dirty reads, non-repeatable reads, and phantom reads. In this isolation level, the transaction holds a read-lock or write-lock. 

For example, the transaction locks the table with read-locks and does not allow any new rows to be inserted into it when the transaction executes the SQL statement for SELECT. 

On the other hand, when the transaction executes the SQL statement for DELETE, the transaction holds write-locks and does not allow any rows to be inserted or updated. Additionally, the other transaction cannot read any rows. However, increasing the number of locking causes the performance problem.

Specifically, as it is the highest level of isolation, it is used to ensure strict consistency and integrity in the system in which each transaction is critical. During money transfer operations between accounts, this isolation level would be the best option. 

## CONCLUSION

In the following table, it is shown which transaction isolation level caused which anomalies. An "X" marks each phenomenon that can occur.

| **Transaction Isolation Level** | **Dirty Read** | **Nonrepeatable Read** | **Phantom Read** |
| ------------------------------- | -------------- | ---------------------- | ---------------- |
| Read Uncommitted                | X              | X                      | X                |
| Read Committed                  | --             | X                      | X                |
| Repeatable Read                 | --             | --                     | X                |
| Serializable                    | --             | --                     | --               |

Concurrency anomalies are prevented by using the correct isolation level. Particularly, higher isolation levels collectively prevent anomalies. However, they increase needing system resources. Therefore, it is essential to comprehend the trade-offs involved and choose an isolation level. Further, choosing the correct isolation level leads to providing data integrity, reliability, consistency, and efficiency in a database management system. 

## REFERENCES

[1] <a href="https://www.mongodb.com/basics/acid-transactions" target="_blank">https://www.mongodb.com/basics/acid-transactions</a>

[2] <a href="https://www.javatpoint.com/acid-properties-in-dbms" target="_blank">https://www.javatpoint.com/acid-properties-in-dbms</a>

[3] <a href="https://learn.microsoft.com/en-us/sql/odbc/reference/develop-app/transaction-isolation-levels?view=sql-server-ver16" target="_blank">https://learn.microsoft.com/en-us/sql/odbc/reference/develop-app/transaction-isolation-levels?view=sql-server-ver16</a>

[4] <a href="https://www.cockroachlabs.com/blog/sql-isolation-levels-explained/" target="_blank">https://www.cockroachlabs.com/blog/sql-isolation-levels-explained/</a>

[5] <a href="https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html" target="_blank">https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html</a>
