<!--
author:   Peter Camacho, Clinical Reporting Unit, Children's Hospital of Philadelphia
email:    camachop@chop.edu
version:  0.0.0
language: en
narrator: US English Female
comment:  SQL (Structured Query Language) is a domain-specific language used in programming and designed for managing data held in a Relational Database Management System (RDBMS), or for stream processing in a relational data stream management system (RDSMS).
link:     https://storage.googleapis.com/chop-dbhi-arcus-education-website-assets/css/custom.css
logo:     https://github.com/arcus/education_fellows/raw/main/img/chop-icon.png
-->


# SQL : Structured Query Language

Overview
========

<div class = "hint">
This Training Module will provide an overview of SQL (<b>S</b>tructured <b>Q</b>uery <b>L</b>anguage).
</div>

Contents
========

* [What is SQL?](#what-is-sql)

  * [When Should SQL be Used?](#when-should-sql-be-used)
  * [When Should SQL Not be Used?](#when-should-sql-not-be-used)
  * [Are all Implementations of SQL The Same?](#when-should-sql-be-used)

* [Basic SQL Syntax](#Basic-SQL-Syntax)

  * [Select Statement](#Select-Statement)
  * [Distinct Clause](#Distinct-Clause)
  * [Where Clause](#Where-Clause)
  * [Dealing with Null Values](#Dealing=with-Null-Values)
  * [Order By Statement](#Order-By-Statement)
  * [Limit Clause](#Limit-Clause)
  * [Adding Comments](#Adding-Comments)
  * [Aliasing](#Aliasing)

* [Advanced SQL Syntax](#Advanced-SQL-Syntax)

  * [Case Statement](#Case-Statement)
  * [Like Operator](#Like-Operator)
  * [Regular Expression Functions](#Regular-Expression-Functions)
  * [Aggregate Functions](#Aggregate-Functions)
  * [Group By Statement](#Group-By-Statement)
  * [Having Clause](#Having-Clause)
  * [Sub Queries](#Sub-Queries)
  * [With Statement](#With-Statement)
  * [Exists Statment](#Exists-Statment)

* [SQL Joins](#SQL-Joins)

  * [Inner Join](#Inner-Join)
  * [Left Join](#Left-Join)
  * [Cartesian Joins - When Joining Goes Wrong](#cartesian-joins---when-joining-goes-wrong)


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

**SQL** should be used any time you need to access data stored within a **Relational Database**.

More specifically, **SQL** is best suited for composing/structuring/formatting datasets for export and downstream analysis in programs like R or Python.

> **Historical Note**:
> 
> **SQL** was created in the early 1970's by IBM as a method for more easily accessing information from their internal database system. 
> 
> By 1979 Relational Software, Inc. (now Oracle Corporation) released the first commercially available implementation of **SQL** as a part of their Oracle V2 database application.
> 
> Today **SQL** is the most common programing language for extracting and organizing data in Relational Database Systems.

Ideally, all of your Data Transformations should be done using **SQL** in order to ensure that the final dataset exported from your queries don't require any additional major transformations before the analysis phase of your work can begin. 

The reason for this is that, especially for large datasets, **SQL** is a much more efficient tool for large-scale data transformations than your traditional scripting or analytic packages.

Additionally, making sure that all of your data transformations are done using **SQL** is an easy way to ensure greater reproducibility and standardization of your work (just be sure to save/document all of your **SQL** queries!).

### When Should SQL Not be Used?

![](img/misusing-tools.jpeg)

Although **SQL** is a great tool for organizing your data into meaningful reports for data extraction, it is not a great tool to use for your actual analysis work.

Despite having many functions for text parsing, **SQL** is not the tool/language you want to use for any NLP (Natural Language Processing) work.

**SQL** also doesn’t have any capabilities to directly support Data Visualization work.

For all of these "downstream analytics" use cases, you will want to use an actual analytical programing language or tool like **R** or **Python**.

### Are all Implementations of SQL The Same?

Although all **SQL** implementations have a similar structure, and the same basic syntax, each different **SQL** database product often has its own minor variations in dialect.

Colloquially people often refer to the different **SQL** dialects as different "flavors" of **SQL**.

Some Popular "Flavors" of **SQL**:

* [**MySQL**](https://www.mysql.com/) (open source)
* [**SQLite**](https://www.sqlite.org) (open source)
* [**PostgreSQL**](https://www.postgresql.org/) (open source)
* [**Oracle**](https://www.oracle.com/database/technologies/appdev/sql.html) (proprietary)
* [**BigQuery**](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) (proprietary)

The most common difference between different **SQL** "flavors" are the availability of different functions that users can use for data manipulation, as well as the types of error messages that will be returned to the user when running code with syntax issues.

That said, knowing the specific "flavor" of **SQL** your database uses is especially useful when first getting started writing queries and troubleshooting errors.

> **Important Note**:
>
>Throughout the remainder of this documentation we will be using the [**BigQuery**](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) **SQL** syntax to write our code (this is the "Flavor" of **SQL** used in Arcus Labs here at CHOP).

## Basic SQL Syntax

At a high level, you’re going to have to provide three pieces of information when constructing **SQL** "**queries**":

 1. The name of the **Table(s)** where the data is stored.
 2. The **Column(s)** from the **Table(s)** you want to look at.
 3. The **filtering condition(s)** you want to apply to your data pull. 

You put these basic pieces of information together using the syntax shown below to create a **SQL** query: 

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

  * [Distinct Clause](#Distinct-Clause)

* [Where Clause](#Where-Clause)

  * [Dealing with Null Values](#Dealing=with-Null-Values)

* [Order By Statement](#Order-By-Statement)
* [Limit Clause](#Limit-Clause)
* [Adding Comments](#Adding-Comments)
* [Aliasing](#Aliasing)

### Select Statement

The **Select Statement** is used to specify which columns you would like to have returned as output from your **SQL** query.

The basic components of a **Select Statement** are the `SELECT` and `FROM` keywords. 

> The `FROM` keyword is used to specify the table you would like to use as the base of your query, and the `SELECT` keyword is used to provide a list of columns (from the table(s) referenced in your query) that you would like returned as output.

**Select All Columns**

If you would like to return ALL of the columns from the table(s) specified in your **SQL** query, you can use the `*` wild card character as shown in the example below:

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
> This format is known as "**Dot Notation**".

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
> This is another example of the use of "**Dot Notation**" in **SQL**.
> 
> Though not required for a single table select statement, it is a good idea to follow this practice any time you are writing a select statement in order to make sure its clear which table each column is coming from. Doing this will make things less error-prone if you ever want to add additional tables to your query and will make it easier for other programmers to read your code.

### Distinct Clause

The `distinct`clause in **SQL** can be placed directly after the `select` key word, and can be used to limit your result set down to only the unique row values. 

> This can be especially useful when exploring a dataset for the first time and trying to become familiar with the data in each column of a given table. 

The code block below provides an example of using this syntax to invesitgate the unique combinations of values from the `sex` and `ethnicity` columns from the `patient` table; as you can see the `distinct` clause will work on any number of columns:

```sql
select distinct
  patient.sex
  ,patient.ethnicity
from arcus.patient

```

> **Pro Tip**: The `distinct` clause is especially useful for removing duplicates rows from the result set of your `SQL` queries; in the event that duplicate rows would cause errors during analysis.

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
> To read more about the basic types of "Operators" avaiable for use in a **SQL** query, click [here](https://www.tutorialspoint.com/sql/sql-operators.htm) for some helpful documentation from **tutorialspoint.com**.

### Dealing with Null Values

Like many programing languages, **SQL** deals with "blank" values in a very specific way.

**SQL** uses the concept of `null` to represent "blank" row values.

If you ever find yourself in a situation where you need to filter on `null` values you can use the `is null` or `is not null` opperators as shown below:

```sql
select *
from arcus.allergy
where
    allergy_delete_reason_name is null --exclude any rows where the "allergy_delete_reason_name" column has data (i.e. exclude "deleted" allergies).

```

Its also worth noting that `null` values are treated very differently from actual data. To illustrate this point, we can look at the below example:

> The `where` clause of this code block is attempting to include all rows other than those where the **allergy\_delete\_reason\_name** is equal to **'Allergy Outgrown'**, however we can see that doing this also excludes any rows where the **allergy\_delete\_reason\_name** value is `null`.

```sql
select distinct
  allergy_delete_reason_name
from arcus.allergy
where
    allergy_delete_reason_name <>'Allergy Outgrown'

```
In order to make sure that records where the **allergy\_delete\_reason\_name** is `null` are also included in our output we will need to add another line to the select statement to explicitly include them, as shown below:

```sql
select distinct
  allergy_delete_reason_name
from arcus.allergy
where
    (
        allergy_delete_reason_name <>'Allergy Outgrown'
        or allergy_delete_reason_name is null
    )

```

> **IMPORTANT NOTE**: This is a very subtle distinction that can drastically alter the output of your SQL statements, especially when writing "exclusion" logic like in the example above, so its a good idea to make sure you have a firm grasp on this before moving forward.

### Order By Statement

Another useful peace of **SQL** syntax for exporing datasets is the `order by` statement, which (as its name suggests) is used to order your result set by a given set of columns.

When listing columns in the `order by` statment you can specify that they be sorted in either ascending (`asc`) or descending (`desc`) order. 

```sql
select distinct
  patient.sex
  ,patient.ethnicity
from arcus.patient
order by
  patient.sex asc
  ,patient.ethnicity desc

```

> **Note:** By default, all items in the `order by` clause will be sorted in `asc` order if no explicit ordering argument/type is provided.

### Limit Clause

The `limit` clause can be used to limit the result set of your select statement to (at most) a pre-defined number of rows.

> To do this all you need to do is add the word `limit` as the last line of your query, followed by the number of rows you would like your result set truncated at. 

The example below pulls all columns from the encounter table, and limits the result set to only 10 rows.

```sql
select * 
from arcus.encounter
limit 10

```

> **Tip**: As you can see, this is also a great peace of syntax to use for exporing tables you might be unfarmiliar with (in the event you want to see the kind of data a table/query contain but you don't want to wait for all rows of the query to return; which can take a long time for larger tables or more complex queries).

### Adding Comments

"**Comments**" are essentially explanatory or helpful bits of text that programmers can add to their code as documentation for themselves or other reviewers of their code, and that don't actually effect the execution of the SQL code in any way.

In **SQL** there are 2 different techniques that can be used for adding comments, **single-line** and **multi-line** comments

> **Single-Line** Comments can be created by typing 2 minus signs in a row (i.e. `--`). 
> 
> Once added to your code, anything that appears to the right of the `--` comment delimiter will be treated as comment text.
> 
> **Multi-Line** Comments can be started by adding the `/*` characters to your code, and the multi-line comment can be closed by adding the `*/` characters. 
> 
> Once created, any text that appears between the `/*` and `*/` "tags" will be treated as comment text.

The code block below provides an example of each of these styles of commenting:

```sql
/* This is a simple demographics query*/
select
  patient.pat_id      --unique patient identifier.
  ,patient.sex        --patient sex {Male, Female, Unknown, null}
  ,patient.race       --patient race
  ,patient.ethnicity  --patient ethnicity {Hispanic or Latino, Not Hispanic or Latino, Refused, Unknown}
  ,patient.state_abbr --Two Character State Abbreviation.
from arcus.patient


/*
    Aren't Comments Great!
*/
```

### Aliasing

In SQL, it is possible to assigne a custom (or "short hand") name to a table or column in your query using a tequnique called **Aliasing**.

> **Aliasing** *Tables* can be helpful for a **SQL** programmer so that they don't have to type out the full name of a table each time they want to make reference to it. 

> **Aliasing** *Columns* can be helpful to consumers of your data by assigning clearer, more "comprehensible", names for a given column than the name that might be assgined to it in the database.

**Aliases** are assigned by placing the `as` key word directly after the item (table/column) you would like to alias, followed by the name you would like to assign as its **alias**.

In the example below, we can see **Aliasing** being used to rename the `patient` table to `p`, and renaming the `pat_id` and `state_abbr` columns to `unique_patient_id` and `state_shortname`.

```sql
select
  p.pat_id as unique_patient_id
  ,p.sex
  ,p.race
  ,p.ethnicity
  ,p.state_abbr as state_shortname
from arcus.patient as p

```

> **Note**: We will see aliasing again in a few other contexts later on in this documentation, however I wanted to be sure to make you aware of these 2 most basic/common cases of aliasing before moving fowards.

## Advanced SQL Syntax

The sections that follow will expand upon the concepts we learned about in the previous section on [Basic SQL Syntax](#Basic-SQL-Syntax) and will introduce us to serveral more advance fuctions and concepts.

**SECTION CONTENTS**

* [Case Statement](#Case-Statement)
* [Like Operator](#Like-Operator)
* [Regular Expression Functions](#Regular-Expression-Functions)
* [Aggregate Functions](#Aggregate-Functions)

  * [Group By Statement](#Group-By-Statement)
  * [Having Clause](#Having-Clause)

* [Sub Queries](#Sub-Queries)

  * [With Statement](#With-Statement)
  * [Exists Statment](#Exists-Statment)

### Case Statement

The `case` statement is used to produce conditional row-level output based on columns/rows provided as input.

> Often when creating datasets you will come across the need to define your own "custom categories/groupings" given some raw row data as input. This is where the `case` statement can come in handy!

The `case` statement has 4 main components (shown below). 

```sql
CASE                --COMPONENT 1: start tag of the case statement.
  WHEN (…) THEN (…) --COMPONENT 2: conditional when "some input" then "some output" logic.
  …
  ELSE (…)          --COMPONENT 3: declariation of default value to be returned if when/then conditions are not met.
END                 --COMPONENT 4: end tag of case statment.
```

>Its important to note that the `case`, `else`, and `end` components can only be listed once (and the `case` and `end` components must always be listed).
>
>However, you can list as many occurrences of the `when/then` component as you would like. 
>
>When multiple `when/then` components are listed, SQL will walk through each of them in the order they are listed; and will return output for the first `when` condition to be evaluated as TRUE.
>
> Finally, if no `else` clause is explicitly specified SQL imposes a condition of `else null` by default (but you'll see I included it in the example query below anyway for the sake of illustration).

The example below uses a `case` statement to create a column called **birth\_weight\_category**, which "buckets" patients birth weights into 1 of 3 categories ('Below Average', 'Average','Above Average').

```sql
select
  patient.pat_id
  ,patient.birth_weight_kg
  ,case
    when (patient.birth_weight_kg > 4.5) then 'Above Average' 
    when (patient.birth_weight_kg < 2.5) then 'Below Average'
    when (patient.birth_weight_kg between 2.5 and 4.5) then 'Average'
    else null
   end as birth_weight_category --assumes the average birth weight is between 2.5 kg and 4.5 kg
from arcus.patient

```

### Like Operator

The `like` operator can be used to filter on row values that contain a specific "pattern of text" in a column of interest (also known as "text/pattern matching").

For the purpose of "pattern matching", the `like` operator is able to utilize the 2 distinct **Wildcard Charaters** listed below:

|Wildcard Characters|Description|
|---|---|
|`%`|"Wildcard" for 0 or more characters.|
|`_`|"Wildcard" for exactly 1 characters.|

The code block shown below uses the `like` opperator, in the `where` clause, to filter on records from the `allergy` table where the `allergen_name` is starts with the text "stra".

```sql
select distinct allergy.allergen_name
from arcus.allergy
where
    upper(allergy.allergen_name) like upper('stra%')

```
> **WARNING**: 
> 
> The `like` operator (and almost everything else in **SQL**) is **CASE SENSITIVE**!
> 
> This means an upper and lower case version of the same letter will be treated differently (i.e. `'a'<>'A'`). 
> 
> For this reason I recommend that you ALWAYS use either the `lower()` or `upper()` functions, as shown above, when dealing with text/string based data in your sql queries.

### Regular Expression Functions

**Regular Expression Functions** are a class of function that utilize "[Regular Expression](https://en.wikipedia.org/wiki/Regular_expression)" "[Metacharacters](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended)" to perform some kind of pattern matching on text data.

Similar to the [`like`](#Like-Operator) opperator's "wildcard" characters, **Regular Expression "[Metacharacters](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended)"** are used in **Regular Expression Functions** to allow for more dynamic forms of text based pattern matching.

The most common set of **Regular Expression "[Metacharacters](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended)"** are listed below:

|Metacharacter|Description|
|:---|:---|
|^|Matches the starting position within the string.|
|\$|Matches the ending position within the string.|
|.|Matches any single character (similar to the `_` wildcard in a `like` statement).|
|*|Matches 0 or more occurrences of the preceding character.|
|\||This character (known as the "choice operator") can be used to delimit multiple match patterns, and will provide a match on either the expression before or the expression after it is listed in your search string.|

> For a full list of **Regular Expression "Metacharacters "**, follow this [link](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended).

The example below uses the **BigQuery SQL** `regexp_contains()` function to filter on records where the **allergen\_name** either *starts* with "stra" or *ends* with "egg".

> **WARNING**: In BigQuery **SQL** regular expression functions are "**Case Sensitive**".
>
> This means an upper and lower case version of the same letter will be treated differently (i.e. `'a'<>'A'`). 
> 
> For this reason I recommend that you ALWAYS use either the `lower()` or `upper()` functions, as shown below, when dealing with text/string based data in your sql queries.

```sql
select distinct allergy.allergen_name
from arcus.allergy
where
    regexp_contains(
        lower(allergy.allergen_name)
        ,lower('^stra|egg$')
    )
order by
    allergy.allergen_name
    
```
As you can see from even just this simple example, regular expression functions can be much more useful & dynamic than the [`like`](#Like-Operator) operator for filtering on complex text based data.

> **Learning More about "Regular Expression Functions"**: 
> 
> For detailed documentation on all of the [BigQuery Regexp Functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#regexp_contains) that can be used, follow this [link](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#regexp_contains).


### Aggregate Functions

**Aggregate Functions** can be used to summarize the values for multiple rows of data in some meaningful way. 

When used by themselfs, **Aggregate Functions** will return a single value given multiple rows of input.

See the table below for a list of the most commonly used  **Aggregate Functions**:

|Function|Description|
|:---|:---|
|`count()`|Returns a Count of the number of non-null values among the column(s)/rows provided as input.|
|`sum()`|Returns the summation of all values from a column provided as input.|
|`min()`|Returns the minimum value from a column provided as input.|
|`max()`|Returns the maximum value from a column provided as input.|
|`avg()`|Returns the average of all values from a column provided as input.|

The below table utilizes each of these **Aggregate Functions** to analyse the `birth_weight_kg` column from the `patient` table:

```sql
select
    count(patient.birth_weight_kg) as pat_weight_count
    ,sum(patient.birth_weight_kg) as sum_pat_weight_kg
    ,min(patient.birth_weight_kg) as min_pat_weight_kg
    ,max(patient.birth_weight_kg) as max_pat_weight_kg
    ,avg(patient.birth_weight_kg) as avg_pat_weight_kg
from arcus.patient

```

#### Group By Statement

The `GROUP BY` statement is used to group column results into only the unique/distinct values among them, and is used in combination with [**AGGREGATE FUNCTIONS**](#Aggregate-Functions) to generate summary statistics about the larger dataset that was "grouped" (i.e. "collapsed") by the `GROUP BY` statement. 

The code block below shows an example of using the `GROUP BY` statement to summarize some simple information from the **patient** table.

```sql
select
    patient.sex
    ,count(birth_weight_kg) as pat_count
    ,min(birth_weight_kg) as min_weight
    ,max(birth_weight_kg) as max_weight
    ,avg(birth_weight_kg) as avg_weight
from arcus.patient
group by
    patient.sex

```

##### Having Clause

The `having` clause can be used to filter your result set on the value of an **Aggregate Function** (which is something you will get an error on if you try to do in the `where` clause).

> In terms of placement in your structure, the `having` clause can be placed directly after your `group by` statement, and before your `order by` statement (if listed).

The example below uses the `having` clause to filter on only those patients from the **encounter** table that have more than 5 records (i.e. encounters) listed, and then returns a list of the encounter counts for each of these patients (sorted in descending order by "encounter count").

```sql
select
  encounter.pat_id
  ,count(distinct encounter_id) as encounter_count
from arcus.encounter
group by
  encounter.pat_id
having
  count(*)>5
order by 
  encounter_count desc

```

> **Pro Tip**: 
> 
> The `having` clause is also a great tool to use for determining which columns in your tables are potential "Primary Keys" (and which are not); "primary keys" are columns that have a unique value for each row of data.
> 
> e.g. The query below shows that the **pat\_id** column from the patient table contains a unique value for each row:
> 
> `select pat_id, count(*) from arcus.patient group by pat_id having count(*)>1`

### Sub Queries

A **Sub Query** is essentially a "nested" **SQL** query that is referenced inside of a larger **SQL** query.

> **Sub Queries** can appear in the `from` section of your `select` statement like a regular table and are demarcated by open and close parentheses, followed by an alias name that you would like to use to reference it later on in your query.

The example below creates a very simple subquery called **strawberry\_allergies**, which contains all records from the **allergy** table relating to patients with  "strawberry" allergies. 

It then references this table to calculate the "noted age in years" for each patient.

```sql
select
    strawberry_allergies.pat_id
    ,round(strawberry_allergies.noted_age/365.25, 2) as noted_age_years
from (
    select allergy.*
    from arcus.allergy
    where
        upper(allergy.allergen_name) like upper('strawberry%')
) as strawberry_allergies

```

#### With Statement

The **WITH** statement can be used to create a sort of "detached Sub Query" (or "Temporary Table") that will be created before your primary **SELECT** statement runs. 

The code block below provides and example of using the `with` statement to create a "temp table" that is then refrenced in the `from` clause of the main `select` statement:

```sql
with
    strawberry_allergies as (
        select allergy.*
        from arcus.allergy
        where
            upper(allergy.allergen_name) like upper('strawberry%')
    )
select distinct
    round(noted_age/365.25, 2) noted_age_years
from strawberry_allergies
order by
  noted_age_years

```

> **Pro Tip**: This approach is often use to increase code readability, but can also be used to increase query performance in certain situations.

#### Exists Statment

The `exists` statement can be used to filter your query results on data contained (or not contained) within a separate **sub query**.

The example below uses the `exist` clause to filter the patient table on only those patients that have a documented "strawberry" allergy.

```sql
select *
from arcus.patient
where
    exists( --filter on only patients that have a "strawberry" allergy.
        select 1
        from arcus.allergy
        where
            patient.pat_id = allergy.pat_id --tell the "exists()" statement to evaluate based on pat_id and values shared between the "allergy" and "patient" tables.
            and upper(allergy.allergen_name) like upper('strawberry%') --limit on only "strawberry" allergy records.
    )

```

> **Note**: As we will see after reading the section on **SQL Joins**, the exists clause is similar to an "`INNER JOIN`".

## SQL Joins

**What are SQL Joins**

Most queries require something more complex than referencing data from a single table. This is where **SQL**’s "**Join**" functionality comes into play.

"**SQL Joins**" are used to combine **Rows** from 2 (or more) **Tables**, based on some set of **Columns** they have in common.

There are two basic peaces of information you need to know to write successful **joins**:

 1. What "**Criteria**" would you like your **join** evaluated against?
 2. What "**Type**" of **join** do you want to use?

**JOIN CRITERIA**

**Join Criteria** are "conditions" that you would like evaluated as the basis for your **SQL Join**. 

> When the "conditions" in your **Join Criteria** evaluate as TRUE for a row then a join will be performed for those rows, and when the **Join Criteria** are evaluated as FALSE no join for those rows will take place.

In the simplest case, your **Join Criteria** will be an equality statement referencing the shared columns (between your tables) that you would like evaluated when resolving your join. 

The "shared columns" used in your **Join Criteria** are also some times called **Join Keys**. There are 2 different categories of **Join Keys**, these are known as **Primary Keys** and **Foreign Keys** respectively.

- A **Primary Key** is a column (or set of columns) that contain a unique value for each row in your Table.

- A **Foreign Key** is a column (or set of columns) in your table that make reference to a **Primary Key** in some other Table (or set of Tables) in your Database.

**JOIN TYPES**

There are 4 basic **Join Types** that can be used. Each of these **Join Types** are listed below, and have their own unique behavior:

- `left join`
- `right join`
- `inner join`  
- `outer join`  

"**Venn-diagrams**" are often helpful tools for discussing (and visualizing) the unique behaviors of each different **Join Type**. An image of your typical **Venn-diagram** is displayed below.

![](img/venn-diagram.png)

> When depicting **SQL Joins** in a "**Venn-diagram**" like this you can imagine that the circle on the left hand side represents all data from your "base table" (i.e. the table in your query that is referenced first), and the circle on the right represents all data in your "join table" (i.e. the table you would like to join too).
> 
> Using this model, each of the segments of the **ven-diagram** can be thought of as representing a different type of join. 

The image below uses a different **ven-diagram**  to provide a visual representation of each different **Join Type** (which are referred to as  "**inner**", "**left**", "**right**", "**full**" joins respectively).

![](img/join-types.jpg)

That said, check out the next few sections of this documentation for a detailed explanation of the `inner` and `left` **join types**.

> **TLDR on ignoring the `right` and `full` joins**: 
> 
> The `full` join type is a bit convoluted and only useful in a very narrow set of situations, so I wont be going into any more detail on it in this text.
> 
> Additionally, I don't recommend using the `right` join type in any situation so you will notice it has also been omitted from the rest of this documentation. 
> 
> The reason for this is that anything you want to do with a right join can be done by re-ordering your query and using a `left` join. This means that the `right` join is at best a bit redundant, and at most is sort of confusing for anyone reading your queries.

**SECTION CONTENTS**


* [Inner Join](#Inner-Join)
* [Left Join](#Left-Join)

  * [Cartesian Joins - When Joining Goes Wrong](#cartesian-joins---when-joining-goes-wrong)

### Inner Join

The `inner` join type tells your query to only return records where your join criteria evaluate as TRUE (i.e. it it will only return rows that have shared "join keys" between the 2 tables that your are attempting to join).

This concept is represented in the 2 diagrams below: 

![](img/inner-join-venn-diagram.png)

This first diagram (shown above) uses the **Venn-Diagram** analogy to show that the only rows that will be returned are those that have shared values for your join keys, which is represented by the inner most section of the **ven-diagram** (i.e. the section where data between your 2 tables "over laps").


![](img/inner-join-table-example.png)

This second diagram (shown above) uses a table based representation of this same `inner join` behavior, where the only rows that we end up with are rows that have shared "join keys" between the 2 tables we are attempting to join, and provides a greate illistration of what the columns in our final result set will look like (where the "matching" rows from table 1 and table 2 are appended together).

To provide you with a more concrete example of a query that uses an `inner join` check out the **SQL** below, which can be used to pull all encounter level diagnosis information in our "**Arcus Lab**". This query uses an `inner join` to connect the **encounter\_diagnosis** and **master\_diagnosis** tables on their shared **dx\_id** columns.

```sql
select 
  --------------------------
  --encounter_diagnosis info
  --------------------------
  encounter_diagnosis.pat_id
  ,encounter_diagnosis.encounter_id
  ,encounter_diagnosis.dx_type
  ,encounter_diagnosis.line
  --------------------------
  --master_diagnosis info 
  --------------------------
  ,master_diagnosis.dx_name
  ,master_diagnosis.icd10_list
  ,master_diagnosis.icd9_list
from arcus.encounter_diagnosis
inner join arcus.master_diagnosis
  on encounter_diagnosis.dx_id = master_diagnosis.dx_id
order by
  encounter_diagnosis.encounter_id
  ,encounter_diagnosis.line

```

> **Note**: The **dx\_id** column in the **encounter\_diagnosis** table is a **foreign key**, and the **dx\_id** column in the **master\_diagnosis** table is a "**primary key**".

### Left Join

The `left` join type tells your query to return ALL records from your "base table" and any rows from your "join table" where your join criteria evaluate as TRUE (i.e. it won't filter out any rows from your first table, even when the table your trying to join to doesn't have any "matching" rows based on your **join criteria**).

This concept is represented in the 2 diagrams below: 

![](img/left-join-venn-diagram.png)

This first diagram (shown above) uses the **Venn-Diagram** analogy to show that ALL rows from your "base table" will be returned as well as any rows from your "join table" that have shared join key values (and any rows from your "join table" that don't have shared join key values will be ommitted). This is represented by the left most and inner most sections of the **ven-diagram** (i.e. the sections where data from your "base table" are represented).

![](img/left-join-table-example.png)

This second diagram (shown above) uses a table based representation of this same `left join` behavior, where we end up will all rows from our original "base table" and any rows from our "join table" that have shared "join keys". This visual also provides a greate illistration of what the columns in our final result set will look like (where the "matching" rows from table "a" and table "b" are appended together, and any rows where our "join table" does not have a mapping join key are assigned all `null` values).

To provide you with a more concrete example of a query that uses a `left join`, check out the **SQL** below which can be used to pull all **Patient** level demographic information and patient **Allergy** information from our "**Arcus Lab**" into a single select statement (which also won't ommit any patients who dont have allergys). This query uses a `left join` to connect the **patient** and **allergy** tables on their shared **pat\_id** columns.


```sql
select
  allergy.allergy_id
  ,patient.pat_id
  ,patient.sex
  ,patient.ethnicity
  ,patient.race
  ,allergy.allergen_name
  ,allergy.allergen_type_name
  ,allergy.reaction_name
  ,round(noted_age/365.25, 2) as noted_age_years
  ,allergy.allergy_status_name
  ,allergy.allergy_delete_reason_name
from arcus.patient
left join arcus.allergy
    on patient.pat_id = allergy.pat_id
-- where
--   allergy.allergy_id is null

```
> **Note**: The **pat\_id** column in the **patient** table is a "**primary key**", and the **pat\_id** column in the **allergy** ctable is a **foreign key**.

### Cartesian Joins - When Joining Goes Wrong

A “**Cartesian**” Join is essentially a join that “results in more rows than either of the individual tables it started with”. This happens when niether of the colums referenced in your **Join Criteria** are a "**Primary Key**".

 
**"PRIMARY KEY" JOINS**

"**Primary Key**" joins can be considered “Stable” because the result set of these joins will have (at most) the same number of rows as the original “**foreign key**” table in the join, as shown in the diagram below: 

![](img/stable-join-product.png)

**“CARTESIAN” JOINS**

**Cartesian** Joins (i.e. a join where neither of the columns in the **Join Criteria** are a "**Primary Keys**") are considered "Unstable" because the result set of these joins will increase exponentially the larger your tables are. This effect is depicted in the diagram below:

![](img/cartesian-join-product.png)

“Cartesian” Joins can be very "memory intensive" operations as the size of the result set increases exponentially with the size of their 2 input tables. Additionally, if they go “unnoticed”, they can seriously effect the validity of your SQL reports (e.g. if your trying to infer a count of something by counting the number of rows, an unintentional cartesian join will result in you overcounting the value you were trying to measure).

That said, make sure you understand the relationship between each of the columns you are using in your **Join Criteria** to make sure that you aren't accidentally writing a **Cartesian** Join.
