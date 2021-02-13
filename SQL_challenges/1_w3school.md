# Part 1: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?

SELECT * FROM Customers WHERE Country = 'UK';

2. What is the name of the customer who has the most orders?

SELECT Customers.CustomerName, Max_order.count_orders
FROM
(SELECT  CustomerID, COUNT(*) as count_orders 
FROM Orders 
GROUP BY CustomerID
ORDER BY COUNT(*) DESC
LIMIT 1) AS Max_order
LEFT JOIN Customers ON Max_order.CustomerID = Customers.CustomerID
;

Result:
CustomerName	count_orders
Save-a-lot Markets	31

3. Which supplier has the highest average product price?

SELECT Suppliers.SupplierName, Avg_sup.avg_price
FROM
(SELECT SupplierID, AVG(Price) as avg_price 
FROM Products 
GROUP BY SupplierID 
ORDER BY avg_price DESC
LIMIT 1) AS Avg_sup
LEFT JOIN Suppliers ON Avg_sup.SupplierID = Suppliers.SupplierID
;

Result:
SupplierName	avg_price
Aux joyeux ecclésiastiques	140.750000

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

SELECT COUNT(DISTINCT(Country)) FROM Customers;

Result: 21

5. What category appears in the most orders?

SELECT CategoryName, SUM(Quantity) as sum_q
FROM
(SELECT Categories.CategoryName as CategoryName, Products.ProductName, OrderDetails.Quantity as Quantity
FROM (( Products
LEFT JOIN Categories ON Products.CategoryID = Categories.CategoryID)
LEFT JOIN OrderDetails ON Products.ProductID = Products.ProductID)) AS dt_cat_sum
GROUP BY CategoryName
ORDER BY sum_q DESC
LIMIT 1
;

Result:
CategoryName	sum_q
Confections	667121

6. What was the total cost for each order?

SELECT OrderDetails.OrderID, SUM(OrderDetails.Quantity * Products.Price) as mult
FROM OrderDetails
LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID
GROUP BY OrderID;

7. Which employee made the most sales (by total price)?

SELECT Employees.LastName, Employees.FirstName, SUM(Dt_order_total.order_total) as total
FROM
(SELECT OrderDetails.OrderID, SUM(OrderDetails.Quantity * Products.Price) as order_total
FROM OrderDetails
LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID
GROUP BY OrderID) AS Dt_order_total
LEFT JOIN Orders ON Orders.OrderID = Dt_order_total.OrderID
LEFT JOIN Employees ON Employees.EmployeeID = Orders.EmployeeID
GROUP BY Orders.EmployeeID
ORDER BY total DESC
LIMIT 1
;

Result:
LastName	FirstName	total
Peacock	Margaret	275109.98

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

SELECT * FROM Employees
WHERE Notes LIKE '%BS%';

Result:
EmployeeID: 3, 5

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

SELECT Suppliers.SupplierName, DT_avg.avg_price
FROM
(SELECT SupplierID, AVG(Price) as avg_price
FROM Products 
GROUP BY SupplierID
HAVING COUNT(ProductID) >= 3
ORDER BY avg_price DESC
LIMIT 1) AS DT_avg
LEFT JOIN Suppliers ON Suppliers.SupplierID = DT_avg.SupplierID
;

Result:
SupplierName	avg_price
Tokyo Traders	46.000000