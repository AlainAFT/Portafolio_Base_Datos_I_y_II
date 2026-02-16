# Practica 1 - Plan de ejecución

```
USE AdventureWorks2022;
GO

SELECT 
    p.Name,
    SUM(sod.OrderQty) AS TotalCantidad,
    SUM(sod.LineTotal) AS TotalVenta
FROM Sales.SalesOrderDetail sod
JOIN Production.Product p
    ON CAST(sod.ProductID AS VARCHAR(50)) = CAST(p.ProductID AS VARCHAR(50))
JOIN Sales.SalesOrderHeader soh
    ON sod.SalesOrderID = soh.SalesOrderID
WHERE 
    YEAR(soh.OrderDate) = 2013
    AND p.Name LIKE '%Bike%'
GROUP BY 
    p.Name
ORDER BY 
    TotalVenta DESC;
GO
```

Explicar porque es ineficiente la consulta mirando su plan de ejecución

**esquema del plan de ejecución**

![[Pasted image 20260216102731.png]]
