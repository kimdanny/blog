---
title: "Introduction to Database and Database Management Systems(DBMS)"
categories:
  - database
tags:
  - database
  - SQL
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## 1. How can we manage our data?
### 1-1. File-based System
File-based system is a contrasting concept to the database system.
It is a collection of application programs that perform services for the end users.
Each program defines and manages its own data. Think of a customer-client relationship.
Say we have to query data of customers' orders. In this case, customer Invoice application program retrieve data of invoices.
Similarly, querying data of mailing list of customers will invoke running of customer mailing list application program, which will give you the whole mailing list.  

There are critical disadvantages/limitations of managing data with the file-based approach.
One, it is inevitable to separate and isolate all the data you have, for each program contains its own data.
Therefore, users of one program are not able to aware of potentially useful data held by other application programs.  

Another limitation that can be easily come up with is that programs are written to satisfy particular functions which means
any new requirement needs a new set of programs. Other than these two limitations, many more presents, such as
'Duplication of data' and 'Incompatible file formats' to name just a few.  

Then how can we solve this problem? This is where Database System comes into play which was a ground breaking idea in the field of data management.

### 1-2. Database (DB) and Database Management Systems (DBMS)
"**Database** is an organized collection of data, generally stored and accessed electronically from a computer system".  
"**Database Management System** is a software that interacts with end users, applications, and the database itself to capture and analyze the data."  
"**Database model** is a type of data model that determines the logical structure of a database and fundamentally determines in which manner data can be stored, organized and manipulated."  
(Borrowed words from Wikipedia).  

One of the famous DB models is the **Relational Database Model**. In relational DB model,
data comprises entities, attributes and relationships of an information.

Actually, what is more important is DBMS, because it is what enables users to interact with DB, and it is what provides controlled access to the DB.
DBMS supports 
1. _Data Language (SQL)_ 
2. _Security system_ 
3. _Data integrity system_ 
4. _Concurrency control system_ 
5. _Recovery control system_ and 
6. _User-accessible catalog_  

(What I found particularly interesting was its concurrency control system. We are making a software that should be embedded in a large corporations, so many users i.e. employees have to access the data at the same time.
It would be no good if users queue up for accessing the DB every time).  

Hence, by employing DBMS, you have a full control of data redundancy, consistency, sharing, integrity, security.
Moreover, at economical point of view, you can achieve an 'Economy of scale', which leads to an increase in productivity.  

However, its complexity, which stems from its huge size and other additional systems, and higher impact of a failure can be its major drawbacks.  


From next posts, we will discover how we can interact with DBMS and eventually with DB. Yes, it's about SQL (particularly MySQL).  

[Link to the next post](https://kimdanny.github.io/database/mysql-1/).  

##### Reference
1. My note taken from Dr. John Dowell's lectures at UCL - COMP0009: Logic and Database Theory (19/20)
2. Definition of DB and DBMS: [_https://en.wikipedia.org/wiki/Database_](https://en.wikipedia.org/wiki/Database)
3. Definition of DB models: [_https://en.wikipedia.org/wiki/Database_model_](https://en.wikipedia.org/wiki/Database_model)