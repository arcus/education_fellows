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

</div>

Contents
========

* [What is SQL?](#What-is-SQL)
    * [When Should SQL Be Used?](#When-Should-SQL-Be-Used)
    * [When Should SQL Not be Used?](#When-Should-SQL-Not-be-Used)
* [Basic SQL Syntax](#Basic-SQL-Syntax)
    * [Select Statement](#Select-Statement)
    * [Case Statment](#Case-Statement)
    * [Where Clause](#Where-Clause)
        * [Comparison Operators](#Comparison-Operators)
        * [Logical Operators](#Logical-Operators)
    * [Group By](#Group-By)
        * [Aggregate Functions](#Aggregate-Functions)
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

## Basic SQL Syntax

The sections that follow will provide an overview of the basic syntax and structure used for composing SQL "**queries**".

> **Vocabulary Note**:
> 
> **Queires** are essentially questions or requests for data, written in a specific strucutre that your Relational Database can understand.

**SECTION CONTENTS**

* [Select Statement](#Select-Statement)
* [Case Statment](#Case-Statement)
* [Where Clause](#Where-Clause)
* [Comparison Operators](#Comparison-Operators)
* [Logical Operators](#Logical-Operators)
* [Group By](#Group-By)
* [Aggregate Functions](#Aggregate-Functions)
* [Having Clause](#Having-Clause)

### Select Statement
#### Select All Columns
#### Select Specific Columns
#### Select Distinct Values
### Case Statement
### Where Clause
#### Comparison Operators
#### Logical Operators
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

