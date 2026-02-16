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
El problema principal es que hace una operación innecesaria para la consulta porque las dos tablas que comparamos en el primer *JOIN* hacemos un casteo de los valores siendo que ambos datos son de tipo *INT*  .  
En el primer Join se esta comparando la llave primaria de la tabla producto, se nos dijo en clase de que cada llave primaria de las tablas tiene un índice especifico para esas llaves. 

Lo que se hace prácticamente el casteo de esta llave primaria es que cambia la forma de comparar la conexión entre tablas dentro de un *JOIN* ,  en vez de realizar una **Clustered Index Seek | búsqueda de índices** y saltando lo innecesario , realiza una búsqueda por **Clustered Index Scan|escaneo de índices** que hace que lee todas las filas de la tabla y aplicar el *cast* en cada una. 

Cuando Quitas el cast de esa consulta ves como en la tabla de **NESTED LOOP | bucles anidados** 
recorre mucho menos filas que el que tiene cast en su consulta.

# **Con cast:**

![[Pasted image 20260216133658.png]]

En el apartado de **Numero real de filas por ejecución** es de 2604 filas

# **Sin cast :**

![[Pasted image 20260216133841.png]]
Por ejemplo aquí en el mismo apartado tiene 5 filas para todas las ejecuciones.
Tiene mejores resultados , como en el costo estimado en la CPU , Costo de operador.

# Practica 2