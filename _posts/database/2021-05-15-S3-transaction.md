---
title: "DB (3) - Transaction Management"
categories:
  - database
tags:
  - database
  - SQL
  - transaction
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
This post is part of the [Database Series](https://kimdanny.github.io/categories/#database).  
[Click here to read from the previous post](https://kimdanny.github.io/database/S2-sql/).  

## 1. About Transaction
    "Transaction is an action, or series of actions, carried out by user or application,
    which reads or updates contents of database."
    - Database Systems 4th edition

Transaction is a logical unit of work on the DB.
Consider the transaction example case below:
```
read(staffNo=x, salary);
new_salary = salary * 1.1;
write(staffNo=x, new_salary)
```
Within the DB, we can have one of two outcomes:
1. **Success** - transaction _commits_ and database reaches a new consistent state.
2. **Failure** - transaction _aborts_, and database must be restored to consistent state before it started (_rolled back_ / undone).

Here, committed transaction cannot be aborted, and aborted transaction that is rolled back can be restarted later.

Therefore, we have four basic properties of transactions, ACID:
**A**tomicity: All or nothing property.

**C**onsistency: DBMS must transform DB from one consistent state to another consistent state.

**I**solation: Incomplete transactions should not be visible to other transactions.

**D**urability: Effects of a committed transaction are permanent.


## 2. Concurrency Control
Concurrency control is a process of managing simultaneous operations on the database
without having them interfere with on another. 
It prevents interference when two or more users are accessing DB simultaneously and at least on is updating data.
Although two transactions may be correct in themselves, interleaving of operations may produce an incorrect result.

Due to concurrency issue following problems can occur:
- _Lost update_: successfully completed update is overridden by another user.
- _Uncommitted dependency_: one transaction can see intermediate results of another transaction before it has committed.
- _Inconsistent analysis_: transaction reads several values but second transaction updates some of them during execution of the first.

### 2-1. Serialisability
As the name suggests, serialising runs transactions serially, but this limits degree of concurrency or parallelism in system.
However, serialisability identifies those executions of transations guaranteed to ensure consistency.
Serial schedule is a scheduling where operations of each transaction are executed consecutively without any interleaved operations from other transactions, 
while non-serial schedule is a schedule where operations from set of concurrent transactions are interleaved.

**The goal of serialisability is** to find `non-serial schedules` that allow transactions to execute concurrently without interfering with one another; 
we want to _find non-serial schedules that are equivalent to some serial schedule_. This is called a `serialisable schedule`.

### 2-2. Locking
Transaction uses locks to deny access to other transactions and so prevent incorrect updates.
This is used to ensure serialisability. 
A transaction must claim a shared or exclusive lock on a data item before read or write; shared lock when read, and exclusive lock when write.
Reads cannot conflict, so more than one transaction can hold shared locks on same item at the same time.

Same as other concurrency problems, deadlock can also occur in transactions. 
This can be handled by timeouts or other deadlock detection and recovery techniques.


For more, search with following keywords:  
two-phase locking, timestamping, database recovery, log file, checkpointing, deferred update, immediate update, shadow paging,
nested transaction model, sagas, multi-level transaction model, dynamic restructuring, workflow models, 


#### Reference
1. My notes taken from Dr. John Dowell's lectures at UCL - COMP0022: Database and Information Management (20/21)
2. Database Systems: A Practical Approach to Design, Implementation, and Management, 4th Edition by Thomas Connolly & Carolyn Begg