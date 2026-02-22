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

![diagrama](https://github.com/AlainAFT/Portafolio_Base_Datos_I_y_II/blob/main/imagenes/Pasted%20image%2020260216102731.png?raw=true)

El problema principal es que hace una operación innecesaria para la consulta porque las dos tablas que comparamos en el primer *JOIN* hacemos un casteo de los valores siendo que ambos datos son de tipo *INT*  .  
En el primer Join se esta comparando la llave primaria de la tabla producto, se nos dijo en clase de que cada llave primaria de las tablas tiene un índice especifico para esas llaves. 

Lo que se hace prácticamente el casteo de esta llave primaria es que cambia la forma de comparar la conexión entre tablas dentro de un *JOIN* ,  en vez de realizar una **Clustered Index Seek | búsqueda de índices** y saltando lo innecesario , realiza una búsqueda por **Clustered Index Scan|escaneo de índices** que hace que lee todas las filas de la tabla y aplicar el *cast* en cada una. 

Cuando Quitas el cast de esa consulta ves como en la tabla de **NESTED LOOP | bucles anidados** 
recorre mucho menos filas que el que tiene cast en su consulta.

 **Con cast:**

![diagrama](https://github.com/AlainAFT/Portafolio_Base_Datos_I_y_II/blob/main/imagenes/Pasted%20image%2020260216133658.png?raw=true)



En el apartado de **Numero real de filas por ejecución** es de 2604 filas

 **Sin cast :**

![](https://github.com/AlainAFT/Portafolio_Base_Datos_I_y_II/blob/main/imagenes/Pasted%20image%2020260216133841.png?raw=true)

Por ejemplo aquí en el mismo apartado tiene 5 filas para todas las ejecuciones.
Tiene mejores resultados , como en el costo estimado en la CPU , Costo de operador.

# Practica 2



**Consulta**
```
SELECT soh.SalesOrderID , ST.TerritoryID , c.CustomerID from Sales.SalesOrderHeader soh
inner join Sales.Customer C
on C.CustomerID = soh.CustomerID
inner join Sales.SalesTerritory ST
on st.TerritoryID = soh.TerritoryID
where  ST.TerritoryID = 6 and  soh.SalesOrderID > 40000 and C.CustomerID > 28000 
```

Es una consulta simple donde se quiere saber cuales son las ordenes mayores de 40000 , del `consumidorid` mayor de 28000 y que hayan sido en el `territorioid` = 6

La consulta ya en si es rápida pero con la ayuda de los índices se puede conseguir en menos tiempo.

# sin índices

![[Pasted image 20260222142632.png]]
# con índice


![[Pasted image 20260222142700.png]]

**Creación del índice**

```
create nonclustered index IDX_SOH_COSTUMER_TERRITORY
on Sales.SalesOrderHeader (TerritoryId,CustomerId,SalesOrderId)
```
Cree una conexión entre estos 3 atributos de la tabla SALESORDERHEADER para que puedan ser filtrados en menos tiempo en la condición  "WHERE"

**Mejoras tras la inclusión de este índice**

Realmente , si se logra apreciar en las capturas de imagen , cada sección del plan ejecución se ha visto mejorada en el tiempo de respuesta. Aunque haya sido mínimo el tiempo de mejora que rondo en milisegundos.

Donde si se ha visto mucho cambio a sido en el Costo de CPU  porque cuando no tienes el índice creado usa la búsqueda de índices `Cluster`  , al usar un `JOIN` para tabla `salesorderheader` llamara a todas las filas de esa tabla y es por eso que cuando ves en el plan de ejecución el numero de filas leídas en la búsqueda de índices `cluster` de esta tabla sale `31465` filas leídas y en cambio cuando el índice `nonclustered` existe entre `TERRITORIID , SALESORDERID Y CUSTOMERID`  el numero de filas leídas se reduce a `847`


