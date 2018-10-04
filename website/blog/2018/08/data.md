# Data

## T-SQL Pop Quiz

Try your hand at some more *Transact SQL* interview questions.

### *1. Adding a day to DateTime and Date Data Types*

Suppose you have 2 variables: a **DateTime** and a **Date** variable. You try to add 1 day to both variables.

```sql
DECLARE @TestDateTimeValue datetime = GETDATE()
DECLARE @TestDateValue date = GETDATE()

SELECT @TestDateTimeValue + 1  -- Using DateTime
SELECT @TestDateValue + 1  -- Using Date
```

What are the results?

1. Both SELECT statements generate an error message.
2. The first statement adds a **minute** to *@TestDateTimeValue*, and the second adds a **day** to *@TestDateValue*.
3. Both statements add a **day** to the respective variables.
4. The first statement adds a **day** to *@TestDateTimeValue*, and the second generates an error.

### *2. Subquery Syntax and Results*

Suppose you create a basic Vendor Master table and a related Order table, with orders for the vendors. The data stores two vendors in the master table and two orders in the order table (one for each vendor). Here's the DDL and insert statements.

```sql
CREATE TABLE dbo.VendorMaster
(
    VendorPK int,
    VendorName varchar(100)
)

CREATE TABLE dbo.Order
(
    OrderID int,
    VendorFK int,
    OrderDate date,
    OrderAmount decimal(10,2)
)

INSERT INTO dbo.VendorMaster VALUES
    (1, 'Vendor A'),
    (2, 'Vendor B')

INSERT INTO dbo.Order VALUES
    (1, 1, '2008-01-01', 100),
    (2, 2, '2009-01-01', 150)
```

Note that the VendorMaster table has **VendorPK** and the Order table has **VendorFK**.

You want to write a query that returns the vendors that have at least one order in the year 2008. How many rows will SQL Server return for the following query?

```sql
SELECT *
FROM dbo.VendorMaster
WHERE VendorPK IN (
    SELECT VendorPK
    FROM dbo.Order
    WHERE YEAR(OrderDate) = 2008)

```

1. One Row
2. Two Rows
3. Zero Rows
4. The query generates an error message.

### *3. SCOPE_IDENTITY() and @@IDENTITY*

Suppose you have a table that has an identity column that SQL Server generates. You insert a row into the table and want to capture and store the identity generated for the new row, regardless of any other process. You use both the **SCOPE_IDENTITY()** function and the **@@IDENTITY** system global variable.

```sql
CREATE TABLE dbo.TestTable
(
    IdentityKey int identity,
    TestValue varchar(100)
)

INSERT INTO dbo.TestTable VALUES ('Test 1')

SELECT SCOPE_IDENTITY()
SELECT @@IDENTITY
```

Both SELECT Statements return a value of 1

Can you describe a situation where one statement returns the desired value of 1 and the other returns something other than 1?

### *4. The BETWEEN Statement*

You create a test table of LastName values. You insert nine rows and then create an index on the LastName column.

```sql
CREATE TABLE dbo.NameTest
(
    LastName varchar(50)
)

INSERT INTO dbo.NameTest VALUES
    ( 'Anderson' ),
    ( 'Bailey' ),
    ( 'Barry' ),
    ( 'Benton' ),
    ( 'Boyle' ),
    ( 'Canton' ),
    ( 'Celkins' ),
    ( 'Cotton' ),
    ( 'Davidson' )

CREATE INDEX LastName ON dbo.NameTest (LastName)
```

You execute the following query:

```sql
SELECT *
FROM dbo.NameTest
WHERE LastName BETWEEN 'B' AND 'C'
```

How many rows does this query return?

1. Four Rows
2. Six Rows
3. Seven Rows
4. Zero Rows
5. The query generates an error message.

## *Answers?*

I will discuss these topics at the next SQL Tech Session and post my answers in next month's newsletter. If you have any questions (or answers with explanations) before then, feel free to pop me a message.