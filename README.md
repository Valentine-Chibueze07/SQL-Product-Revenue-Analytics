# SQL-Product-Revenue-Analytics
SQL Product Revenue Analytics (Project 4)
 **Overview**

This project analyzes product-level sales performance using SQL Server.

It focuses on order line items to evaluate:

SKU revenue contribution

Category performance by state

Unit of measure (UOM) trends

Basket-level order composition

Product-specific sales patterns

All business logic was implemented using T-SQL.

 **Business Objectives**

Identify top-performing SKUs

Measure revenue by product category

Analyze quantity sold by UOM

Detect orders with no line items

Investigate specific product conditions (e.g., Rice 5kg in Carton UOM)

Build reusable basket-level reporting view

 **Technical Skills Demonstrated**

CRUD operations on line items

INNER JOIN across multiple tables

LEFT JOIN anomaly detection

GROUP BY & aggregation

Filtering with conditions

Revenue ranking

View creation (vw_OrderBasket)

Multi-table business logic implementation

 **Key SQL Highlights**
✔ Top 10 SKUs by Revenue (Delivered Only)
SELECT TOP 10
    SKUName,
    SUM(LineTotal) AS TotalRevenue
FROM SalesOrderLines L
INNER JOIN SalesOrders O
    ON L.OrderID = O.OrderID
WHERE O.Status = 'Delivered'
GROUP BY SKUName
ORDER BY TotalRevenue DESC;

✔ Category Revenue Split per State
SELECT
    R.StateName,
    L.Category,
    SUM(L.LineTotal) AS Revenue
FROM SalesOrderLines L
INNER JOIN SalesOrders O ON L.OrderID = O.OrderID
INNER JOIN Retailers R ON O.RetailerID = R.RetailerID
GROUP BY R.StateName, L.Category
ORDER BY R.StateName;

✔ Orders With No Lines (Data Integrity Check)
SELECT O.OrderNumber
FROM SalesOrders O
LEFT JOIN SalesOrderLines L
    ON O.OrderID = L.OrderID
WHERE L.LineID IS NULL;

✔ View Creation – Order Basket
CREATE VIEW vw_OrderBasket AS
SELECT
    O.OrderNumber,
    L.SKUName,
    L.Category,
    L.UOM,
    L.Qty,
    L.UnitPrice,
    L.LineTotal
FROM SalesOrderLines L
INNER JOIN SalesOrders O
    ON L.OrderID = O.OrderID;

**Key Insights Generated**

Identified highest revenue SKUs

Measured state-level product demand

Evaluated category dominance

Flagged incomplete orders

Built reusable basket intelligence view

 **Business Impact**

This project simulates:

✔ Product profitability analysis
✔ Regional demand tracking
✔ SKU optimization strategy
✔ Revenue concentration monitoring
✔ Commercial sales mix analysis
