---
title: "DB (1) - Introduction to Database and Database Management Systems(DBMS)"
categories:
  - database
tags:
  - database
  - SQL
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
This is the first post of the [Database Series](https://kimdanny.github.io/categories/#database).

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

## 2. Three-level ANSI-SPARC Architecture
The American National Standards Institute Standards Planning and Requirements Committee (ANSI-SPARC) 
recognized the need for a three-level approach with a system catalog.
Despite the fact that this model is not a current standard of the current DBMS, it is still valuable to learn 
in that it gives a basis for understanding the core functionality of widely used DBMS.

![relational DB](/images/db/three-level-ansi-sparc.png)     

There are some important objectives of three-level architecture:
- All users should be able to access the same data
- A user's view is immune to changes made in other views
- Users should not need to know physical DB storage details
- Internal structure of DB should be unaffected by changes to physical aspects of storage
- DBA should be able to change conceptual structure of DB without affecting all users

**External Level** is users' view ot the database. 
This describes the part of database that is relevant to a particular user.  

**Conceptual Level** is community view of the database.
This describes what data is stored in database and relationships among the data.  

**Internal Level** is a physical representation of the database on the computer. 
This describes how the data is stored in the database.

## 3. Data Independence
Major goal of the three-level architecture is to provide data independence: upper levels are unaffected by 
changes to lower levels. Two types of data independence are _Logical Independence_ and _Physical Independence_.  

**Logical Independence**:
- Refers to immunity of external schemas to changes in conceptual schema.
- Conceptual schema changes (addition or removal of entities)
- DBMS should not require changes to external schema or rewrites of application programmes.

**Physical Independence**:
- Refers to immunity of conceptual schema to changes in the internal schema.
- Internal schema changes (using different file organisations, storage structures)
- DBMS should not require change to conceptual or external schemas.




From next posts, we will discover how we can interact with DBMS and eventually with DB. Yes, it's about SQL (particularly MySQL).  

[Link to the next post](https://kimdanny.github.io/database/S2-sql/).  

#### Reference
1. My note taken from Dr. John Dowell's lectures at UCL - COMP0009: Logic and Database Theory (19/20)
2. Definition of DB and DBMS: [_https://en.wikipedia.org/wiki/Database_](https://en.wikipedia.org/wiki/Database)
3. Definition of DB models: [_https://en.wikipedia.org/wiki/Database_model_](https://en.wikipedia.org/wiki/Database_model)
4. My notes taken from Dr. John Dowell's lectures at UCL - COMP0022: Database and Information Management (20/21)
5. Database Systems: A Practical Approach to Design, Implementation, and Management, 4th Edition by Thomas Connolly & Carolyn Begg