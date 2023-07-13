# Northwind_Traders_Analysis

The database contains the sales data for Northwind Traders, a
fictitious speciality foods export/import company.
Analysing the sales for Northwind helps to give insights as to how the company has been performing. Northwind Traders have total number of:
- 9 Employees
- 91 Customers
- 77 Products of which 8 are no longer being sold
- 29 Suppliers
  
The dataset has been cleaned and Exploratory Data Analysis (EDA) has been carried out using SQL and Tableau for visualization.

## About the dataset
This dataset was gotten from this [repository](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)
It contains the following tables:

- Customers – who buy from Northwind
- CustomerDemographics
- Employee – who work for Northwind
- EmployeeTerritories
- OrderDetails – details of the transactions taking place
- Orders - details of the orders
- Products – the products that Northwind trades in
- Region
- Shippers - details of the shippers who ship the products
- Suppliers - who supply to the company
- Territories

To give Northwind traders a better undestanding of their operations, it is best to carry out an Exploratory Data Analysis (EDA) using SQL


## Exploartory Data Analysis
### List of products per unit quantity
A total of 77 products were recorded at the start of the sales year.
```
SELECT ProductName, QuantityPerUnit
FROM Products
```
### Current Product List
Presently, these are the products that are still being sold by Northwind traders.
```
SELECT ProductID, ProductName
FROM Products
WHERE Discontinued = 0
ORDER BY ProductID
```

### Discontinued product list
A total of 8 products are no longer being supplied by Northwind traders.
```
SELECT ProductID, ProductName
FROM Products
WHERE Discontinued = 1
```

### Most expensive and least expensive product
Identifying the most expensive and least expensive product gives insight on the sales perfomance of each product.
```
SELECT 'Most expensive',  ProductName, UnitPrice
FROM Products
WHERE UnitPrice = (SELECT  MAX(UnitPrice)
FROM Products)
UNION
SELECT 'Least expensive' AS category ,ProductName, UnitPrice
FROM Products
WHERE UnitPrice = (SELECT  MIN(UnitPrice)
FROM Products)
ORDER BY UnitPrice DESC
```
### Products that cost less than $20
```
SELECT  ProductId, ProductName, UnitPrice
FROM Products
WHERE UnitPrice < 20
```

### Products between $15 and $25
```
SELECT ProductId, ProductName, UnitPrice
FROM Products
WHERE UnitPrice BETWEEN 15 AND 25
ORDER BY UnitPrice;
```
### Products above average price
```
SELECT ProductName, UnitPrice
FROM Products
WHERE UnitPrice > (
    SELECT AVG(UnitPrice)
    FROM Products
    )
ORDER BY UnitPrice;
```

### Ten most expensive products
```
SELECT TOP 10 ProductName, UnitPrice
FROM Products
ORDER BY UnitPrice DESC;
```

### Current and discontinued products
```
SELECT 'Current products' AS Product_status, COUNT(*) AS Number_of_products
FROM Products
WHERE Discontinued = 0
UNION
SELECT 'Discontinued products', COUNT(*)
FROM Products
WHERE Discontinued = 1
```

### Stock less than quantity on order
```
SELECT ProductName, UnitsOnOrder,UnitsInStock
FROM Products
WHERE UnitsInStock < UnitsOnOrder
```

### Subtotal of each order
```
SELECT OrderId, ROUND(SUM(UnitPrice- (UnitPrice*Discount)),2) AS Newprice,
ROUND(SUM((UnitPrice- (UnitPrice*Discount))*Quantity),2) AS Sales
FROM [Order Details]
GROUP BY OrderId;

```

### Employees sales amount by country
```
SELECT  o.EmployeeID,Country AS Employee_Country,
ROUND(SUM((UnitPrice- (UnitPrice*Discount))*Quantity),2) AS Sales
FROM Orders AS o
INNER JOIN [Order Details] AS od
ON od.OrderID = o.OrderID
INNER JOIN Employees AS e
ON o.EmployeeID=e.EmployeeID
GROUP BY  o.EmployeeID,e.Country
order by  EmployeeID asc
```

### List of products in alphabetical order
```
SELECT ProductName
FROM Products
ORDER BY ProductName ASC;
```

### Total sales by Category
```
SELECT CategoryName,
ROUND(SUM((od.UnitPrice- (od.UnitPrice*Discount))*Quantity),2) AS Sales
FROM Products AS p
INNER JOIN Categories AS c
ON p.CategoryID=c.CategoryID
INNER JOIN [Order Details] AS od
ON od.ProductID=p.ProductID
GROUP BY CategoryName
```

### customers and suppliers by city
```
SELECT 'Customer' AS Class, CompanyName,City
FROM Customers
UNION
SELECT 'Supplier', CompanyName ,City
FROM Suppliers
ORDER BY City ASC
```
