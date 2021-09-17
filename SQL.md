<!--
author:   Peter Camacho, Clinical Reporting Unit, Children's Hospital of Philadelphia
email:    camachop@chop.edu
version:  0.0.0
language: en
narrator: US English Female
comment:  SQL (Structured Query Language) is a domain-specific language used in programming and designed for managing data held in a Relational Database Management System (RDBMS), or for stream processing in a relational data stream management system (RDSMS).
link:     https://chop-dbhi-arcus-education-website-assets.s3.amazonaws.com/css/custom.css
logo:     https://github.com/arcus/education_fellows/raw/main/img/chop-icon.png
-->


# SQL : Structured Query Language

## Overview

<div class = "hint">
This Training Module will provide an overview of SQL (<b>S</b>tructured <b>Q</b>uery <b>L</b>anguage).
</div>

Contents
========

* [What is SQL?](#What-is-SQL)

  * [When Should SQL Be Used?](#When-Should-SQL-Be-Used)
  * [When Should SQL Not be Used?](#When-Should-SQL-Not-be-Used)



|field1|field2|
|---|---
|a|b|

## What is SQL?

To put it simply, SQL (**S**tructured **Q**uery **L**anguage) is the programing language used to interact with “**Relational Databases**”.

> **Vocabulary Note**:
> 
> A **Relational Database** is a type of database that stores data in objects called Tables. Tables themselves are objects comprised of Columns and Rows (similar to data in an Excel file). 
>
> ![](img/example-table.png)
>
> Tables within the database are related to one another by shared columns, sometimes referred to as "join keys" (more on join keys later!).
>
> The primary benefit of the **Relational Databases** model is the ability to use these "join keys" to create complex reports combining information from multiple tables to derive meaningful information from your data (this is done using SQL!)

You can think of **SQL** as the super-secret code that you can use to “ask explicit questions” about the information in your Relational Database.

### When Should SQL Be Used?

SQL should be used any time you need to access data stored within a **Relational Database**.

More specifically, **SQL** is best suited for composing/structuring/formatting datasets for export and downstream analysis in programs like R or Python.

> **Historical Note**:
> 
> SQL was created in the early 1970's by IBM as a method for more easily accessing information from their internal database system. 
> 
> By 1979 Relational Software, Inc. (now Oracle Corporation) released the first commercially available implementation of **SQL** as a part of their Oracle V2 database application.
> 
> Today **SQL** is the most common programing language for extracting and organizing data in Relational Database Systems.

Ideally, all of your Data Transformations should be done using SQL in order to ensure that the final dataset exported from your queries don't require any additional major transformations before the analysis phase of your work can begin. 

The reason for this is that, especially for large datasets, **SQL** is a much more efficient tool for large-scale data transformations than your traditional scripting or analytic packages.

Additionally, making sure that all of your data transformations are done using **SQL** is an easy way to ensure greater reproducibility and standardization of your work (just be sure to save/document all of your **SQL** queries!).

### When Should SQL Not be Used?

![](img/misusing-tools.jpeg)

Although **SQL** is a great tool for organizing your data into meaningful reports for data extraction, it is not a great tool to use for your actual analysis work.

Despite having many functions for text parsing, **SQL** is not the tool/language you want to use for any NLP (Natural Language Processing) work.

**SQL** also doesn’t have any capabilities to directly support Data Visualization work.

For all of these "downstream analytics" use cases, you will want to use an actual analytical programing language or tool like **R** or **Python**.

### Are all Implementations of SQL The Same?

Although all SQL implementations have a similar structure, and the same basic syntax, each different SQL database product often has its own minor variations in dialect.

Colloquially people often refer to the different SQL dialects as different "flavors" of SQL.

Some Popular "Flavors" of SQL:

* [**MySQL**](https://www.mysql.com/) (open source)
* [**SQLite**](https://www.sqlite.org) (open source)
* [**PostgreSQL**](https://www.postgresql.org/) (open source)
* [**Oracle**](https://www.oracle.com/database/technologies/appdev/sql.html) (proprietary)
* [**BigQuery**](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) (proprietary)

The most common difference between different SQL "flavors" are the availability of different functions that users can use for data manipulation, as well as the types of error messages that will be returned to the user when running code with syntax issues.

That said, knowing the specific "flavor" of SQL your database uses is especially useful when first getting started writing queries and troubleshooting errors.

> **Important Note**:
>
>Throughout the remainder of this documentation we will be using the [**BigQuery**](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) SQL syntax to write our code (this is the "Flavor" of SQL used in Arcus Labs here at CHOP).

## Basic SQL Syntax

At a high level, you’re going to have to provide three pieces of information when constructing **SQL** "**queries**":

 1. The name of the **Table(s)** where the data is stored.
 2. The **Column(s)** from the **Table(s)** you want to look at.
 3. The **filtering condition(s)** you want to apply to your data pull. 

You put these basic pieces of information together using the syntax shown below to create a SQL query: 

```sql
SELECT _2_ FROM _1_ WHERE _3_

```

> **Vocabulary Note**:
> 
> **Queries** are essentially questions or requests for data, written in a specific structure that your Relational Database can understand.

The sections that follow will provide an overview of this basic **SQL** syntax and go into detail on additional modifications to this basic syntax that can be used to format the output from your queries.

<hr><hr>

**SECTION CONTENTS**

* [Select Statement](#Select-Statement)
* [Where Clause](#Where-Clause)
* [Aggregate Functions](#Aggregate-Functions)
* [Group By](#Group-By)
* [Having Clause](#Having-Clause)

### Select Statement

The **Select Statement** is used to specify which columns you would like to have returned as output from your **SQL** query.

The basic components of a **Select Statement** are the `SELECT` and `FROM` keywords. 

The `FROM` keyword is used to specify the table you would like to use as the base of your query, and the `SELECT` keyword is used to provide a list of columns (from the table(s) referenced in your query) that you would like returned as output.

**Select All Columns**

If you would like to return ALL of the columns from the table(s) specified in your SQL query, you can use the `*` wild card character as shown in the example below:

```sql
select *
from arcus.patient

```

> **Additinal Info**:
> 
> Notice that the `FROM` line of this query is followed by 2 words separated by a period. 
> 
> The first word is the name of the Schema/Catalog that your data is stored in, and the second word is the name of the specific data Table you would like to reference as the base of your query.
> 
> This format is known as Dot Notation.

**Select Specific Columns**

If you would only like to return a specific set of columns in your select statement you will need to explicitly list out each of those columns after the select keyword, with each separate column reference separated by a comma:

```sql
select
  patient.pat_id
  ,patient.sex
  ,patient.race
  ,patient.ethnicity
  ,patient.state_abbr
from arcus.patient

```

> **Additional Info:**
> 
> Notice that each column listed in the **Select Statement** first lists the name of the table that the column belongs to, then the name of the column itself (separated by a period). 
> 
> This is another example of the use of Dot Notation in SQL.
> 
> Though not required for a single table select statement, it is a good idea to follow this practice any time you are writing a select statement in order to make sure its clear which table each column is coming from. Doing this will make things less error-prone if you ever want to add additional tables to your query and will make it easier for other programmers to read your code.

### Where Clause

The **Where Clause** is the section of your query used to specify any "filtering logic" that should be applied to your query before returning any output. 

The below example uses the **Where Clause** to filter output on only those records that represent the 1st encounter for each patient.

```sql
select * 
from arcus.encounter
where
  encounter.pat_encounter_num = 1
  
```

Although the above example lists only one constraint for the dataset, the **where clause** can contain any number of filtering arguments needed. 

Check out the code block below for an example of a where clause that includes multiple constraints, and makes use of both **Comparison** and **Logical** Operators. 

```sql
select * 
from arcus.encounter
where
  pat_encounter_num = 1
  and (
    appt_age = 0
    or appt_age = 1
    or appt_age = 2
  )
  
```

> **Additional Reading**:
> 
> To read more about the basic types of "Operators" avaiable for use in a SQL query, click [here](https://www.tutorialspoint.com/sql/sql-operators.htm) for some helpful documentation from **tutorialspoint.com**.

### Aggregate Functions
#### Group By Statement
#### Having Clause





## Advanced SQL Syntax

### Case Statement

```sql
select
  patient.pat_id
  ,patient.state_abbr
  ,case
    when patient.state_abbr = 'PA' then 1
    when patient.state_abbr <> 'PA' then 0
    when patient.state_abbr is null then -1
    else 0
   end is_pa_resident
from arcus.patient

```

### Distinct Clause

### Order By Statement

```sql
select
  patient.pat_id
  ,patient.state_abbr
  ,case
    when patient.state_abbr = 'PA' then 1
    when patient.state_abbr <> 'PA' then 0
    when patient.state_abbr is null then -1
    else 0
   end is_pa_resident
from arcus.patient
order by
  ,case
    when patient.state_abbr = 'PA' then 1
    when patient.state_abbr <> 'PA' then 0
    when patient.state_abbr is null then -1
    else 0
   end DESC
  ,patient.state_abbr ASC
  ,patient.pat_id

```

### Limit Clause
### Like Statment
### Regular Expression Functions
### Sub Queries
### With Statement
### Exists Statment

## SQL Joins
### Inner Join
### Left Join
### Cross Join

## Advanced SQL Syntax II (Bonus Round!)
### Ordered Analytic Functions

## DDL - Data Definition Language
> **NOTE**: Up until now all of the code we have looked at have been examples of **DQL** (Data Query Language).

### Create Table
### Create View
### Dropping Tables/Views
### Altering Tables/Views
#### Renaming Tables
#### Adding Columns
#### Recasting Columns

## DML - Data Manipulation Language
### Update Statment
### Insert Statment
### Delete Statment

