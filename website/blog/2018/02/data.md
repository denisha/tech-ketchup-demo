# Data Pop Quiz

As the first article within the data architecture world, I thought I'd see how well you would do with some *Transact SQL* interview questions.

Let's dive in...

### *1. Scalar Variables*

Suppose we have a SQL Server table with the following DDL (data definition language):

```sql
CREATE TABLE dbo.ControlTable
    (ControlId int identity
    CompanyName varchar(100))
GO
```

Let's say that we are expecting that this table contains only one record, but because of an update / insert error, we have 2.

If we wanted to populate a scalar variable with the company name. You write either of these simple queries:

#### Example A

```sql
DECLARE @CompanyName varchar(100)

SELECT @CompanyName = CompanyName FROM ControlTable

SELECT @CompanyName
```

#### Example B

```sql
DECLARE @CompanyName varchar(100)

SET @CompanyName = (SELECT CompanyName FROM ControlTable)

SELECT @CompanyName
```

What are the results?

1. Both Examples **A** and **B** generate an error because both examples are trying to read multiple rows into a scalar variable
2. **A** generates an error, while **B** returns one of the 2 names.
3. **A** returns one of the 2 names, while **B** generates an error.
4. SQL Server converts the variables in both examples to table variables and returns both names.

### *2. Inserting multiples rows using the VALUES clause*

Using the ControlTable above, how would you pre-populate the table with multiple companyNames using a single INSERT statement?


### *3. Comma-Separated Values from Multiple Rows*

Given the following data set:

Purchaser|Amount|Reason
---|---|---
Kevin|50|SQL Book
Kevin|60|Dinner
Steve|55|.Net Book
Steve|45|Music CDs
Steve|35|Lunch

You want to build the following result:

Purchaser|Total|Purchases
---|---|---
Kevin|110|Sql Book, Dinner
Steve|135|.Net Book, Music CDs, Lunch

How do you accomplish this?

1. Use the T-SQL FOR XML PATH('') function.
2. Use the T-SQL STRINGCONCAT function.
3. Use the SQL Server CLR and a .NET function.
4. T-SQL cannot accomplish this. You'd need to use a reporting tool.



## The Answer Sheet

### *1. Scalar Variables*

The answer is **3**. Although a scalar variable can only hold one value, a select statement (as in the example A) will populate the variable with the last value from the rows that the query scans. The **SET** statement (Example B) forces the use of a subquery and SQL Server will generate an error when reading more than one row. Replacing the *SET* with *SELECT* will result in the same error. Example A is far riskier, as the result is not guaranteed and a lack of an error masks the problem.

### *2. Inserting multiples rows using the VALUES clause*

SQL Server allows multiple values in the *VALUES* clause:

```sql
INSERT INTO ControlTable VALUES
    (1, 'Vendor A'),
    (2, 'Vendor B'),
    (3, 'Vendor C'),
    (4, 'Vendor D'),
    (5, 'Vendor E')
```

This can also be used in a *SELECT* statement

```sql
SELECT * FROM (VALUES
    (1, 'Vendor A'),
    (2, 'Vendor B'),
    (3, 'Vendor C'),
    (4, 'Vendor D'),
    (5, 'Vendor E')) tbl(Id, Name)
```

### *3. Comma-Separated Values from Multiple Rows*

The answer is **1**. You can combine **FOR XML** with the **STUFF** function:

```sql
SELECT Purchaser, SUM(PurchaseAmount) AS Total,
    (STUFF ((SELECT ', ' + PurchaseReason FROM TestPurchases inside
            WHERE inside.Purchaser = outside.Purchaser
            FOR XML PATH ('')), 1, 1, ''))
    AS Purchases
FROM TestPurchases outside
GROUP BY Purchaser
```

If you'd like to discuss any of these, feel free to contact me.