Assignment

Topic: Denormalization and Controlled Redundancy


1. Introduction

In database design, normalization is used to eliminate redundancy, reduce update anomalies, and ensure data integrity. Typically, relations are normalized up to Third Normal Form (3NF) or higher during the logical database design phase.

However, as highlighted by Connolly and Begg, highly normalized databases may sometimes suffer from performance problems because many user queries require multiple join operations. These joins can increase query execution time, especially in large databases with heavy transaction loads.

To address this issue, denormalization is used as a performance optimization technique during the physical database design phase. Denormalization improves data retrieval performance by introducing a controlled amount of redundancy into the database.



2. Definition of Denormalization

Denormalization is defined as:

"The deliberate introduction of redundancy into a normalized database to improve the performance of query processing"

It is important to note that denormalization:

- Is applied after normalization has been completed.
- Is based on analysis of transaction and query performance.
- Is used selectively where performance benefits outweigh the disadvantages.
- Does not replace normalization but complements it.


3. Controlled Redundancy

Denormalization introduces redundancy, but the redundancy must be carefully managed.

Controlled redundancy means:

- Data duplication is intentional and planned.
- Additional storage is accepted in exchange for better performance.
- Database triggers, stored procedures, or application logic are used to maintain consistency.
- The risk of update anomalies is minimized through proper controls.

According to Connolly and Begg, controlled redundancy should only be introduced when there is clear evidence that it will improve system performance.


4. Purpose of Denormalization

The main purposes of denormalization are:

- Reduce complex join operations.
- Improve query response time.
- Optimize read-heavy systems such as reporting and decision-support systems.
- Improve performance in very large databases.
- Reduce the amount of processing required for frequently executed queries.
- Enhance user experience by providing faster access to information.


5. Denormalization Techniques

According to Connolly and Begg, several techniques can be used to introduce controlled redundancy.

 Technique 1: Combining One-to-One (1:1) Relationships

Definition :

Combining two tables that share a one-to-one relationship into a single table.

Purpose :

To eliminate join operations and improve data retrieval performance when both tables are frequently accessed together.

Process :

- Identify two tables linked by a 1:1 relationship.
- Determine whether the tables are usually accessed together.
- Merge the attributes of both tables into a single relation.
- Remove unnecessary joins.

Example :

Before Denormalization :

Student Table :

StudentID| Name
S101| Alice

Scholarship Table :

StudentID| ScholarshipName| Amount
S101| Merit Award| 5000

After Denormalization :

Student Table :

StudentID| Name| ScholarshipName| Amount
S101| Alice| Merit Award| 5000

Benefits :

- Faster retrieval of student information.
- Reduced join processing.
- Simplified queries.

SQL Implementation :

CREATE TABLE Student (
    StudentID VARCHAR(10) PRIMARY KEY,
    Name VARCHAR(50),
    ScholarshipName VARCHAR(50),
    Amount INT
);

 Technique 2: Duplicating Non-Key Columns (1:* Relationship)

Definition :

Copying a non-key attribute from the parent table (one side) to the child table (many side).

Purpose :

To avoid repeated joins when users frequently require descriptive information from the parent table.

Process :

- Identify attributes frequently used in queries.
- Duplicate the selected attribute in the related table.
- Ensure consistency through triggers or application updates.

Example (E-Commerce System)

Normalized Query (Requires JOIN) :

SELECT oi.OrderID, p.ProductName, oi.Quantity
FROM Order_Items oi
JOIN Products p
ON oi.ProductID = p.ProductID;

After Denormalization : 

ALTER TABLE Order_Items
ADD ProductName VARCHAR(100);

SELECT OrderID, ProductName, Quantity
FROM Order_Items;

Benefits : 

- Faster invoice generation.
- Reduced join operations.
- Improved reporting performance.

Limitation :

If the product name changes, all duplicated values must also be updated to maintain consistency.



Technique 3: Storing Derived Data

Definition :

Storing calculated values instead of calculating them every time a query is executed.

Purpose :

To reduce processing overhead and improve performance of frequently executed calculations.

Process :

- Identify frequently calculated values.
- Store the computed result in a separate attribute.
- Update the stored value whenever underlying data changes.

Example : 

Before (Calculated Each Time) :

SELECT SUM(Quantity * Price) AS TotalAmount
FROM OrderDetails
WHERE OrderID = 101;

After Denormalization :

ALTER TABLE Orders
ADD TotalAmount DECIMAL(10,2);

Benefits :

- Faster query execution.
- Reduced CPU usage.
- Improved performance for reports and dashboards.



6. Real-Life Scenario

Banking System :

In a banking system, account balances are frequently requested by customers through ATMs, mobile banking applications, and online portals.

Calculating the balance from all transaction records every time would be inefficient and time-consuming.

Solution :

Store the current balance directly in the Account table and update it whenever a transaction occurs.

Account Table :

AccountID| Balance
A101| 25000

Transaction Table :

TransactionID| AccountID| Amount
T1001| A101| 5000

Benefits :

- Faster balance retrieval.
- Reduced computational cost.
- Better customer experience.
- Improved system performance.


7. Comparison: Normalized vs. Denormalized

Feature     | Normalized (3NF)             | Denormalized
1.Performance | Slower for complex queries  | Faster query execution
2.Data Integrity|  High                      | Risk of inconsistency
3.Storage     | Efficient                    | Increased redundancy
4.Maintenance | Easier                       | More complex
5.Query Complexity| More joins required      | Fewer joins required


8. Risks and Considerations :

According to Connolly and Begg:

- Denormalization may introduce update anomalies.
- Additional storage space is required.
- Data consistency becomes more difficult to maintain.
- Extra mechanisms are needed to synchronize duplicated data.
- Poorly planned denormalization can create maintenance problems.

Therefore, denormalization should only be implemented after careful performance evaluation.

9. Best Practices :

- Always normalize the database first.
- Apply denormalization only after performance analysis.
- Use denormalization selectively for frequently executed queries.
- Maintain consistency using triggers, stored procedures, or application logic.
- Continuously monitor performance improvements after implementation.


10. Conclusion :

Denormalization and controlled redundancy are important techniques for improving database performance. While normalization focuses on eliminating redundancy and ensuring data integrity, denormalization improves efficiency by reducing query complexity and minimizing join operations.

According to Connolly and Begg, denormalization should be applied carefully and only when performance gains justify the additional storage requirements and maintenance effort. A balanced approach between normalization and denormalization leads to an efficient and reliable database system.


