# Data

## *2NF - Second Normal Form*

A table in 2NF is narrowed to a single purpose. This clarifies the database design, makes it easier to describe and use a table, and tends to eliminate modification anomalies.

A table is in 2NF if:

* The table is in 1st normal form, and
* All the non-key columns are dependent on the table's primary key

What does this mean?

The primary key provides a way to uniquely identify each row in a table. What we mean by this, is that if we wanted to find a particular value such as an employee's start date, we would need to know what the primary key (EmployeeId) is, to look up the answer.

Once we have identified the table's purpose, we look at each column in the table and ask, "Does this column serve to describe what the primary key identifies?"
* If the answer is "Yes", then the column is dependent on the primary key and belongs in the table.
* If the answer is "No", then the column should be moved to a different table.

When all the columns relate to the primary key, they naturally share a common purpose, such as describing an employee. So as we can see, by having a table in 2NF, it has a single purpose, such as storing employee information.

## Issues with our Data Model

The first issue is that the SalesStaffInformation table has 2 columns which are not dependent on the EmployeeId. Though they describe which office the SalesPerson is based out of, the SalesOffice and OfficeNumber columns don't serve to describe who the employee is.

The second issue is that there are several columns that don't completely rely on the entire Customer table primary key. For the given customer, it doesn't make sense that you should know both the CustomerId and EmployeeId to find the customer.

For a customer, you only need to know the CustomerId. Therefore, the Customer table is not in 2NF because there are columns that do not depend on the entire primary key. By definition, they should then be moved to another table. But is that the correct thing to do?

## Fix the model...

SalesStaffInformation

    EmployeeId (PK)    
    SalesPerson
    SalesOffice
    OfficeNumber
    
<br/><br/>
Customer

    CustomerId (PK)
    EmployeeId (PK)(FK)
    CustomerName
    CustomerCity
    CustomerPostalCode

In the SalesStaffInformation table, neither the SalesOffice nor OfficeNumber columns are completely dependent on the primary key. So here we create a SalesOffice table and add a foreign key to the SalesStaffInformation table to identify in which office the sales person is based.

SalesStaffInformation

    EmployeeId (PK)    
    SalesPerson
    SalesOfficeId(FK)


SalesOffice

    SalesOfficeId (PK)        
    SalesOffice
    OfficeNumber

In the Customer table, none of the columns are dependent on the entire primary key (CustomerId, EmployeeId). This makes things a little trickier. Rather than moving the fields to a new table, we need to realise that the problem lies in the primary key. None of the columns depend on the EmployeeId field.

This hints at the fact that this table is trying to serve 2 purposes.
* To indicate which customers are dealt with by each employee
* To identify customers and their locations

By removing the EmployeeId, the table's purpose becomes clear, to identify and describe each customer.

Customer

    CustomerId (PK)
    CustomerName
    CustomerCity
    CustomerPostalCode

Now we create a table to describe which customers are dealt with by which employees. We'll name this table SalesStaffCustomer and add the foreign key columns CustomerId and EmployeeId. Both these columns also form the primary key on this table and are the only columns required in this table.

SalesStaffCustomer

    CustomerId (PK)(FK)
    EmployeeId (PK)(FK)

Let's look at the tables with data:

Customer

|CustomerId (PK)|Name|City|PostalCode|
|---------------|----|----|----------|
| 1000 | Ford | Dearborn | 48123 |
| 1010 | GM | Detroit | 48213 |
| 1020 | Dell | Austin | 78720 |
| 1030 | HP | Palo Alto | 94303 |
| 1040 | Apple | Cupertino | 95014 |
| 1050 | Boeing | Chicago | 60601 |

SalesStaffInformation

| EmployeeId (PK) | SalesPerson | SalesOfficeId |
|-----------------|-------------|---------------|
| 1003 | Mary Smith | 10 |
| 1004 | John Hunt | 20 |
| 1005 | Martin Hap | 10 |

SalesOffice

| SalesOfficeId (PK) | SalesOffice | OfficeNumber |
|--------------------|-------------|--------------|
| 10 | Chicago | 312-555-1212 |
| 20 | New York | 212-555-1212 |

Notice that most of the redundancy in the data has been eliminated, there are no update, insert or delete anomalies. If we deleted all sales people, we would still be able to retain all our customer records. Also deleting a SalesOffice, does not require us to delete the records containing Sales people.

SalesStaffCustomer

| CustomerId (PK) | EmployeeId (PK) |
|-----------------|-----------------|
| 1000 | 1003 |
| 1010 | 1003 |
| 1020 | 1004 |
| 1030 | 1004 |
| 1040 | 1004 |
| 1050 | 1005 |

The SalesStaffCustomer table is strange in that it consists of only keys. It's called an __intersection__ or __linking__ table. __Linking__ tables are useful when you need to model many-to-many relationships.
This table requires no other columns and creating a composite primary key using both columns makes sense. 

*However, what happens in the case where you need to use the relationship between Customer and Employee to, for example, enforce a restriction of interaction within the database. To clarify, what happens if only the employee(s) who service(s) a specific customer may place orders on their behalf?*

*Even in this scenario, there is no need for any additional columns in our linking table as long as we setup the foreign key relationship correctly. On the Order table, the relationship between the EmployeeId and CustomerId is not linked to the id columns of the SalesStaffInformation and Customer tables, but to the linking table, SalesStaffCustomer. If done correctly, the database ensures that the composite primary key is used as the foreign key and your data integrity is maintained.*

For all practical purposes, this is a pretty workable database. Three out of our four tables are even in 3NF, but there is one table with a minor issue... Have you noticed?