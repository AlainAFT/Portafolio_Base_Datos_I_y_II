#sqlserver  #ISW #SIS-221  #semetre_3 #_2026  #base_de_datos_II
# Clase 1

13 de febrero de 2026
# Comandos Básicos parte 2

## DCL

## TCL

# TABLAS TEMPORALES
## Tablas Temporales Locales 

**características** 

*Solo existen en su pagina en la que fue creada y no en las demas*

*eliminar :*  aunque se elimina por si solo y me refiero cuando cierres la sesión de SQL o tu gestor de base de datos se eliminara automáticamente .
pero puedes usar 

`Delete if exist`

*ejemplo :* 

`IF OBJECT_ID('tempdb..#NombreTabla') IS NOT NULL`
    `DROP TABLE #NombreTabla;`   

*estructura :*

`Select into # " Nombre de la tabla temporal "`
`// codigo restante` 


*ejemplo :*

`Select  A.id  into # Autores Actuales`
`from Autores as A`
`inner join Materiales as M`
`on A.id = M.idA`
`where M.tipo = 'A'`

**Implementaciones :**

Nos sirve para poder alivianar las consultas que hacemos en SQL , normalmente sirve como para guardar resultados de una unión y guardarla para ocuparla para que no se repite pero obviamente si esta dentro de un ciclo de algún tipo. 

Esto no solo nos ahorra el costoso procesamiento de consultas, sino que incluso puede ayudar a hacer que su código se vea un poco más limpio.

==Sin embargo, hay un punto que quiero resaltar. Si la sesión en la que estamos trabajando tiene sesiones anidadas posteriormente, las tablas temporales de SQL Server serán visibles en las sesiones inferiores de la jerarquía, pero no arriba en la jerarquía. Permítame visualizar esto.==

**Diagrama :**

En este diagrama rápido, se va a crear una tabla temporal de SQL en la sesión 2. Las sesiones siguientes (sesiones 3 y 4) pueden ver la tabla temporal de SQL Server. Pero la sesión 1, que está por encima de la sesión 2, no podrá ver la tabla temporal de SQL Server.



