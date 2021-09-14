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
    * [Are all Implementations of SQL The Same?](#Are-all-Implementations-of-SQL-The-Same)
* [Basic SQL Syntax](#Basic-SQL-Syntax)
    * [Select Statement](#Select-Statement)
    * [Case Statment](#Case-Statement)
    * [Where Clause](#Where-Clause)
    * [Aggregate Functions](#Aggregate-Functions)
    * [Group By Statement](#Group-By-Statement)    
    * [Having Clause](#Having-Clause)
* [Advanced SQL Syntax](#Advanced-SQL-Syntax) 
    * [Wildcard Operators](#Wildcard-Operators)
        * [Like Statment](#Like-Statment)
        * [Regular Expression Functions](#Regular-Expression-Functions)
    * [Sub Queries](#Sub-Queries)
    * [With Statement](#With-Statement)
    * [Exists Statment](#Exists-Statment)
* [SQL Joins](#SQL-Joins)
    * [Inner Join](#Inner-Join)
    * [Left Join](#Left-Join)
    * [Cross Join](#Cross-Join)
* [Advanced SQL Syntax II (Bonus Round!)](#Advanced-SQL-Syntax-II-Bonus-Round)
    * [Ordered Analytic Functions](#Ordered-Analytic-Functions)
* [DDL - Data Definition Language](#DDL---Data-Definition-Language)
    * [Create Table](#Create-Table)
    * [Create View](#Create-View)
    * [Dropping Tables/Views](#dropping-tablesviews)
    * [Altering Tables/Views](#altering-tablesviews)
    * [Renaming Tables](#Renaming-Tables)
    * [Adding Columns](#Adding-Columns)
    * [Recasting Columns](#Recasting-Columns)
* [DML - Data Manipulation Language](#dml---data-manipulation-language)
    * [Update Statment](#Update-Statment)
    * [Insert Statment](#Insert-Statment)
    * [Delete Statment](#Delete-Statment)


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

Ideally, all of your Data Transformations should be done using SQL in order to ensure that the final dataset exported from your queries doesn't require any additional major transformation before analysis work can begin. 

The reason for this is that, especially for large datasets, **SQL** is a much more efficient tool for large-scale data transformations than your traditional scripting or analytic packages.

Additionally, making sure that all of your data transformations are done using **SQL** is an easy way to ensure greater reproducibility and standardization of your work (just be sure to save/document all of your **SQL** queries!).

### When Should SQL Not be Used?

![](img/misusing-tools.jpeg)

Although **SQL** is a great tool for organizing your data into meaningful reports for data extraction, it is not a great tool to do for your actual analysis work.

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

That said, knowing the specific "flavor" of SQL your database uses is especially useful when first getting started writting queries and troubleshooting errors.

> **Important Note**:
>
>Throughout the remainder of this documenation we will be using the [**BigQuery**](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) SQL syntax to write our code (this is the "Flavor" of SQL used in Arcus Labs here at CHOP).

## Basic SQL Syntax

At a high level, you’re going to have to provide three peaces of information when constructing **SQL** "**queries**":

 1. The name of the **Table(s)** where the data is stored.
 2. The **Column(s)** from the **Table(s)** you want to look at.
 3. The **filtering condition(s)** you want to applied to your data pull. 

> **Vocabulary Note**:
> 
> **Queries** are essentially questions or requests for data, written in a specific structure that your Relational Database can understand.

The sections that follow will provide an overview of the basic syntax and structure used for composing **SQL** "**queries**" using these 3 peaces of information.

<hr><hr>

**SECTION CONTENTS**

* [Select Statement](#Select-Statement)
* [Case Statment](#Case-Statement)
* [Where Clause](#Where-Clause)
* [Aggregate Functions](#Aggregate-Functions)
* [Group By](#Group-By)
* [Having Clause](#Having-Clause)

### Select Statement

**Select All Columns**

```sql
select *
from arcus.patient
limit 10

```

**Select Specific Columns**

```sql
select
  patient.pat_id
  ,patient.sex
  ,patient.race
  ,patient.ethnicity
  ,patient.state_abbr
from arcus.patient


```

**Select Distinct Values**

```sql
select distinct
  patient.sex
  ,patient.ethnicity
from arcus.patient

```

**Ordering Results of Select Statement**

```sql
select distinct
  patient.sex
  ,patient.ethnicity
from arcus.patient
order by
  patient.sex ASC
  ,patient.ethnicity DESC

```

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

### Where Clause


**Comparison Operators**

```sql
select * 
from arcus.encounter
where
  appt_age = 2

```

```sql
select * 
from arcus.encounter
where
  appt_age <= 2

```

```sql
select * 
from arcus.encounter
where
  appt_age >= 2

```

```sql
select * 
from arcus.encounter
where
  appt_age <> 2

```

**Logical Operators**


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

```sql
select * 
from arcus.encounter
where
  pat_encounter_num = 1
  and appt_age in (
    0
    ,1
    ,2
  )
  
```

```sql
select * 
from arcus.encounter
where
  pat_encounter_num = 1
  appt_age between 0 and 2
  
```

### Group By Statement
#### Aggregate Functions
#### Having Clause

## Advanced SQL Syntax
#### Wildcard Operators
#### Like Statment
#### Regular Expression Functions
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

