# Data

## *3NF - Third Normal Form*

Once a table is in 2NF, we are guaranteed that the table serves a single purpose and every column is dependant on the primary key. But what about dependancies between columns, can this cause inconsistency?

Third Normal Form addresses this issue.

A table is in 3NF if:

* The table is in 2nd normal form, and
* It contains only columns that are *non-transitively dependant* on the primary key.

What does *non-transitively dependant* mean?

### Transitive

When something is *transitive*, its meaning or relationship is the same in the middle as it is across the whole.
<br />
Mathematically we can think of it like so: if **A** is greater than **B**, and **B** is greater than **C**, then it can be inferred that **A** is greater than **C**.
<br />
Or... 10 > 5 and 5 > 3 therefore 10 > 3.
<br />
Simple, right?

What this means is that we need to check whether one column may be related to others, through a second column.

### Dependance

An object has a *dependance* on another object if it relies on it. In the case of databases, dependance means that the value of one column can be derived from another. For example, Age is dependant on Date of Birth (and also the current date).

## Transitive dependance

It is simplest to think of *transitive dependance* to mean that a column's value *relies* upon another column *through* a second intermediate column.

Consider three columns: AuthorNationality, Author and Book. Column values for AuthorNationality and Author rely on the Book; if you know the Book, you can find the Author or AuthorNationality. But also, AuthorNationality relies on Author because knowing the Author means you can determine the AuthorNationality. In this case, the AuthorNationality relies on the Book, via the Author. This is *transitive dependance*.

This can be generalised as being three columns: A, B and PK. If the value of A relies on PK, and B relies on PK, and A also relies on B, then A *relies* on PK *through* B. That means A is transitively dependant on PK.

Here are some examples:

| Primary Key (PK) | Column A | Column B | Transitive Depedence? |
|--------------|--------------|--------------|--------------|
| PersonID | FirstName | LastName | No, In Western cultures a person’s last name is based on their father’s LastName, whereas their FirstName is given to them. |
| PersonID | BodyMassIndex | IsOverweight | Yes, BMI over 25 is considered overweight. It wouldn’t make sense to have the value IsOverweight be true when the BodyMassIndex was < 25. |
| PersonID | Weight | Sex | No: There is no direct link between the weight of a person and their sex. |
| VehicleID | Model | Manufacturer | Yes: Manufacturers make specific models.  For instance, Ford creates the Fiesta; whereas, Toyota manufactures the Camry. |

To be non-transitively dependant means that all columns are dependant on the primary key (a criteria for 2NF), and no other columns in the table.

## Issues with our Data Model

In our Customer table there is one transitive dependancy:

Customer

    CustomerId (PK)
    CustomerName
    CustomerCity
    CustomerPostalCode

CustomerCity relies on CustomerPostalCode which relies on CustomerId. Generally speaking a postal code applies to one city. Although all the columns are dependant on the primary key, CustomerId, there is an opportunity for an update anomaly as you could update the CustomerPostalCode without making a corresponding update to the CustomerCity.

## Fix the model...

It is ok that CustomerPostalCode relies on CustomerId; however, we break 3NF by including CustomerCity in the table. To fix this we'll create a new table, PostalCode, which includes PostalCode as the PK and City as a column.

The CustomerPostalCode remains in the customer table as a foreign key. In this way, the city and postal code is still known for each customer, but we've now eliminated the update anomaly.

PostalCode

    PostalCode (PK)
    City

To better visualise this, here are the Customer and PostalCode tables with data

Customer

| CustomerId (PK) | Name | PostalCode (FK)|
|-----------------|--------|------------|
| 1000 | Ford | 48123 |
| 1010 | GM | 48213 |
| 1020 | Dell | 78720 |
| 1030 | HP | 94303 |
| 1040 | Apple | 95014 |
| 1050 | Boeing | 60601 |

PostalCode

| PostalCode (PK) | City |
|-------|-------|
| 48123 | Dearborn |
| 48213 | Detroit |
| 78720 | Austin |
| 94303 | Palo Alto |
| 95014 | Cupertino |
| 60601 | Chicago |

Now each column in the Customer table is dependant on the primary key and do not rely on each other for values. Their only dependancy is on the primary key.
<br />
The same holds true for the PostalCode table.

At this point our data model fulfills the requirements for Third Normal Form. For most practical purposes this is usually sufficient; however, there are cases where even more data model refinements can take place. Read about [BCNF (Boyce-Codd Normal Form)](http://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) and [more](http://en.wikipedia.org/wiki/Database_normalization), if you're interested in this.

## Can Normalisation get out of hand?

Can you take this too far? Yes, absolutely! There are times when it isn't worth the time and effort to fully normalise a database. We could have left our db in 2NF. The CustomerCity and CustomerPostalCode isn't really a deal breaker if we remember that both fields must be updated should either need to change.

Remember that a normalised database is optimised for writing data. Inserts, Updates and Deletes are more efficient and accurate. If introducing anomolies in these areas can severly impact your application, then you should normalise. If this is not the case, then you need to decide whether you can rely on the user (the developer) to recognise this and update the fields together.

There are times when you'll intentionally denormalise data. If you need to present summarised or compiled data to a user, and that data is time consuming or resource intensive to create, it may make sense to maintain this data separately.

*That's it. I hope you enjoyed this content. I will be looking at basic T-SQL queries and challenges at some Tech-(Tues|Wednes)days which I'll summarise in future segments.*