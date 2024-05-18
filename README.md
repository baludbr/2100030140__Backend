# Retail Store Database

This project contains SQL scripts for creating and managing a simple retail store database. The database consists of four tables: `Customers`, `Products`, `Orders`, and `OrderItems`.

## Table of Contents

- [Database Schema](#database-schema)
- [Data Insertion](#data-insertion)
- [Sample Queries](#sample-queries)
- [Usage](#usage)
- [Requirements](#requirements)
- [License](#license)

## Database Schema

The database schema includes the following tables:

### Customers

| Column      | Data Type | Description              |
|-------------|-----------|--------------------------|
| CustomerID  | INT       | Primary key              |
| FirstName   | VARCHAR   | Customer's first name    |
| LastName    | VARCHAR   | Customer's last name     |
| Email       | VARCHAR   | Customer's email         |
| DateOfBirth | DATE      | Customer's date of birth |

### Products

| Column      | Data Type | Description              |
|-------------|-----------|--------------------------|
| ProductID   | INT       | Primary key              |
| ProductName | VARCHAR   | Name of the product      |
| Price       | DECIMAL   | Price of the product     |

### Orders

| Column    | Data Type | Description              |
|-----------|-----------|--------------------------|
| OrderID   | INT       | Primary key              |
| CustomerID| INT       | Foreign key to Customers |
| OrderDate | DATE      | Date the order was placed|

### OrderItems

| Column      | Data Type | Description              |
|-------------|-----------|--------------------------|
| OrderItemID | INT       | Primary key              |
| OrderID     | INT       | Foreign key to Orders    |
| ProductID   | INT       | Foreign key to Products  |
| Quantity    | INT       | Quantity of the product  |

## Data Insertion

Here are the SQL statements to insert sample data into the tables:

```sql
INSERT INTO Customers (CustomerID, FirstName, LastName, Email, DateOfBirth) VALUES
(1, 'John', 'Doe', 'john.doe@example.com', '1985-01-15'),
(2, 'Jane', 'Smith', 'jane.smith@example.com', '1990-06-20');

INSERT INTO Products (ProductID, ProductName, Price) VALUES
(1, 'Laptop', 1000.00),
(2, 'Smartphone', 600.00),
(3, 'Headphones', 100.00);

INSERT INTO Orders (OrderID, CustomerID, OrderDate) VALUES
(1, 1, '2023-01-10'),
(2, 2, '2023-01-12');

INSERT INTO OrderItems (OrderItemID, OrderID, ProductID, Quantity) VALUES
(1, 1, 1, 1),
(2, 1, 3, 2),
(3, 2, 2, 1),
(4, 2, 3, 1);
```
## Sample Queries


1.List all customers:
```sql
SELECT * FROM Customers;
```
2.Find all orders placed in January 2023:
```sql
SELECT * FROM Orders
WHERE OrderDate BETWEEN '2023-01-01' AND '2023-01-31';
```
3.Get the details of each order, including the customer name and email:
```sql
SELECT Orders.OrderID, Customers.FirstName, Customers.LastName, Customers.Email, Orders.OrderDate
FROM Orders
JOIN Customers ON Orders.CustomerID = Customers.CustomerID;

````
4.List the products purchased in a specific order (e.g., OrderID = 1):
````sql
SELECT Products.ProductName, OrderItems.Quantity
FROM OrderItems
JOIN Products ON OrderItems.ProductID = Products.ProductID
WHERE OrderItems.OrderID = 1;
`````

5.Calculate the total amount spent by each customer:

````sql
SELECT Customers.CustomerID, Customers.FirstName, Customers.LastName, SUM(Products.Price * OrderItems.Quantity) AS TotalSpent
FROM Customers
JOIN Orders ON Customers.CustomerID = Orders.CustomerID
JOIN OrderItems ON Orders.OrderID = OrderItems.OrderID
JOIN Products ON OrderItems.ProductID = Products.ProductID
GROUP BY Customers.CustomerID, Customers.FirstName, Customers.LastName;
````
6.Find the most popular product (the one that has been ordered the most):

````sql
SELECT Products.ProductID, Products.ProductName, SUM(OrderItems.Quantity) AS TotalOrdered
FROM OrderItems
JOIN Products ON OrderItems.ProductID = Products.ProductID
GROUP BY Products.ProductID, Products.ProductName
ORDER BY TotalOrdered DESC
LIMIT 1;
````

7.Get the total number of orders and the total sales amount for each month in 2023:


````sql

SELECT strftime('%Y-%m', OrderDate) AS Month, 
       COUNT(*) AS TotalOrders, 
       SUM(Products.Price * OrderItems.Quantity) AS TotalSales
FROM Orders
JOIN OrderItems ON Orders.OrderID = OrderItems.OrderID
JOIN Products ON OrderItems.ProductID = Products.ProductID
WHERE strftime('%Y', OrderDate) = '2023'
GROUP BY strftime('%Y-%m', OrderDate);
````

8.Find customers who have spent more than $1000:

````sql

SELECT Customers.CustomerID, Customers.FirstName, Customers.LastName, SUM(Products.Price * OrderItems.Quantity) AS TotalSpent
FROM Customers
JOIN Orders ON Customers.CustomerID = Orders.CustomerID
JOIN OrderItems ON Orders.OrderID = OrderItems.OrderID
JOIN Products ON OrderItems.ProductID = Products.ProductID
GROUP BY Customers.CustomerID, Customers.FirstName, Customers.LastName
HAVING TotalSpent > 1000;
````
## Usage
To use this database, run the table creation and data insertion scripts in your SQL database management system. Then, you can execute the sample queries to interact with the database.

## Requirements
A SQL database management system (e.g., MySQL, SQLite, PostgreSQL, SQL Server)
SQL client or interface to run the scripts
## License
This project is licensed under the MIT License. See the LICENSE file for more details.  


This work is associated with  `2100030140_ D BALAJI REDDY`

