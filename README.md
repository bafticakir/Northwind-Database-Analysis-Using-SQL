# 🗂 Northwind Database Analysis Using SQL

This project explores the Northwind sample database using various SQL queries to demonstrate filtering, joining, aggregation, and data exploration techniques. It’s ideal for practising SQL in a real-world relational schema.

---

## 🧭 Database Exploration

We start by examining the structure of key tables:

- `categories` – Contains product category information  
- `customers` – Stores customer data including names and contact details  
- `employees` – Stores employee data including name, birth date, ID, notes, and photo  
- `order_details` – Contains order details such as OrderID, ProductID, and Quantity  
- `products` – Contains product-related data like name, price, quantity per unit, and foreign keys to category and supplier  
- `shippers` – Information about shipping companies used to deliver orders  
- `suppliers` – Supplier data including company name, contact details, and location  

```sql
USE northwind;
DESC categories;
DESC customers;
```

---

## 🔎 Filtering & Pattern Matching

```sql
-- Customers whose names start with 'A'
SELECT * 
FROM customers
WHERE CustomerName LIKE 'a%';

-- Customers whose names end with 'a'
SELECT * 
FROM customers
WHERE CustomerName LIKE '%a';

-- Customers whose names match 'Q_e%'
SELECT *
FROM customers 
WHERE CustomerName LIKE 'Q_e%';
```

---

## 🔗 Join Operations

```sql
-- Products supplied by "Tokyo Traders"
SELECT *
FROM products
INNER JOIN suppliers ON products.SupplierID = suppliers.SupplierID
WHERE SupplierName = 'Tokyo Traders'
ORDER BY products.ProductName;

-- Product IDs
SELECT ProductID FROM products;

-- All product details
SELECT * FROM products;

-- Customers who have never placed an order
SELECT a.*, b.*
FROM customers AS a
LEFT JOIN orders AS b ON a.CustomerID = b.CustomerID
WHERE b.OrderID IS NULL;

-- Products never ordered
SELECT *
FROM products AS p
LEFT JOIN order_details AS od ON p.ProductID = od.ProductID
WHERE od.OrderID IS NULL;

-- Cross join: Customer and Employee (specific customer)
SELECT * 
FROM customers
CROSS JOIN employees
WHERE customers.CustomerName = 'Alfreds Futterkiste';

-- All customers and their orders using LEFT + RIGHT JOIN
SELECT *
FROM customers
LEFT JOIN orders ON customers.CustomerID = orders.CustomerID
WHERE orders.CustomerID IS NOT NULL

UNION

SELECT *
FROM customers
RIGHT JOIN orders ON customers.CustomerID = orders.CustomerID
WHERE customers.CustomerID IS NOT NULL;
```

---

## 📊 Aggregations & Grouping

```sql
-- Count cities in each country (UK only)
SELECT COUNT(DISTINCT City) AS 'Number of customer', Country, City 
FROM customers
GROUP BY Country, City
HAVING City = 'uk';

-- Orders count by shipper
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders 
FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;

-- Total sales per product
SELECT 
    a.ProductName, 
    a.Price, 
    SUM(b.Quantity) AS total_quantity, 
    SUM(b.Quantity * a.Price) AS TotalSales
FROM products a
JOIN order_details b ON a.ProductID = b.ProductID
GROUP BY a.ProductName, a.Price
ORDER BY TotalSales DESC;

-- Alternate total product sales
SELECT p.ProductName, SUM(od.Quantity * p.Price) AS Sales
FROM products AS p
INNER JOIN order_details AS od ON p.ProductID = od.ProductID
GROUP BY p.ProductName;

-- Customers per country
SELECT COUNT(*) AS 'Customers#', Country
FROM customers
GROUP BY Country
ORDER BY `Customers#` DESC;

-- Quantity sold per product
SELECT SUM(o.Quantity) AS 'TotalQuantity', p.ProductName
FROM products p
JOIN order_details o ON p.ProductID = o.ProductID
GROUP BY p.ProductName
ORDER BY TotalQuantity DESC;

-- Quantity sold by category
SELECT c.CategoryName, COUNT(CategoryName) AS 'Quantity sold'
FROM products AS p
INNER JOIN categories AS c ON p.CategoryID = c.CategoryID
GROUP BY CategoryName;

-- Orders handled by each employee
SELECT e.EmployeeID, e.FirstName, COUNT(*) AS 'TotalOrders'
FROM employees e
JOIN orders o ON e.EmployeeID = o.EmployeeID
GROUP BY e.EmployeeID
ORDER BY e.EmployeeID;
```

---

## 🧾 Entity Details & Relationships

```sql
-- Supplier and product combinations
SELECT DISTINCT s.SupplierName, p.ProductName
FROM suppliers s
JOIN products p ON s.SupplierID = p.SupplierID;

-- Category of each product
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID;

-- Products in "Meat/Poultry" category
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID
WHERE c.CategoryName = 'Meat/Poultry';

-- Order info with customer and employee names
SELECT o.OrderID, o.OrderDate, c.CustomerName, CONCAT(e.FirstName, ' ', e.LastName) AS EmployeeName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
JOIN employees e ON o.EmployeeID = e.EmployeeID;

-- Product, category, and supplier info
SELECT p.ProductName, c.CategoryName, s.SupplierName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID
JOIN suppliers s ON p.SupplierID = s.SupplierID;

-- Orders from 1996
SELECT o.OrderID, o.OrderDate, c.CustomerID, c.CustomerName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
WHERE YEAR(o.OrderDate) = 1996;

-- Product count by category
SELECT c.CategoryName, COUNT(p.ProductName) AS NoOfProducts
FROM categories c
LEFT JOIN products p ON p.CategoryID = c.CategoryID
GROUP BY c.CategoryName
ORDER BY NoOfProducts;
```

---

## 📦 Technologies Used

- **MySQL** – Querying and data analysis  
- **SQL Workbench / CLI** – Query execution environment

---

## ✅ Objectives

- Practise SQL skills on a realistic schema  
- Analyse customer, product, and order data  
- Use joins, filters, pattern matching, and aggregations  
- Extract insights into sales, customer trends, and supply chain

---

## 📌 How to Use

1. Clone this repository  
2. Load the `Northwind` database into MySQL  
3. Run the `.sql` files or manually test the queries in a SQL editor

---

> Created with 🔥 by **Bafti Çakır**