![Diagrama](https://www.sqlshack.com/wp-content/uploads/2017/02/word-image-81.png)



La tabla temporal de SQL se descarta o destruye una vez que la sesión se desconecta. Muchas veces podrá ver que los desarrolladores usan el comando “DROP # Table_Name” al final de su declaración solo para poder limpiar la tabla. Pero depende totalmente de usted y de lo que está tratando de lograr.

También tiene que tener en cuenta que, en caso de conflicto de nombres (recuerde que las tablas temporales de SQL Server se crean en tempdb), el servidor SQL agregará un sufijo al final del nombre de la tabla para que sea único dentro de la base de datos tempdb. Pero este proceso es transparente para el desarrollador/usuario. Puede utilizar el mismo nombre que declaró ya que está limitado a esa sesión.

## Tabla Temporales Globales


**Definición:**

Son lo contrario de las locales por el nombre nos indica de que este tipo tablas pueden ser vista por todas las sesiones que tengamos anidadas , y al igual que las ==tablas temporales== se eliminaran cuando la sesión se desconecta y ya no hay mas referencias a la tabla.


Siempre puede tratar de usar el comando “DROP” para limpiarlo manualmente. Lo cual es algo que le recomendaría.

![Diagrama](https://s33046.pcdn.co/wp-content/uploads/2017/02/word-image-82.png)

**declaración**:

Para tratar de crear una tabla temporal global de SQL, simplemente use los dos símbolos de libra delante del nombre de la tabla.

**Ejemplo :**
    `## Global_Table_Name.`

# variables de tabla

las variables se declaran utilizando la instrucción de  ==DECLARE== 

Muchos creen que las variables de tabla existen solo en la memoria, pero eso simplemente no es cierto. Ya que residen en la base de datos tempdb de forma muy similar a las tablas temporales de SQL Server locales.

En lo que respecta al rendimiento, las variables de tabla son útiles con pequeñas cantidades de datos (como solo unas pocas filas). De lo contrario, una tabla temporal de SQL Server es útil al examinar grandes cantidades de datos. Ahora, para la mayoría de los scripts, lo más probable es que vea el uso de una tabla temporal de SQL Server en lugar de una variable de tabla. Esto no quiere decir que uno sea más útil que el otro, solo tiene que elegir la herramienta adecuada para el trabajo.

**EJEMPLO:**


```
DECLARE @TotalProduct AS TABLE

(ProductID INT NOT NULL PRIMARY KEY,

Quantity INT NOT NULL)

INSERT INTO @TotalProduct

         ( [ProductID], [Quantity] )

SELECT

  A.[ProductID],

  [Quantity] = SUM(B.Quantity)

FROM dbo.Product AS A

INNER JOIN dbo.SalesDetails AS B ON A.ProducitID = B.ProductID
```
# clase 2 

16 de febrero de 2026

# metodos de acceso

## recorrido secuencial

Es un recorrido del registro completo y de manera lineal , es complicado o ineficiente al buscar cosas en especifico.

## recorrido por claves

El recorrido por claves hace referencia a las `indices agrupadas | clustered index` que son índices que se crean automáticamente para cada llave primaria de cada tabla. Asi hace de que cada vez que hay conexiones entre tablas estas llaves tienen un acceso rápido.

# recorrido por índices

El recorrido es por relaciones que creas entre llaves o columnas se las conoce como `Indices no agrupados | no clustered index`  

## recorrido por `Hashing`

Trata de crear una llave con datos o con las características del objeto o dato y pues el chiste es que cada dato tenga diferente llave de manera aleatoria.

**Ejemplo:**

Digamos un `string`:

Dormir  y que creamos una función que atreves de operaciones cifre una llave para esta para palabra para que se pueda acceder de manera directa al dato . Si hacemos referencia al `ASCII` pues cada letra tiene un numero y pues digamos que la suma de dichas letras es nuestro codigo. Eso seria un ejemplo simple.




# clase 3

20 de febrero 2026

# índices

## índices agrupados

Usualmente son las llaves primarias en las tablas

Se crea solo no intervienes 

Tener cuidado al usar funciones porque puede cambiar el tipo de búsqueda 

## `índices no clusterizados`

Su creación son manuales 

No crear demasiados `indices no clusterizados` en una misma tabla porque cuando consultes esa tabla sera mas lenta de los normal


Limite de creación son 999  `índices no clusterizados`

Necesitan mantenimiento para estos tipos de índices

breve mención de las [[Vistas]] en `SQL`


# clase 4

# Optimización de consultas 

## orden de ejecución de consultas

**Jerarquía en la ejecución en las consultas :**

![[Pasted image 20260223092326.png]]


**Operadores Lógicos**

![[Pasted image 20260223092420.png]]

**jerarquía de operadores lógicos**

![[Pasted image 20260223092457.png]]
## técnica de reescritura de consultas

Si se puede permite usar un `JOIN implicito` es mucho mejor que el `JOIN EXPLiCITO`

**JOIN explicito** 


![[Pasted image 20260223092743.png]]

**JOIN implícito**

Es mas eficiente pero es mas cargada para tu `cpu` porque trae toda la tabla `product y productsubcategory` Y luego compara las llaves primarias. 

![[Pasted image 20260223092751.png]]

**subconsultas correlacionales**

la subconsulta depende de la primera porque necesita el `DEPARTMENTID` para poder sacar el promedio de salario de ese departamento.

Osea se ejecuta varias veces este `AVG()` , mas o menos uno por fila

```
SELECT e.EmployeeID, e.Name, e.Salary
FROM Employees e
WHERE e.Salary > (
    SELECT AVG(Salary)
    FROM Employees
    WHERE DepartmentID = e.DepartmentID  -- referencia a la fila exterior
);
```

**subconsultas independientes**

Por ejemplo aquí no necesita nada de la anterior consulta solo espera el valor del salario de cada empleado y se compara con el promedio de todos los empleados

```
SELECT e.EmployeeID, e.Name, e.Salary
FROM Employees e
WHERE e.Salary > (
    SELECT AVG(Salary)
    FROM Employees
);
```

**Evitar Operaciones costosas , usa las esenciales porque funciones de agregación o operaciones de ordenamiento son pesadas para el rendimiento**

**Aprovecha los índices**

# clase 5 

# Manejo de buffer 

## `Buffer`

Es una pagina de `8 KB` donde se almacenan datos en la memoria , y podemos decir que la memoria esta conformada por capas de `buffer`. 

## grupo de `buffers`

El **grupo de `buffers`** proporciona una extensión de memoria que mejora ligeramente el rendimiento 

Un grupo de buffers en una base de datos ofrece varios beneficios que contribuyen a mejorar el rendimiento, la eficiencia y la capacidad de respuesta del sistema.
## Beneficios del grupo de `buffers`

**Lectura rápida es datos ya leídos** : Cuando la consulta ya sido realizada recientemente , se almacenan los datos o resultados en el `Buffer Pool` si se necesitan nuevamente. Y es mas rápido porque esta accediendo a la memoria Volátil.

**Reducción de trafico en el disco duro:** A lo que me refiero que en vez de consultar al disco duro donde esta la información , consulta en el `Buffer Pool` que es mas eficiente y en general mejora el rendimiento del sistema.

**Mejora de la Concurrencia**: Cuando el servidor es manejado por varios usuarios , se pueden hacer muchas transacciones , consultas  y cambios durante sus acciones `/Concurrencia/` . Todo esto puede ser realizado en los grupos de `Buffers` en vez del almacenamiento físico asi evitando mucho costo de velocidad.

## Consulta para ver el `buffer`

```
SELECT 
COUNT(*) AS pagina_buffers,
DB_NAME(database_id) as nombre_base_datos
from sys.dm_os_buffer_descriptors
group by database_id
```

## Limpieza del `buffer`
```
DBCC DROPCLEANBUFFERS;
```

# Control de concurrencia

## DEFINICION 
Es un aspecto que hace que el permita que muchos usuarios puedan hacer transacciones en una misma base de datos simultáneamente.
**Su importancia** es que garantiza la consistencia de la base de datos y que  aísla cada transacción de diferente usuarios asi evitando inconsistencias.

## Transacción

Son Instrucciones o pasos que no pueden ser divididas o partidas porque son unidades atómicas, donde se logra hacer todo o no se logra hacer nada.

**Tipos de transacción**
**Implícito** 
     Especifica cualquier función `INSERT , Update y DELETE` como tipo de transacción

**Explicito**
    Se usa `Commit , ROLL BACK Y BEGIN TRANSACCION` para estas.

## Propiedades

**Consistencia**
	 La garantía de que la base de dato se mantenga valida antes y después de que se haga una transacción

**Durabilidad**
     Hace que si una transacción ya fue confirmada esos cambios permanezcan en la base de datos sin importar lo que pase

**Atomicidad**
     Garantiza de que todo lo que se hace en la transacción se haga todo y si no se hace , vuelve a su estado Normal de antes `Control + Z`

**Aislamiento**
     Ejecuta cada transacción de manera asilada , que no es visible para los demas hasta que se complete la transacción asi que se ejecuta sin tomar en cuenta las demas pero puede ser revertida para mantener la consistencia para evitar conflictos
## Sentencia SQL-T

**`Commit`**
Se coloca al final de la transaccion y esto hace para que se guarden los cambios cuando ya todo este realizado .

**`ROLLBACK`**
Es como un retorno al estado anterior , se usa cuando se detectan errores o incosistencias y se vuelve cuando la base de datos era valida.

**`bEGIN TRANSACTION`**
Marca el inicio de la transaccion

## Niveles de aislamiento

**No confirmado** -. Es un nivel donde las transacciones pueden leer datos por transacciones aun no confirmadas o realizadas . /`Flujo de datos` / 

```
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; GO SELECT * FROM HumanResources.Employee;
```

**Confirmados**-. Es un nivel donde las transacciones leen solo datos por transacciones ya realizadas descartando las no realizadas aun .
```
SET TRANSACTION ISOLATION LEVEL READ COMMITTED; GO SELECT * FROM HumanResources.Employee;
```

**Repetible** -. Garantiza que las transacciones puedan leer los mismos datos varias veces y que estos no cambien entre lecturas.

```
SET TRANSACTION ISOLATION LEVEL READ COMMITTED; GO SELECT * FROM HumanResources.Employee;
```

**Serializable** -. Un nivel mayor de aislamiento que hace que las transacciones se realicen de manera secuencial sin que ocurran ninguna interferencia .
```
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ; GO SELECT * FROM HumanResources.Employee;
```
## Estrategias de Bloqueo 

**Exclusivo** -. No permiten que los datos sean accedidos mientras se esta transacción.

```
BEGIN TRANSACTION; UPDATE Sales.Customer SET TerritoryID=2 WHERE CustomerID = 1; COMMIT TRANSACTION
```

**Compartido** -. Permite que múltiples transacciones accedan a los mismo datos.
```
 BEGIN TRANSACTION; SELECT * FROM Sales.Customer WHERE CustomerID = 1; COMMIT TRANSACTION;
```

**`DEADLOCK`** -. Es cuando ocurre que dos o mas transacciones se bloquean entre si y esperan los resultados de la primera transacción en entrar , causa una parálisis en la base de datos 

```
-- Primera transacción
BEGIN TRANSACTION; 
UPDATE Sales.Customer SET TerritoryID=2 
WHERE CustomerID = 1;
 WAITFOR DELAY '00:00:05'; -- Simular retraso 
 UPDATE HumanResources.Employee SET JobTitle = 'Manager' WHERE NationalIDNumber = 2; COMMIT TRANSACTION;
 
 -- Segunda transacción (Ejecutar en otra sesión simultánea) 
 BEGIN TRANSACTION; 
 UPDATE HumanResources.Employee SET JobTitle = 'Director' 
 WHERE NationalIDNumber = 2; 
 WAITFOR DELAY '00:00:05'; -- Simular retraso 
 UPDATE Sales.Customer SET TerritoryID=2 
 WHERE CustomerID = 1; COMMIT TRANSACTION;
```



# Enlaces



[Cuándo utilizar las tablas temporales de SQL frente a las variables de tabla September 16, 2019 by Aamir Syed](https://www.sqlshack.com/es/cuando-utilizar-las-tablas-temporales-de-sql-frente-a-las-variables-de-tabla/)
