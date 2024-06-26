**LINQ and SQL**

**LINQ**

1. **Advantages using LINQ than stored procedure**

**Debugging** - It is really very hard to debug the Stored procedure but as LINQ is part of .NET, you can use visual studio's debugger to debug the queries.

**Deployment** - With stored procedures, we need to provide an additional script for stored procedures but with LINQ everything gets complied into single DLL hence deployment becomes easy.

**Type Safety** - LINQ is type safe, so queries errors are type checked at compile time. It is really good to encounter an error when compiling rather than runtime exception!

**Built-in security** - One reason I preferred stored procs before LINQ was that they forced the use of parameters, helping to reduce SQL injection attacks. LINQ to SQL already parameterizes input, which is just as secure.

**Reduction in work** - Before LINQ, I spent a lot of time building DALs, but now my DataContext is the DAL. I've used OPFs too, but now I have LINQ that ships with multiple providers in the box and many other 3rd party providers, giving me the benefits from my previous points.

1. **When should I used CompiledQuery**

static readonly Func&lt;Entities, IEnumerable<Tag&gt;> AllTags =

CompiledQuery.Compile&lt;Entities, IEnumerable<Tag&gt;>

(

e => e.Tags

);

IEnumerable&lt;Tag&gt; GetByName(string name)

{

using (var db = new Entities())

{

return AllTags(db).Where(t => t.Name.Contains(name)).ToList();

}

}

You should use a CompiledQuery when all of the following are true:

- The query will be executed more than once, varying only by parameter values.
- The query is complex enough that the cost of expression evaluation and view generation is "significant" (trial and error)
- You are not using a LINQ feature like IEnumerable&lt;T&gt;.Contains() which won't work with CompiledQuery.
- You have already simplified the query, which gives a bigger performance benefit, when possible.
- You do not intend to further compose the query results (e.g., restrict or project), which has the effect of "decompiling" it.

CompiledQuery does its work the first time a query is executed. It gives no benefit for the first execution. Like any performance tuning, generally avoid it until you're sure you're fixing an actual performance hotspot.

1. **Anonymous Types**

Anonymous types are types that are generated by compiler at run time. When we create a anonymous type we do not specify a name. We just write properties names and their values. Compiler at runtime create these properties and assign values to them.

var k = new { FirstProperty = "value1", SecondProperty = "value2" };

Console.WriteLine(k.FirstProperty);

Anonymous class is useful in LINQ queries to save our intermediate results.

There are some restrictions on Anonymous types as well:

- Anonymous types can not implement interfaces.
- Anonymous types can not specify any methods.
- We can not define static members.
- All defined properties must be initialized.
- We can only define public fields.

1. **Diference between deferred execution and Lazy evaluation in C#**

In practice, they mean essentially the same thing. However, it's preferable to use the term deferred.

- Lazy means "don't do the work until you absolutely have to."
- Deferred means "don't compute the result until the caller actually uses it."

When the caller decides to use the result of an evaluation (i.e. start iterating through an IEnumerable&lt;T&gt;), that is precisely the point at which the "work" needs to be done (such as issuing a query to the database).

The term _deferred_ is more specific/descriptive as to what's actually going on. When I say that I am lazy, it means that I avoid doing unnecessary work; it's ambiguous as to what that really implies. However, when I say that execution/evaluation is deferred, it essentially means that I am not giving you the real result at all, but rather a ticket you can use to claim the result. I defer actually going out and getting that result until you claim it.

1. **Difference Skip() and SkipWhile()**

**Skip()** **:** It will take an integer argument and from the given IEnumerable it skips the top n numbers

**SkipWhile ():** It will continue to skip the elements as far as the input condition is true. It will return all remaining elements if the condition is false.

1. **Expression trees**

An **Expression Tree** is a data structure that contains Expressions, which is basically code. So it is a tree structure that represents a calculation you may make in code. These pieces of code can then be executed by "running" the expression tree over a set of data.

In LINQ, expression trees are used to represent structured queries that target sources of data that implement IQueryable&lt;T&gt;. For example, the LINQ provider implements the IQueryable&lt;T&gt; interface for querying relational data stores. The C# compiler compiles queries that target such data sources into code that builds an expression tree at runtime. The query provider can then traverse the expression tree data structure and translate it into a query language appropriate for the data source.

**SQL**

1. **Joins**

This is a keyword used to query data from more tables based on the relationship between the fields of the tables. Keys play a major role when JOINs are used.

There are various types of join which can be used to retrieve data and it depends on the relationship between tables.

- **Inner Join.**

Inner join return rows when there is at least one match of rows between the tables.

- **Right Join.**

Right join return rows which are common between the tables and all rows of Right hand side table. Simply, it returns all the rows from the right hand side table even though there are no matches in the left hand side table.

- **Left Join.**

Left join return rows which are common between the tables and all rows of Left hand side table. Simply, it returns all the rows from Left hand side table even though there are no matches in the Right hand side table.

- **Full Join.**

Full join return rows when there are matching rows in any one of the tables. This means, it returns all the rows from the left hand side table and all the rows from the right hand side table.

1. **Normalization and denormalization**

Normalization is the process of minimizing redundancy and dependency by organizing fields and table of a database. The main aim of Normalization is to add, delete or modify field that can be made in a single table.

DeNormalization is a technique used to access the data from higher to lower normal forms of database. It is also process of introducing redundancy into a table by incorporating data from the related tables.

The normal forms can be divided into 5 forms, and they are explained below -.

- **First Normal Form (1NF):.**

This should remove all the duplicate columns from the table. Creation of tables for the related data and identification of unique columns.

- **Second Normal Form (2NF):.**

