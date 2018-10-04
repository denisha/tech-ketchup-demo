# Data

## *1NF - First Normal Form*

The first steps to making a proper SQL table is to ensure the information is in first normal form.  With a table in first normal form, it is easier to search, filter, and sort the information. The rules for 1st normal form are:

* That the data is in a database table.  The table stores information in rows and columns where one or more columns, called the primary key, uniquely identify each row.
* Each column contains atomic values, and there are not repeating groups of columns.

Tables in 1NF cannot contain sub columns. This means that is you are storing several cities, you cannot list them in one column, separated using a semi-colon or comma.

When a value is atomic it cannot be further subdivided. For example, _Cape Town_ is atomic; whereas _Cape Town, Durban, Pretoria_ is not. Related to this requirement, is that there should be no repeating groups of columns such as _City1Name, City2Name,_ and _City3Name_.

SalesStaffInformation

    EmployeeId (PK)    
    SalesPerson
    SalesOffice
    OfficeNumber
    Customer1Name
    Customer1City
    Customer1PostalCode
    Customer2Name
    Customer2City
    Customer2PostalCode
    Customer3Name
    Customer3City
    Customer3PostalCode

Our example table is transformed to 1NF by placing the repeating customer related columns into their own table:

SalesStaffInformation

    EmployeeId (PK)    
    SalesPerson
    SalesOffice
    OfficeNumber

<br/><br/>
Customer

    CustomerId (PK)
    EmployeeId (FK)
    CustomerName
    CustomerCity
    CustomerPostalCode


The repeating columns now become separate rows in the Customer table linked by the EmployeeId foreign key. Our original data now looks like this in 1NF

<table>
  <tr>
    <th>EmployeeId (PK)</th>
    <th>SalesPerson</th>
    <th>SalesOffice</th>
    <th>OfficeNumber</th>    
  </tr>
  <tr>
    <td>1003</td>
    <td>Mary Smith</td>
    <td>Chicago</td>
    <td>312-555-1212</td>
  </tr>
  <tr>
    <td>1004</td>
    <td>John Hunt</td>
    <td>New York</td>
    <td>212-555-1212</td>
  </tr>
  <tr>
    <td>1005</td>
    <td>Martin Hap</td>
    <td>Chicago</td>
    <td>312-555-1212</td>
  </tr>
</table>

<br/><br/>

<table>
  <tr>
    <th>CustomerId (PK)</th>
    <th>EmployeeId (FK)</th>
    <th>Name</th>
    <th>City</th>
    <th>PostalCode</th>
  </tr>
  <tr>
    <td>1000</td>
    <td>1003</td>
    <td>Ford</td>
    <td>Dearborn</td>
    <td>48123</td>
  </tr>
  <tr>
    <td>1010</td>
    <td>1003</td>
    <td>GM</td>
    <td>Detroit</td>
    <td>48213</td>
  </tr>
  <tr>
    <td>1020</td>
    <td>1004</td>
    <td>Dell</td>
    <td>Austin</td>
    <td>78720</td>
  </tr>
  <tr>
    <td>1030</td>
    <td>1004</td>
    <td>HP</td>
    <td>Palo Alto</td>
    <td>94303</td>
  </tr>
  <tr>
    <td>1040</td>
    <td>1004</td>
    <td>Apple</td>
    <td>Cupertino</td>
    <td>95014</td>
  </tr>
  <tr>
    <td>1050</td>
    <td>1005</td>
    <td>Boeing</td>
    <td>Chicago</td>
    <td>60601</td>
  </tr>
</table>

<br/>
This design is superior to our original table in several ways:

* The original design limited each SalesStaffInformation entry to 3 customers. In the new design, the number of customers associated to each entry is "unlimited".
* It was cumbersome to Sort the original data be Customer. Now it is simple.
* The same for filtering. It is much easier to filter on one customer name related comlumn than three.
* The insert and delete anomalies for Customer have been eliminated. You can delete all the customers for a SalesPerson without having to the delete the entire SalesStaffInformation row.

There are still modification anomalies that exist in both tables, but these are fixed once we reorganise them as 2NF. 

But that's for the next installment...