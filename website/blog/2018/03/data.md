# Data

## *Db Normalisation*

## Introduction

Database normalisation is the process used to organise a database into tables and columns. The idea is that a table stores information related to a *specific* topic. Only columns supporting that topic should be included in the table. 

By limiting a table to one purpose, you reduce the number of duplicate data contained in the database, which helps to eliminate some issues when trying to modify the data.

There are rules for desiging tables called *normal forms*. The first three normal forms are most commonly used within db design.

## Reasons for Normalisation

There are 3 main reasons to normalise a database.

1. Minimise duplicate data
2. Minimise or avoid data modification issues
3. Simplify queries

Let's look at some data which is not normalised and identify some potential issues.

Consider the following table:

<table>
  <tr>
    <th>EmployeeId (PK)</th>
    <th>SalesPerson</th>
    <th>SalesOffice</th>
    <th>OfficeNumber</th>
    <th>Customer1</th>
    <th>Customer2</th>
    <th>Customer3</th>
  </tr>
  <tr>
    <td>1003</td>
    <td>Mary Smith</td>
    <td>Chicago</td>
    <td>312-555-1212</td>
    <td>Ford</td>
    <td>GM</td>
    <td></td>
  </tr>
  <tr>
    <td>1004</td>
    <td>John Hunt</td>
    <td>New York</td>
    <td>212-555-1212</td>
    <td>Dell</td>
    <td>HP</td>
    <td>Apple</td>
  </tr>
  <tr>
    <td>1005</td>
    <td>Martin Hap</td>
    <td>Chicago</td>
    <td>312-555-1212</td>
    <td>Boeing</td>
    <td></td>
    <td></td>
  </tr>
</table>

This table serves multiple purposes:
1. Identifying the organisation's salespeople
2. Listing the sales offices and phone numbers
3. Associating a salesperson to a sales office
4. Showing the customers of each salesperson

When looking at this, a red flag should be raised. This table definitely does not serve one purpose and because of this, it introfuces a number of issues with data duplication, data update issues, and an increase in the effort to query the data.

## Data Duplication and Modification Anomalies

Each salesperson has both the SalesOffice and OfficeNumber listed and is duplicated across each salesperson. This duplication causes 2 problems.
1. It increases storage and decreases performance as the table is wider
2. It becomes difficult to maintain the data when making changes

For example:
* If the Chicago office moved, we would have to update all records for salespersons in Chicago. Although this is a simple table, these updates can become messy.
* If John Hunt quits, what happens to the New York office's information?

These are modification anomalies and there are 3 that can occur.

### Insert Anomaly

We cannot insert data into the table unless we know all the information. In this example we have to have an EmployeeId (it's the primary key) before we can add a new SalesOffice. That means that we have to wait until we have an employee working at an office before we can add the office information.

### Update Anomaly

Because the same information is stored in multiple rows, we will need to update each row and if the update is not successful across all the rows, we end up having data inconsistencies.

### Deletion Anomaly

Deleting a row can remove more than one piece of information. In the case where a single salesperson works in an office, if we delete that salesperson, we will also delete the information pertaining to the salesoffice.

## Search and Sort Issues

The last reason to consider is making it easier to search and sort data. If we did a search on *Ford* as a customer, we have to write the following or similar query:

```sql
SELECT SalesOffice
FROM SalesStaff
WHERE Customer1 = 'Ford' OR
      Customer2 = 'Ford' OR
      Customer3 = 'Ford'
```

If we wanted to sort by customer, we'd would not be able to in a single query.

We will look at how normalisation can help us improve this situation in next month's offering.

If you got this far, I have a challenge for you. How would you do perform a sort by customer across the data in table above? Give it a go... you may submit your answer to techketup@globalkinetic.com with the subject "data 2018-03".