Meeting all requirements of the first normal form. Placing the subsets of data in separate tables and Creation of relationships between the tables using primary keys.

- **Third Normal Form (3NF):.**

This should meet all requirements of 2NF. Removing the columns which are not dependent on primary key constraints.

- **Fourth Normal Form (4NF):.**

Meeting all the requirements of third normal form and it should not have multi- valued dependencies.

1. **Function vs Stored Procedure**

Functions follow the computer-science definition in that they MUST return a value and cannot alter the data they receive as parameters (the arguments). Functions are not allowed to change anything, must have at least one parameter, and they must return a value. Stored procs do not have to have a parameter, can change database objects, and do not have to return a value.

SP can have input/output parameter

1. **Indexes**

An index is performance tuning method of allowing faster retrieval of records from the table. An index creates an entry for each value and it will be faster to retrieve data.

- **Unique Index.**

This indexing does not allow the field to have duplicate values if the column is unique indexed. Unique index can be applied automatically when primary key is defined.

- **Clustered Index.**

This type of index reorders the physical order of the table and search based on the key values. Each table can have only one clustered index.

- **NonClustered Index.**

NonClustered Index does not alter the physical order of the table and maintains logical order of data. Each table can have 999 nonclustered indexes.

1. **Constraint**

Constraint can be used to specify the limit on the data type of table. Constraint can be specified while creating or altering the table statement. Sample of constraint are.

- NOT NULL.
- CHECK.
- DEFAULT.
- UNIQUE.
- PRIMARY KEY.
- FOREIGN KEY.

1. Clause

SQL clause is defined to limit the result set by providing condition to the query. This usually filters some rows from the whole set of records.

Example – Query that has WHERE condition

Query that has HAVING condition.

1. Union, minus, interact

UNION operator is used to combine the results of two tables, and it eliminates duplicate rows from the tables.

MINUS operator is used to return rows from the first query but not from the second query. Matching records of first and second query and other rows from the first query will be displayed as a result set.

INTERSECT operator is used to return rows returned by both the queries.

1. **ACID**

**ACID** описывает требования к [транзакционной системе](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0"%20\o%20"Транзакционная%20система) (например, к [СУБД](https://ru.wikipedia.org/wiki/%D0%A1%D0%A3%D0%91%D0%94)), обеспечивающие наиболее надёжную и предсказуемую её работу.

**Atomicity –** Атомарность гарантирует, что никакая [транзакция](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D1%8F_%28%D0%B8%D0%BD%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0%29"%20\o%20"Транзакция%20%28информатика%29) не будет зафиксирована в системе частично. Будут либо выполнены все её подоперации, либо не выполнено ни одной.

**Consistency –** Транзакция, достигающая своего нормального завершения (EOT — end of transaction, завершение транзакции) и, тем самым, фиксирующая свои результаты, сохраняет согласованность базы данных. Другими словами, каждая успешная транзакция по определению фиксирует только допустимые результаты.

в банковской системе может существовать требование равенства суммы, списываемой с одного счёта, сумме, зачисляемой на другой

**Isoltation –** Во время выполнения транзакции параллельные транзакции не должны оказывать влияния на её результат. Изолированность — требование дорогое, поэтому в реальных БД существуют режимы, не полностью изолирующие транзакцию

**Durability -** Независимо от проблем на нижних уровнях (к примеру, обесточивание системы или сбои в оборудовании) изменения, сделанные успешно завершённой транзакцией, должны остаться сохранёнными после возвращения системы в работу. Другими словами, если пользователь получил подтверждение от системы, что транзакция выполнена, он может быть уверен, что сделанные им изменения не будут отменены из-за какого-либо сбоя.

1. **Having where difference**

The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.

HAVING: is used to check conditions after the aggregation takes place.  
WHERE: is used to check conditions before the aggregation takes place.

This code:

select City, CNT=Count(1)

From Address

Where State = 'MA'

Group By City

Gives you a table of all cities in MA and the number of addresses in each city.

This code:

select City, CNT=Count(1)

From Address

Where State = 'MA'

Group By City

Having Count(1)>5

Gives you a table of cities in MA with more than 5 addresses and the number of addresses in each city.

(An aggregate function performs a calculation on a set of values, and returns a single value. Except for COUNT(\*), aggregate functions ignore null values. Aggregate functions are often used with the GROUP BY clause of the SELECT statement.)

1. **GROUP BY**

The GROUP BY statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

The GROUP BY statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG) to group the result-set by one or more columns.

SELECT column_name(s)  
FROM table_name  
WHERE condition  
GROUP BY column_name(s)ORDER BY column_name(s);

EXAMPLE:

SELECT COUNT(CustomerID), Country  
FROM Customers  
GROUP BY Country;

Find duplicate values in sql tables:

SELECT

name, email, COUNT(\*)

FROM

users

GROUP BY

name, email

HAVING

COUNT(\*) > 1

1. **UNION vs UNION ALL**

The UNION command is used to select related information from two tables, which is like a JOIN command. However, when using UNION command, all the selected columns need to be of the same data type. With UNION, only distinct values are selected

UNION ALL command is equal to UNION command, except that UNION ALL selects all the values.

The difference between Union and Union all is that Union all will not eliminate duplicate rows, instead it just pulls all the rows from all the tables fitting your query specifics and combines them into a table.

_A UNION statement effectively does a SELECT DISTINCT on the results set. If you know that all the records returned are unique from your union, use UNION ALL instead, it gives faster results._

UNION removes duplicate records (where all columns in the results are the same), UNION ALL does not.

There is a performance hit when using UNION instead of UNION ALL, since the database server must do additional work to remove the duplicate rows, but usually you do not want the duplicates (especially when developing reports).